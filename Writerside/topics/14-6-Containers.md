# 14.6. Containers

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Containers

The S++ STL contains a number of container types, including implementations for arrays, vectors, maps, sets, queues,
stacks and others. The majority of these containers are defined dynamically, separating their external API from the
internal implementation, allowing different "backends" to be used. This is also seen in the string implementation, where
the backend generic type is used to define the string type, such as rope string or vector string.

### Array

The most basic type of container is the `Arr[T]` type, analogous to a `T[]` type in C++. The `Arr[T]` guarantees that
the elements are stored contiguously in memory, and that the array is resizable. Elements in the array will be stored
directly next to each other, such that the memory location of the nth element will always be
`memory location of the array + (n - 1) * the size of T`.

### Vector

The `Vec[T]` type is a dynamic array, which is resizable and has a number of utility methods for adding, removing and
