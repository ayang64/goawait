![Go](https://github.com/massahud/goawait/workflows/Go/badge.svg)

GoAwait
=======

GoAwait is a simple module for asynchronous waiting.

Use goawait when you need to wait for asynchronous tasks to complete before continuing normal execution. 
It is very useful for waiting on integration and end to end tests.

## Documentation

[GoDoc](https://pkg.go.dev/github.com/massahud/goawait?tab=doc)

### Polling

GoAwait has polling functions that polls for something until it happens, or until the context is canceled.

```go
goawait.UntilNoError(cancelCtx, 500 * time.Millisecond, connectToDatabase)

goawait.Untiltrue(cancelCtx, 500 * time.Millisecond, messageReceived)
```

The polling functions are based on [Bill Kennedy's **retryTimeout** concurrency example](https://github.com/ardanlabs/gotraining/blob/master/topics/go/concurrency/channels/example1/example1.go)

### Await

Await is a type that allows applications to have a common wait specification. This way they can
reuse the same GoAwait configuration in different awaits. 

```go
defaultAwait := goawait.NewAwait(appContext, 10 * time.Second, 500 * time.Millisecond)

defaultAwait.UntilNoError(connectToServer)

defaultAwait.Untiltrue(pageHasElement)
```