![Go](https://github.com/massahud/goawait/workflows/Go/badge.svg)

GoAwait
=======

Package goawait is a simple module for asynchronous waiting. Use goawait when
you need to wait for asynchronous tasks to complete before continuing normal
execution.

GoAwait has functions that take a polling function and execute
that function until it succeeds or the specified timeout is exceeded.

Example with polling function that returns an error:
```go
	ctx, cancel := context.WithTimeout(context.Background(), time.Millisecond)
	defer cancel()
	poll := func(ctx context.Context) error {
			return errors.New("error message")
	}
	if err != goawait.Poll(ctx, 500*time.Microsecond, poll); err != nil {
		return err
	}
```

Example simultaneously polling multiple functions:
```
	ctx, cancel := context.WithTimeout(context.Background(), time.Millisecond)
	defer cancel()
	poll1 := func(ctx context.Context) error {
			return nil
	}
	poll2 := func(ctx context.Context) error {
			return errors.New("error message")
	}
	polls := map[string]goawait.PollFunc{"poll1": poll1, "poll2": poll2}
	if err != goawait.PollAll(ctx, 500*time.Microsecond, polls); err != nil {
		return err
	}
```

Example getting the first success result of multiple functions:
```
	ctx, cancel := context.WithTimeout(context.Background(), time.Millisecond)
	defer cancel()
	faster := func(ctx context.Context) (interface{}, error) {
        time.Sleep(time.Microsecond)
        return "I'm fast", nil
    }
    slower := func(ctx context.Context) (interface{}, error) {
        time.Sleep(time.Millisecond)
        return "I'm slow", nil
    }
    polls := map[string]goawait.PollResultFunc{"faster": faster, "slower": slower}
    firstResult, err := goawait.PollFirstResult(context.Background(), retryInterval, polls)
	if err != nil {
		return err
	}
    fmt.Printlf("function %s returned first, with result: %v\n", firstResult.Name, firstResult.Result) 
```

[GoDoc](https://pkg.go.dev/github.com/massahud/goawait?tab=doc)
