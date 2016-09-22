**-- WORK IN PROGRESS --**

![book cover](/images/cover.png)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Introduction](#introduction)
  - [Preface](#preface)
  - [Which language should I use?](#which-language-should-i-use)
  - [Semantics](#semantics)
  - [Contributions](#contributions)
- [Internals](#internals)
  - [(S) Heap/Stack Memory Allocation](#s-heapstack-memory-allocation)
  - [(S) Garbage Collection](#s-garbage-collection)
  - [(D) Compilation](#d-compilation)
- [Variables](#variables)
  - [(D) Values, Pointers, References](#d-values-pointers-references)
  - [Types](#types)
    - [Dynamic VS Static](#dynamic-vs-static)
- [Flow control statements](#flow-control-statements)
  - [(B) Loops and iteration](#b-loops-and-iteration)
    - [For](#for)
    - [While](#while)
  - [(B) If/Else](#b-ifelse)
  - [(D) Switch](#d-switch)
  - [(D) Defer](#d-defer)
- [Functions](#functions)
- [Package Management](#package-management)
- [Patterns](#patterns)
- [Quick Syntax Reminders](#quick-syntax-reminders)
  - [(D) Strings](#d-strings)
  - [(S) Comments](#s-comments)
- [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Introduction

## Preface

Often, a developer will use more than one programming language at a certain timeframe. Switching back and forth between languages can come with some overhead. These context switches can also result in bugs. For instance, if you switch back and forth between Python and Javascript, there's a likelihood you'll mistake evaluation of an empty array between truthy and falsey. Similarly, if you switch back and forth between Go and Javascript, there's a likelihood you'll make a mistake `switch` statements default behavior of break/fallthrough. Outlining the differences between languages can help mitigate these potential issues, and make it easier to transition back and forth.

This book compares between two programming languages, Golang (or "Go") and ECMAScript (or "Javascript" / "JS"). The merits of this pairing is the popularity of these languages. That's it. They are not similar, in fact, they are quite different. Javascript is an event driven, dynamically typed and interpreted language, while Go is a statically typed and compiled language.

If you're reading this there's a high chance you already know your Javascript and are just starting with Go. If so, make sure you first complete [A tour of go]()

## Which language should I use?

You should always pick the right tool for the right job. Unfortunately, there will never be a simple formula that will instruct you which programming language you should choose to complete a given task.

![science is more art than science](/images/science_art.png)

Aside of technical considerations, other considerations, such as community adoption are also important. It was reported that Facebook moved away from the Erlang language because [it was hard to find qualified programmers](http://highscalability.com/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html)

Having said that, it is worthy to note that Javascript excels in I/O intense applications, and less so in CPU intense applications.  

## Semantics
Each subchapter/subject is denoted with (D),(S) or (B) to indicate how it compares across both languages with 'mostly **D**ifferent', 'mostly **S**imilar' or 'mostly **B**oth'.

This book uses ECMAScript 2015 (ES6).
Also, some subjects will note the run-time environment "NodeJS".

## Contributions
This document is a work in progress. Contributions and PRs are most welcomed.
If you edit the chapters layout, be sure to rebuild the table of contents by
```bash
npm install
npm run toc
```
If you edit go code, be sure to format it with (requires [mdgofmt-cli](https://github.com/pazams/mdgofmt-cli))
```bash
npm run fmt
```
Or just run both commands with
```bash
npm run build
```

# Internals
## (S) Heap/Stack Memory Allocation
Concepts such "Heap" and "Stack" are abstracted away in both languages. You do not needed to worry about it. Even though GO has pointers, it uses [escape analysis](http://blog.rocana.com/golang-escape-analysis) in compile-time to figure out the memory allocation.
## (S) Garbage Collection
Garbage collection is implemented in both languages.
## (D) Compilation 
Go is compiled. Javascript is not, though some Javascript runtimes use JIT compilation. From the developer experience perspective, the biggest effect of compiled languages is compile-time safety. You get compile-time safety with Go, while in Javascript you can use external code linters to ease the missing of this feature.

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

## Types
### Dynamic VS Static

# Flow control statements 
## (B) Loops and iteration
### For
**JS**
```Javascript
for(let i=0;i<10;i++){
    console.log(i);
}
```

**Go**
```Go
for i := 0; i < 10; i++ {
	fmt.Println(i)
}
```

### While
In Go, the `for`'s init and post statement are optional, effectively making it also a "while" statement:

**JS**
```Javascript
var i=0;
while(i<10){
    console.log(i);
    i++;
}
```

**Go**
```Go
i := 0
for i < 10 {
	fmt.Println(i)
	i++
}
```
###Iterating over an Array/Slice

**JS**
```Javascript
['Rick','Morty','Beth','Summer','Jerry'].forEach(function(value,index){
    console.log(value + ' at index ' + index);
});
```

**Go**
```Go
for index, value := range []string{"Rick", "Morty", "Beth", "Summer", "Jerry"} {
	fmt.Printf("%v at index %d", value, index)
}
```


## (B) If/Else 
Go's `if` can contain an init statement, with variables declared scoped only to the `if` and `else` blocks.

**Go**
```Go
if value := getSomeValue(); value < limit {
	return value
} else {
	return value / 2
}
```

## (D) Switch 
The switch statement was one of the motivation for writing this book. Go default behavior inside switch statement is break, while in Javascript it is the other way around

**JS**
```Javascript
switch (favorite) {
    case "yellow":
        console.log("you like the color yellow");
        break;
    case "red":
        console.log("you like the color red");
    case "pruple":
        console.log("you like the color purple");
    default:
        console.log("you like the color white");
} 

```

**Go**
```Go
switch favorite {
case "yellow":
	fmt.Println("you like the color yellow")
case "red":
	fmt.Println("you like the color red")
	fallthrough
case "pruple":
	fmt.Println("you like the color purple")
default:
	fmt.Println("you like the color white")
}
```


## (D) Defer

# Functions

# Package Management

# Patterns

# Quick Syntax Reminders
## (D) Strings
Javascript strings are initialized with single quotes (`'hello'`) or double quotes (`"hello"`). Most Javascripts coding styles prefer the single quotes variation.

Go strings are initialized only with double quotes (`"hello"`).

## (S) Comments
Both languages use the same `/* block comments */`  and `// line comments`.

# License

Copyright Maor Zamski & Daniel Singer

"Go for Javascript Developers" is released under the [Creative Commons Attribution-ShareAlike 4.0 International License.](http://creativecommons.org/licenses/by-sa/4.0/).

The "gopher" used at the cover was [created](https://github.com/golang-samples/gopher-vector) by [Takuya Ueda](https://twitter.com/tenntenn). It is licensed under the [Creative Commons 3.0 Attributions license](https://creativecommons.org/licenses/by/3.0/deed).
