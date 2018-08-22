# Functions
## (S) first-class functions
Both languages treat functions as first-class citizens. Both allow functions to be passed as arguments, to be a returned value, to be nested, and have closures.

Function nesting in Javascript can be done both with named and anonymous functions, while in Go this can only be done with anonymous functions.

## (D) Multiple returns
Go functions can return multiple values

**Go**
```Go
func hello() (string, string) {
	return "hello", "world"
}

func main() {
	a, b := hello()
	fmt.Println(a, b)
}
```
Javascript cannot, however by using destructuring assignment syntax, we can get a similar behavior

**JS**
```Javascript
function hello() {
  return ["hello", "world"];
}

var [a, b] = hello();
console.log(a,b);
```

## (S) IIFE

**JS**
```Javascript
(function () {
  console.log('hello');
}());
```

**Go**
```Go
func main() {
	func() {
		fmt.Println("Hello")
	}()
}
```

## (S) Closures
Both languages have closures. Both require caution when [creating closures inside loops](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures#Creating_closures_in_loops_A_common_mistake). Here are examples in both languages that demonstrate a similar technique to bypass the closure/loop trap:

**JS (with bug)**
```Javascript
var i = 0;
for (;  i < 10 ; i++) {
  setTimeout((function() {console.log(i);}),0);
}
```

**JS (solved)** (note that using `for(let i=0; …` instead of `var` is a more practical solution)
```Javascript
var i = 0;
for (;  i < 10 ; i++) {
  (function (i) {
    setTimeout((function() {console.log(i);}),0);
  }(i));
}
```

**Go (with bug)**
```Go
var wg sync.WaitGroup
wg.Add(10)
for i := 0; i < 10; i++ {
	go func() {
		defer wg.Done()
		fmt.Println(i)
	}()
}
wg.Wait()
```

**Go (solved)**
```Go
var wg sync.WaitGroup
wg.Add(10)
for i := 0; i < 10; i++ {
	go func(i int) {
		defer wg.Done()
		fmt.Println(i)
	}(i)
}
wg.Wait()
```
