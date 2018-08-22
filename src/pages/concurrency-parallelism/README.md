# Concurrency & Parallelism

## (D) Overview

**JS**

The best way to describe Parallelism in Javascript is with this [quote](http://debuggable.com/posts/understanding-node-js:4bd98440-45e4-4a9a-8ef7-0f7ecbdd56cb) by Felix Geisendörfer:
>  Well, in node everything runs in parallel, except your code.

So while your Javascript runtime may use multiple threads for IO, your own code is getting run just by one. That's just how the *evented* model works.
Different Javascript runtimes offer some options for concurrency or parallelism: NodeJS offers [clustering](https://nodejs.org/docs/latest/api/cluster.html), and Browsers offer [web workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers).

**Go**

On the other hand, Go is all about a concurrency which enables parallelism. It offers Goroutines which enables functions to run concurrently, and channels to communicate between them. While Go standard library has the "sync" package for synchronization primitives, it [encourages](https://blog.golang.org/share-memory-by-communicating) more the use of Goroutines and channels, summarized as:

> Do not communicate by sharing memory; instead, share memory by communicating

More on this subject:
- [Go Concurrency Patterns](https://talks.golang.org/2012/concurrency.slide#1)
- [Advanced Go Concurrency Patterns](https://talks.golang.org/2013/advconc.slide#1)


## (D) Async vs Sync APIs
**JS**

JS promotes writing async APIs, since sync APIs always block the caller, e.g:
```Javascript
const fs = require('fs');

// The caller to this function will be blocked while the file is being read.
function fetchA() {
   return fs.readFileSync('a.txt');
}
```
In the example above, the async `fs.readFile()` would be a better choice in most scenarios, making `fetchA()` async and unblocking its caller.

**Go**

On the other hand, Go promotes the sync APIs (see “Avoid concurrency in your API” in https://talks.golang.org/2013/bestpractices.slide#26)
The reasoning behind this is that it is completely up to the caller's choice to be blocked or not by a sync API. Consider the following type definition and sync `fetchA` function:
```Go
type fetchResult struct {
	Message string
	Error   error
}

func fetchA() fetchResult {
	time.Sleep(time.Second * 4)
	return fetchResult{"A data", nil}
}
```
If the caller wants to be blocked, then he can just call the function
```Go
	a := fetchA()
```
If the caller does not want to be blocked, then he could call the function inside a goroutine:
```Go
	aChan := make(chan fetchResult, 0)
	go func(c chan fetchResult) {
		c <- fetchA()
	}(aChan)
```

## (D) Sequential and Concurrent Patterns

**JS**

Even without parallelism, we can structure Javascript code in both sequential and concurrent flows.
For the following exmaples, let’s assume `fetchA()`, `fetchB()` and `fetchC()` are all async functions returning a promise.

**Sequential**

```Javascript
function fetchSequential() {
    fetchA().then(a => {
        console.log(a);
        return fetchB();
    }).then(b => {
        console.log(b);
        return fetchC();
    }).then(c => {
        console.log(c);
    })
}
```

or

```Javascript
async function fetchSequential() {
    const a = await fetchA();
    console.log(a);
    const b = await fetchB();
    console.log(b);
    const c = await fetchC();
    console.log(c);
}
```

**Concurrent**

```Javascript
function fetchConcurrent() {
    Promise.all([fetchA(), fetchB(), fetchC()]).then(values => {
        console.log(values);
    }
}
```

or

```Javascript
async function fetchConcurrent() {
    const values = await Promise.all([fetchA(), fetchB(), fetchC()])
    console.log(values);
}
```

**Go**

For the following examples, assume `fetchB()` and `fetchC()` are defined as a sync function similarly to `fetchA` in the previous section (The full example is available here https://play.golang.org/p/2BVwtos4-j)

**Sequential**

```Go
func fetchSequential() {
	a := fetchA()
	fmt.Println(a)
	b := fetchB()
	fmt.Println(b)
	c := fetchC()
	fmt.Println(c)
}
```
**Concurrent**

```Go
func fetchConcurrent() {
	aChan := make(chan fetchResult, 0)
	go func(c chan fetchResult) {
		c <- fetchA()
	}(aChan)
	bChan := make(chan fetchResult, 0)
	go func(c chan fetchResult) {
		c <- fetchB()
	}(bChan)
	cChan := make(chan fetchResult, 0)
	go func(c chan fetchResult) {
		c <- fetchC()
	}(cChan)

	// order doesn't really matter!
	a := <-aChan
	b := <-bChan
	c := <-cChan
	fmt.Println(a)
	fmt.Println(b)
	fmt.Println(c)
}
```

or

```Go

func fetchConcurrent() {
	aChan := make(chan fetchResult, 0)
	bChan := make(chan fetchResult, 0)
	cChan := make(chan fetchResult, 0)

	go func(c chan fetchResult) {
		c <- fetchA()
	}(aChan)
	go func(c chan fetchResult) {
		c <- fetchB()
	}(bChan)
	go func(c chan fetchResult) {
		c <- fetchC()
	}(cChan)

	for i := 0; i < 3; i++ {
		select {
		case a := <-aChan:
			fmt.Println(a)
		case b := <-bChan:
			fmt.Println(b)
		case c := <-cChan:
			fmt.Println(c)

		}
	}
}

```
