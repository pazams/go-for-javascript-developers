# Keywords & Syntax Comparison

## (D) `this` keyword
**JS**

Inside an object method, `this` refers to the object (with some exceptions).

**Go**

In Go, the closest analogy would be receivers inside method functions.
You *may* use `this` as a receiver:
```Go
type Bar struct {
	foo string
}

func (this *Bar) Foo() string {
	return this.foo
}
```
It is more idiomatic to use short variables as receivers. In the example above `b` would have been a better fit over `this`.

## (D) `new` keyword
**JS**

`new Foo()` instantiates an object from `Foo`, a constructor function or a class.

**Go**

`new(T)` allocates zeroed storage for a new item of type `T` and returns a pointer, `*T`. This is different than Javascript and most other languages where `new` will **initialize** the object, while in Golang it only **zeros** it.

It is worthy to mention that it is [idiomatic](https://blog.golang.org/package-names) to name methods with a "New" prefix to denote it returns a pointer to the type following in the method name. e.g:
```Go
timer := time.NewTimer(d) // timer is a *time.Timer
```

## (D) bind / method values

**JS**
```Javascript
var f = bar.foo.bind(bar2); // when calling f(), "this" will refer to bar2
```

**Go**
```Go
f := bar.foo // f(), is same as bar.foo()
```

## (S) setTimeout / timer

**JS**
```Javascript
setTimeout(somefunction, 3*1000)
```

**Go**
```Go
time.AfterFunc(3*time.Second, somefunction)
```

## (D) setInterval / ticker

**JS**
```Javascript
setInterval(somefunction, 3*1000)
```

**Go**
```Go
ticker := time.NewTicker(3 * time.Second)
go func() {
	for t := range ticker.C {
		somefunction()
	}
}()
```

## (D) String literals
**JS**

Strings are initialized with single quotes (`'hello'`) or double quotes (`"hello"`), yet most coding styles prefer the single quotes variation. Raw string literals use backticks (``` `hello` ```).

**Go**

Strings are initialized with double quotes (`"hello"`) or raw string literals with backticks (``` `hello` ```)

## (S) Comments
Both languages use the same `/* block comments */`  and `// line comments`.
