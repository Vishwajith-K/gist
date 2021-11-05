# Piece 1

```C++
void fun() {
    ...
    fun1(A::VAL1); // some value from A namespace/class is used/accessed
    ...
}

class A {
    ...
    public:
      enum {VAL1, VAL2} en_name_t;
    ...
};

// Above code piece can also be which does not require changes in other parts of the code
// except in class definition
class A {
    ...
    typedef unsigned short en_name_t;
    static en_name_t VAL1 = 0, VAL2 = 1;
    ...
};
```
