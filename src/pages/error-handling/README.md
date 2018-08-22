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
