# gist
Share your mind, use **Issues** or **Pull requests** section of this repository

-----

-----

Can a `struct` member be `const` qualified?

Ans - Yes; may be because const is not a storage class, so complete struct will be bundled and only corresponding member is marked as read only/const (const isn't actually read-only; const and volatile are used to signal their meaning to compiler)

-----

Can the data at a location be modified; location is pointed to by a `struct` member, And the function which is trying to do the modification operation has the `struct`'s pointer? viz, `fn(const struct *this) {*(this->that) = 10;}`?

Ans - Yes

-----

And are braces necessary in `*(this->that)` case?

Ans - No; Arrow has highest precedence over usual de-reference operator

-----

Can a `struct` member be `static`, so that only one copy of variable exists across the `struct` objects?

Ans - Nope; `struct`s are like bundled and stored in a single location (this is also how classes(cpp), structs are differentiated) and declaring a `static` variable means that, the variable needs to be in `.BSS` section. A `struct` object can be `static` and the FAQ has nothing to with this statement - provided to clear a confusion.

-----

`extern` are initialized?

And - Yes and to zero. Give it a thought, if `extern` is outside a function then it's trying to refer a global var in another file/translation unit OR if extern is inside a function then it's declared somewhere outside the function (below the function body) => global. In any of the case, `extern` are zeroed out.

-----

Is/are multi line string supported? Does C support multi-line strings? Can a string span across lines in C?

Ans - Yes, use \ (forward slash) symbol to say that it's a multi-line string

-----

Strigizer operator? How to convert a variable name or a something into a string?

Ans - `#define str(a) (#a)`. Unlike other tools in C, it can only exist in a `#define` construct. And yeah, it's replaced with corresponding string while pre-processing the code.

-----

Is it possible to merge two variables or is it possible to merge a number & a variable?

Ans - Yes. It's termed as Concatenate operator or also as Token-pasting operator. `#define concat(a, b) a##b`

-----

How can I assign value only to a particular member of the `struct` object?

Ans - Designated assignment. `struct std a = {.roll = 10,};` comma is optional. Some compilers have added another symbol in C grammar: `...{roll: 10};` In any case both refer to designated assignment concept.

-----

Is it possible to initialise an element in array while declaration?

Ans - Yes & Designated assignment is used. Rest of the elements in the array will be zeroed. `int a[] = {[10] = 7};`

-----

Logical right shift operator performs logical OR arithmetic shift?

Ans - Arithmetic shift will hold MSB bit and also shift but logical shift pads zeros MSBs and shifts. Right shift operator does a signed right shift, so it's actually arithmetic shift operator.

-----

When will a macro in another macro gets replaced? Will nested macro be first substituted OR parent macro?

Ans - Parent are replaced first then a macros in replaced text are replaced and this continues.

-----

Can I send a `struct` object to a function? OR Can I assign one `struct` object to another object to copy values?

Ans - Yes. You can send a struct object to a function OR even copy contents of one struct object to another struct object. It meets your expectations, **even if the `struct` has an array**.

-----

How're `static` locals differentiated whose name is same?

Ans - It's the job of compiler to handle locals, globals or anything related to C. In the end, it's upto the implementation; C is a standard not a tool provided by an organization - describes behaviors only not how they're implemented (correct me if am wrong).

-----

Can we do pointer arithmetic to function pointers?

Ans - Yes; Pointer arithmetic is usually addition or subtraction of a constant or 2 pointers. Adding an integer 1 to an integer pointer results in +4 of the older value as `sizeof(int)` is 4. So in our case `sizeof` any function pointer is 1B. 'Any' here refers to any return type, any number of args or of any type of args. So adding an integer 1 to a function pointer results in +1 value.

-----

Can we jump to 10'th line in a function directly, using function pointers and pointer arithmetic?

Ans - behavior is bit unknown as of now. But SEGFAULT chances are high!

-----

Inner most `static` variables retain their value?

Ans - Yes.

-----

Can a `struct` member be `volatile`?

Ans - Yes hukum, for sure. `volatile` and `const` are qualifiers only, they're not storage classes. And it satisfies the need that says - `struct` object must be bundled and stored in a single location (storage).

-----

Can a `static` be `volatile`?

Ans - Yes

-----

Can a `static` be `const`?

Ans - Yes. And `static const` OR `const static` didn't matter when I tried.

-----

Can a `static` be `extern`?

Ans - No. It's not a good question if you know list of storage classes. An identifier(variable/function) can belong to a single storage class. So, you can't put an identifier in more than one storage class.

-----

What's `sizeof` `void` and `*(void &)`?

Ans - 1Byte is the size of void and what's the `sizeof(void *)`? Same as `sizeof` any pointer. Pointer stores address (and can point to same typed locations - unless typecasted). Number of bits used to represent an address will be machine dependent (32b = 4B, 64b = 8B).

-----

Does `goto` has scope?

Ans - Yes. To the function level. Any `goto` in the same function can be handled. You can't jump to `goto` a label in other function. This may help to people who're from assmebly background.

-----

Function name and `goto` labels collides?

Ans - No. Function call is made only if we use parathesis; so compiler knows where to jump: whether to a label or function.

-----

Can we de-reference a function pointer and assign a code to it?

Ans - No. Actually don't do it. It's good to learn grammar, but grammar which we don't use is not at all needed.

-----

Can we initialize an array in the midst of a block? (flower braces and type cast)

Ans - No. Assignment to expression with array type - `a = (int []) {4, 5, 6};` causes compiler error. Error note seems to be pretty explanatory. However similar syntax is used for structured objects.

-----

_GCC only_ - Can we use `typeof` (GCC extension) in other user applications?

Ans - Yes, we can use the extension in any user applications. But give it a thought, what if devops team uses clang? or  Turbo C (😜 ). Now your code isn't portable across compilers.

-----

Preprocessor is most-ly find & replace right, so what if I define a macro whose name is a C keyword?

Ans - Yes. It can be useful in testing something or someone's code. Let's say you asked people to write a `sizeof` simulation code. You don't need to look at code just compile it by defining `sizeof` as nothing (gcc provides an option -D) and there-by you may expect an error if they have used sizeof; as sizeof would get replaced with nothing and if the code says `sizeof(datatype)`, it would be `datatype`.

-----

How can I `break` from all the nested looping?

Ans - A `break` will `break` inner-most loop only. However local flags can used to check and break from every other level. As architectures support pipe-lining it'll be tedious if number of nestings are high and so number of flag checks. Instead, `goto` (unconditional branching) can be used in a point where `break` from all the nesting is needed; which may cost only one control hazard.

-----

Can we assign any value to a global variable? Like, is it possible to assign a function return value to a global variable while it's declared?

Ans - No. Only constants, literals are allowed. I got to know it this way, a function returns some number and I initialize a global variable using that function. Thinkable and good right? No it's actually bad. If this was supposed to happen then why would I need a function called `main`? And some other person who tries to extend my code will repeat similar thing. Now which function must be called first? Depending upon order in which variables are initialized?

-----

Can functions `return` a `const` qualified value?

Ans - I find it useless. My understanding from this (https://stackoverflow.com/questions/3541766/const-return-types-in-c#answer-3541779) is that, a function which is called many times and sometimes with same set of values, then `return` value will be same in the case of 'sometimes'. But I tried it using GCC, it doesn't seem to be working! So, `const` as a `return` type qualifier has no use (at least I haven't found one). Cpp gives a different meaning to this question's answer and it's out of scope for this document.

-----

I get confused when `const` and pointers are used together. How does it work? (am actually asking for a simple method 😉 )

Ans - Yes there's a simple method that I've found, and I feel that, it's actually the reason to distinguish between `const`-pointers stuff. Basic rule is: asterisk in a declaration helps us. Any (`const`) qualifier written after the `*` will refer to the pointer variable itself and anything before that corresponds to the data that it points to. So, `const int *a` => any data pointed to by `a` must not be changed but, `a` itself can point to different location. `int * const a` => yeah exactly, once `a` starts pointing to some location, it can not point to other locations (it's a good guy and acts like alias), but data location pointed to by `a` itself can be modified. Of course compiler will not expect that, you modify something in the case of `const int * const a` (just mentioned if you were waiting for that dopamine). Now apply same concept to double pointers. `int *const *a` => data and second dimension (cell/column) can be modified but not the row (first dimension) that it points to. Here's the table which briefs the above info:

Way  of declaration | Can we change `**p` | Can we change `*p` | Can we change `p`
-|-|-|-
`int   ** p` | Yes | Yes | Yes
`const   int   ** p` | No | Yes | Yes
`int   * const * p` | Yes | No | Yes
`int   ** const p` | Yes | Yes | No
`const   int   * const * p` | No | No | Yes
`const   int   ** const p` | No | Yes | No
`int   * const * const p` | Yes | No | No
`const   int   * const * const p` | No | No | No

-----

What actually happens when I write two string literals separated by space? And what if one among them isn't a literal and built at run-time?

Ans - Two string literals are combined and a single literal replaces them. This doesn't happen in pre-processing stage. A string literal and a character-array can't be merged in this manner. Compiler error would pop-up and helps to fix.

-----

I see some macros with no value being assigned to them. Is there any use in doing so?

Ans - We can call these 'placeholders'. These act as compile-time switches. Unlike the macros with values, these will include/exclude a part of code while pre-processing. Good, but how does it help me? They help us write bit more generic, portable code. And these are also used to guard the header-file which avoids inclusion of same header file multiple times (Search for header-guards in C).

-----

I wanted to know about function pointers. Can you brief me?

Ans - Yes. wkt Pointers are spl variables which can hold address of other variable of same type (except generic pointer; assuming not typecasted). On the same lines, function pointer can point to a function of same type. Okay, in the case of functions type is nothing but signature + `return` type. So to declare or allocate a function pointer we need the information on the function that it can point to. Prototype of function gives us these things along with name of the function. Data pointers declaration follows this sequence - type, asterisk, ptr name. Function pointers declaration follows this sequence - `return` type, asterisk and name of ptr enclosed in parenthesis, function argument type/s enclosed in parenthesis.
`ret_type (*ptr_name)(type1, type2);` Yeah, it's bit weird to have name in between the function-type. Adding a `typedef` at the start of line makes the variables itself as a new type. So, the following statements can declare functions in a normal way. Removing asterisk in a `typedef` statement makes the user-defined name a type, and a asterisk must be used to declare function pointers using this name. Name in between in the type weirdness is followed in all these cases. In Python, functions can be passed as arguments to other functions or stored in an array (lists in python) and I was amazed to see the same thing in C with the function-pointers concept. Callbacks (search for it) are highly usual in a large projects which are built using C lang. As a note, `SEGFAULT`s are a given chance when we do pointer arithmatic on function pointers.
`ptr = &fn_name` OR `ptr = fn_name` are allowed ways. And the ptr can be used as if it were a function, while making a call to the corresponding function: `ptr(args)`.


-----

Can we assign a value to a `struct` object apart from in declaration line?

Ans - Yes. First of all, can we assign a `struct` object to another? Like, will the values be directly copied to LHS object? WKT this happens right; so, even the concept behind this question works the same way. But assigning a flower braced value will help you get a compilation error. Typecasting is a must, so that compiler can evaluate the expression. So one of my learning from this is that, flower braces are not part of an expression (except - initialization) (atleast directly). 

-----

Can we modify a `struct`'s member data assuming we have pointer and `const` prefixes the type?

Ans - Nope. Think of same problem on primary data typed values. We can't modify data right? So, same concept applies here.

-----

Can we modify a value given that, the correpsonding location is pointed by a variable which is wrapped in a s`truct` and `struct`'s type is prefixed with `const`?

Ans - Yes. Here `const` is applied to that pointer and not the data at a location it's pointing to. Say `z` is `struct` pointer (`const` qualifier prefixed), `a` is `int` pointer in the `struct` and a points to a location. This is what the FAQ is, and we can change the data that's pointed by a but not the location a points to.

-----

How to pass multi-dimensional arrays to functions?

Ans - check this html page: https://www.eskimo.com/~scs/cclass/int/sx9a.html

-----

Can a `volatile` member be `const`; so that software ensures read-only operations?

Ans - Yes. This can be handy typically in embedded codes where hardware might control value and software mainly listens the value. Or, may be compiler optimizes `const`s as they are constants thorugh out the code and so this trick might help to (kind of) disable optimizations involving the const variable (from math point of view, constant and variable are opposite - C fused opposite words).

-----

Is there any difference between a function which accepts no arguments and a function whose declaration explicitly mentioned void in the place of arguments?

-----

Does grammer needs flower braces to be nested, for 2D array initialization?
Ans - No (GCC based answer).

-----

Can compile-time arrays be declared with no size in the indices?

Ans - Depends. It's a no, if the declaration statement does not initialize with contents - this rule is for any dimensional array. Any empty flower brace initilization of 1D would make array size to zero (tried in GCC and mention me it's usecase/s) else, product of number of elements in flower braces and size of the array type will be the total size of array in bytes. Max of one size in index can be omitted and that too the first one - number of rows if 2D, number of blocks if 3D and so on; rest are necessary as they tell the array structure in memory.

