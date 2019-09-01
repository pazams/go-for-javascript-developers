# Types
## (D) Values, Pointers, References

In Javascript there are value types and reference types. Primitives such as `string` and `number` are value types. Objects, including arrays and functions, are reference types.

In Go, there are value types, reference types, and pointers. References types are slices, maps, and channels. All the rest are value types, but have the ability "*to be referenced*" with pointers.
The most practical difference to remember between references and pointers, is that while you can use both to mutate the underlaying value (when it is mutable), with pointers you can also reassign it.

**JS**
```Javascript
var a = {
    message: 'hello'
}

var b = a;

// mutate
b.message = 'goodbye';
console.log(a.message === b.message); // prints 'true'

// reassign
b = {
    message: 'galaxy'
}
console.log(a.message === b.message); // prints 'false'
```

**Go**
```Go
a := struct {
	message string
}{"hello"}

b := &a

// mutate
// note b.message is short for (*b).message
b.message = "goodbye"
fmt.Println(a.message == b.message) // prints "true"

// reassign
*b = struct {
	message string
}{"galaxy"}
fmt.Println(a.message == b.message) // prints "true"
```

## (D) Arrays/Slices
**JS**

A Javascript `Array` is a dynamically sized list that may contain multiple types of elements.
```Javascript
const foo = ['Rick','Morty'];

// Adds to the end of the array.
foo.push('Beth');

// Removes from the end of the array.
element = foo.pop()
```

Javascript also has a handful of "Typed Arrays", e.g. `Uint8Array` is a type that holds 8-bit unsigned integers. These arrays are not of the same type as `Array`. They support different methods, and commonly used when working with binary data.

**Go**

Go has both slice and array types. Javascript's array is actually more similar to Go's slice than to Go's array, simply because Go's slice is a dynamically sized list, while Go's array size is static. Slice and arrays are always defined over a specific element type. Assigning an array to a new variable does copy the internal data (not a deep copy).Assigning a slice to a new variable does NOT copy the internal data of array, as the slice doesn't contain the internal data in the first place.

Every slice is backed by an array. The backing array may be an array that is defined in the code, or otherwise created transparently by the program.
```Go
a := [3]int{1, 2, 3}

// 1. Create a slice by slicing an array
s1 := a[:]

// 2. Create a slice with slice literal
s2 := []int{1, 2, 3}

// 3.1 Create a zeroed slice with make()
s3_1 := make([]int, 3)

// 3.2 Same as previous line, except an addition of a cap param.
// The cap gives a slice a defined capacity with it's backing array.
// If the following slice will grow by 2 more elements, another array allocation will NOT be performed.
s3_2 := make([]int, 3, 5)

// 4. Create an "empty" slice by variable decleration
var s4 []int
```

Arrays are more limited. They have a known size at compile-time and it cannot change at run-time.
```Go
// 1. Create an array with array literal
a1 := [3]int{1, 2, 3}

// 2. Create an zeroed array by variable decleration
var a2 [3]int
```

Slicing creates a new slice from an existing array or slice. It uses the `[low:high]` syntax, where `low` is the inclusive lower bound, and `high` is the exclusive higher bound. Omitting `low` defaults to 0. Omitting `high` defaults to array/slice length.
```Go
foo := []string{"Rick", "Morty"}

// Adds to the end of the array.
foo = append(foo, "Beth")
fmt.Println(foo)

// Removes from the end of the array.
n := len(foo) - 1 // index of last element
element := foo[n] // optionally also grab the last elemen
foo = foo[:n]     // remove the last element
```
   
Note: a slice that is first slice from an array references the same memory. However, the slice might later reference a different array that the program creates to accommodate a growth in the slice. This example illustrates this:
```Go
fmt.Println("Hello, playground")
foo := [3]string{"a","b","c"}

bar := foo[:]
bar[0] = "changed"	
fmt.Println(foo, bar) // A change in bar is reflected in foo because bar is backed by foo

bar = append(bar, "d")
bar[0] = "changed again"	
fmt.Println(foo, bar) // A change in bar is no longer reflected in foo because the `append` caused bar to backed by a different array
```

## (D) Dictionaries
**JS**

Javascript has 3 types that are used for dictionaries: `Object`, `Map`, `WeakMap`. Even though `Map` and `WeakMap` are dedicated types for dictionaries, `Object` is still often chosen for dictionaries because historically `Object` was the only option before `Map` and `WeakMap` were added to the language. `Object` keys are limited to just String or Symbol types while `Map` and `WeakMap` support any type for keys. 

There are a few other differences between these types, for instance the keys in `Map` are ordered while keys added to `Object` are not.

**Go**

Go has a simple `Map` type. `Map` types are reference types, like slices. Assigning a map to a new variable does NOT copy the internal data.

Initialise a map:
```Go
// Initialise empty map with make.
m1 := make(map[string]int)

// Initialise with struct literals.
m2 := map[string]int{
  "foo": 42,
  "bar": 99,
}
```

Element access:
```Go
v := m["key"] // If the requested key doesn't exist, we get the value type's zero value.
v, ok := m["key"] // Behaves the same as the previous line, but addtionaly the ok bool value can be used to test existence.
```

Iteration:
```Go
// The iteration order is not specified and is not guaranteed to be the same from one iteration to the next.
for key, value := range m {
}
```

## (D) Sets
**JS**

Javascript has 3 types that are used for sets: `Object`, `Set`, `WeakSet`. Similarly to the dictionaries case, `Object` is still often chosen for "set" functionality for historical reasons. `Object` keys are limited to just String or Symbol types while `Set` and `WeakSet` support any type for keys. When using `Object`, the values in the key-value pairs are redundant.

**Go**

Go does not have a type for set. It can be though easily accomplished with a `Map`. [Golang's blog](https://blog.golang.org/go-maps-in-action) suggests the convenient use of `bool` as a value type, i.e. `map[T]bool`. However, if memory usage of the redundant values is of concern, then `map[T]struct{}` is a good alternative, as an empty struct allocates 0 memory (note `interface{}` does allocate memory; see this StackOverflow [answer](https://stackoverflow.com/a/22770744) for more details).
