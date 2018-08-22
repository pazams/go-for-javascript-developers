# Internals
## (S) Heap/Stack Memory Allocation
Concepts such "Heap" and "Stack" are abstracted away in both languages. You do not needed to worry about it. Even though GO has pointers, it uses [escape analysis](http://blog.rocana.com/golang-escape-analysis) in compile-time to figure out the memory allocation.
## (S) Garbage Collection
Garbage collection is implemented in both languages.
## (D) Compilation
Go is compiled. Javascript is not, though some Javascript runtimes use JIT compilation. From the developer experience perspective, the biggest effect of compiled languages is compile-time safety. You get compile-time safety with Go, while in Javascript you can use external code linters to ease the missing of this feature.