-----

Can you brief me about pointer to arrays?

Ans - Yes. Usually a normal single pointer acts as if it were an array and even vice versa true upto some extent.

-----

Can we compare (relational) two compile time (static) struct objects?

-----

How can I do typecheck; I mean whether a variable is of some type or not?

Ans - See `typecheck.h` in Linux. Usually used in macro body. As we wouldn't know type of argument that is expanded.

-----

Why can't we use statment expressions `({...})` instead of `do-while(0)`? `typecheck.h` uses first one and many others use `do`-`while`. Is there any difference?

-----

Though `union`s save memory, we can't use it's members all at once; So, why did designers add it in C grammer? What's the use of `union`s?

Ans - One feature is aliasing - you can give more than one name to a memory location. Assume that, an `union` contains an integer; And also the same `union` hosts an array of bits. With this setup, we can set or clear all the bits of a memory location and it may be useful in the embedded world. The above example can pullup all the bits of a port OR pull-down all the bits with just one assignment statement involving the integer member. Another example that I found on Stackoverflow community is - `float`, `double`s are IEEE754 complaint (most-ly). So, bits in each field of `float` can extracted using unions in easy manner without using deep bit manipulation operators.

-----

Why can't we use `#define` instead of `typedef`? First of all what's the difference between them?

Ans: `typedef` follows scope guidelines but `#define`s won't. `typedef` can ease up function pointer declarations. Multiple pointers in single line, can't be declared if the `#define`s are used. Credits: https://stackoverflow.com/questions/1666353/are-typedef-and-define-the-same-in-c

