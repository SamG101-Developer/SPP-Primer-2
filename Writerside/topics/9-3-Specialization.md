# 9.3. Specialization

S++ supports specialization of generic types and functions. This allows for the creation of optimized versions of
generic functions for specific types, which can be used to improve performance in certain situations. There are two
types of specialization in S++: function specialization and superimposition specialization.

## Function Specialization

```
cls A { }
cls B { }

sup B {
    fun check() -> Bool { return true }
}


fun check[T: A]() -> Bool {
    return false
}

fun check[T: B]() -> Bool {
    return B::check()
}
```
