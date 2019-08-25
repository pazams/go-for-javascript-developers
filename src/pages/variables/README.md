# Variables
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

## (D) Types

### Dictionaries
**JS**
Javascript has 3 types that are used for dictionaries: `Object`, `Map`, `WeakMap`. Even though `Map` and `WeakMap` are dedicated types for dictionaries, `Object` is still often chosen for dictionaries because historically `Object` was the only option before `Map` and `WeakMap` were added to the language. `Object` keys are limited to just String or Symbol types while `Map` and `WeakMap` support any type for keys. 

There are a few other differences between these types, for instance the keys in `Map` are ordered while keys added to `Object` are not.

**Go**
Go has a simple `Map` type. `Map` types are reference types, like slices. Assigning a map to a new variable does not copy the internal data.

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
v, ok := m["key"] // Behaves the same as above, but addtionaly the ok bool value can be used to test existence.
```

Iteration:
```Go
// the iteration order is not specified and is not guaranteed to be the same from one iteration to the next
for key, value := range m {
}
```
