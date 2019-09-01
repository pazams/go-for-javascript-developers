# Error Handling
## (B) Flow control and values

Both languages pass errors as regular values. Also, both languages leverage flow control constructs: Javascript uses `throw` `catch` `finally` block, and Go uses [`panic` `recover` `defer` ](https://blog.golang.org/defer-panic-and-recover)

## (D) Usage
Despite the similarity claimed above, the languages differ on how and when errors are handled:

**JS**

In JS, the way to propagate an error is determined by the synchronous or asynchronous nature of the function.
If a function is synchronous, then it should use `throw` when an error occurs, and the caller should use `try/catch` blocks.

Otherwise, an asynchronous function should propagate the error by passing it as a first value to a callback function, or it should return a rejected promise.

Note the `async/await` mechanism, which is in draft, will consolidate both worlds by having asynchronous errors being handled inside `try/catch` blocks.

**Go**

In Go on the other hand, the way to propagate an error is determined by the degree of severity with context of the whole application.

For example, for a web-server application, if errors occur in a request handling code path, they should not crash the entire server.  Therefore, these errors should be returned as a last argument to the caller.

On the other hand, if an error occurs during the application init, it can be argued that there's no reason to continue, and therefore `panic` would make sense.

## (S) Loss of stack trace
While passing errors as values, one drawback is the loss of stack trace. Both languages suffer from this. Some runtimes and libraries try to help. Some libraries:
- JS: [longjohn](https://github.com/mattinsler/longjohn)
- Go: [errgo](https://github.com/juju/errgo)

## (D) Future of error handling

Go error handling is the topic of a heated debate. Criticisers of the Go language point to the frequent error "nil handling" code. It is common to see this kind of code block throughout Go code:
```Go
if err != nil {
  return nil, err
}
```
Some went as far as creating memes suggesting the above code block should be made into a keyboard button because it is typed so much.

Criticism came also from Go users. This 2018 Some [user-servy](https://blog.golang.org/survey2018-results) shows that 5% of responders marked error-handling in "What is  the biggest challenge you personally face using Go today?". 

The go team has responded, and in [Go 2.0](https://blog.golang.org/go2-next-steps) has error-handling improvement as one of its goals. The path for error-handling improvement proved to be rough. The `try` proposal was rejected following a public discussion that showed a lot of the community wasn't happy with the proposed addition to the language. For more details about the saga, see this [summary](https://www.infoq.com/news/2019/07/go-try-proposal-rejected/).

We will need to wait for the future to see what it holds for Go error-handling.
