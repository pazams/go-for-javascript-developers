![book cover](/images/cover.png)

# Introduction

Often, a developer will use more than one programming language at a certain timeframe. Switching back and forth between languages can come with some overhead. These context switches can also result in bugs. For instance, if you switch back and forth between Python and Javascript, there's a likelihood you'll mistake evaluation of an empty array between truthy and falsey. Similarly, if you switch back and forth between Go and Javascript, there's a likelihood you'll mistake `switch` statements default behavior of break/fallthrough. Outlining the differences between languages can help mitigate these potential issues, and make it easier to transition back and forth.

This document compares between two programming languages, Golang (or "Go") and ECMAScript (or "Javascript" / "JS"). The merits of this pairing is the popularity of these languages. That's it. They are not similar, in fact, they are quite different. Javascript is an event driven, dynamically typed and interpreted language, while Go is a statically typed and compiled language.

If you're reading this there's a high chance you already know your Javascript and are just starting with Go. If so, make sure you first complete [A tour of go](https://tour.golang.org) and [Effective go](https://golang.org/doc/effective_go.html).

## Which language should I use?

You should always pick the right tool for the right job. Unfortunately, there will never be a simple formula that will instruct you which programming language you should choose to complete a given task.

![science is more art than science](/images/science_art.png)

Aside of technical considerations, other considerations, such as community adoption are also important. It was [reported](http://highscalability.com/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html) that Facebook moved away from the Erlang language because it was hard to find qualified programmers.

Having said that, it is worthy to note that Javascript excels in I/O intense applications, and less so in CPU intense applications.

## Semantics
Each subchapter/subject is denoted with (D),(S) or (B) to indicate how it compares across both languages with 'mostly **D**ifferent', 'mostly **S**imilar' or 'mostly **B**oth'.

This document uses ECMAScript 2015 (ES6).
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