-----

Is it possible to compare two macros or compile-time switches or symbolic constants which has no assigned value to them?

Ans - Yes, it is possible.

-----

List of format specifiers in C:

Format Specifier | Data Type
-|-
`%c` | Character
`%d` | Signed integer
`%e` _or_ `%E` | Scientific notation of floats
`%f` | Float values
`%g` _or_ `%G` | Similar as %e or %E
`%hi` | Signed integer (short)
`%hu` | Unsigned Integer (short)
`%i` | Unsigned integer
`%l` _or_ `%ld` or `%li` | Long
`%lf` | Double
`%Lf` | Long double
`%lu` | Unsigned int or unsigned long
`%lli` _or_ `%lld` | Long long
`%llu` | Unsigned long long
`%o` | Octal representation
`%p` | Pointer
`%s` | String
`%u` | Unsigned int
`%x` _or_ `%X` | Hexadecimal representation (small or caps)
`%n` | Prints nothing
`%%` | Prints % character
_`%10d`_ | _10 right aligned_
_`%-10d`_ | _10 left aligned_
_`%010d`_ | _prepend zeroes and right align_
_`%.2f`_ | _2 decimal positions resolution after floating point_
_`%010.2f`_ | _prepend zeros, right align for 10 characters, set resolution to 2 decimal places_
_`%.2s`_ | _first 2 characters of the string only_
_`%0s`_ | _warning_
_`%-010d`_ | _warning_
_`%+010d`_ | _right align for 10 chars, prepend zeros and mandatory sign (+/-)_
_`%*d`_ | _first argument sets alignment and second argument will be printed_
_`%*.*f`_ | _Needs 3 args - alignment, precision, number itself_
