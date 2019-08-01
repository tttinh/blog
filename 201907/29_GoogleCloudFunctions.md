# Google Cloud Functions

[Home](../README.md)

## What are Cloud Functions

Google Cloud Functions is an event-driven serverless compute platform.

- Simplest way to run your code in the cloud
- Automatically scales, highly available and fault tolerant
- No servers to provision, manage, patch or update
- Pay only while your code runs
- Connects and extends cloud services

You write simple, single-purpose functions that are attached to events emitted from your cloud infrastructure and services. An your function is triggered when an event being watched is fired.

![console](img/29_HowItWorks.png?raw=true)

There are two types of Cloud Functions:

- HTTP functions respond to HTTP requests.
- Background functions are triggered by events, like a message being published to Cloud Pub/Sub or a file being uploaded to Cloud Storage.

![console](img/29_UseCases.png?raw=true)

## Events and Triggers

### Events

Events are things that happen within your cloud environment that you might want to take action on. These might be changes to data in a database, files added to a storage system, or a new virtual machine instance being created. Currently, Cloud Functions supports events from the following providers:

- HTTP
- Cloud Storage
- Cloud Pub/Sub
- Cloud Firestore
- Firebase
- Stackdriver Logging

When an event triggers the execution of your Cloud Function, data associated with the event is passed via the function's parameters. The type of event determines the parameters passed to your function. HTTP request events trigger **HTTP functions**, and the other event types trigger **background functions**.

### Triggers

Creating a response to an event is done with a trigger. A trigger is a declaration that you are interested in a certain event or set of events. Binding a function to a trigger allows you to capture and act on events.

![console](img/29_TriggerTypes.png?raw=true)

Binding of triggers to functions happens at deployment time either via the gcloud command-line tool, the UI or Cloud Functions API. Functions and triggers are bound to each other on a many-to-one basis. In other words, you cannot bind the same function to more than a single trigger at a time. You can, however, have the same trigger cause multiple functions to execute by simply deploying two different functions with the same trigger.

## Execution Environment

Cloud Functions can be written using JavaScript, Python 3, or Go. They run in a fully-managed, serverless environment where Google handles infrastructure, operating systems, and runtime environments completely on your behalf. Each Cloud Function runs in its own isolated secure execution context, scales automatically, and has a lifecycle independent from other functions.

![console](img/29_Runtimes.png?raw=true)

To allow Google to automatically manage and scale the functions, they must be stateless—one function invocation should not rely on in-memory state set by a previous invocation.

When the request volume exceeds the number of existing instances, Cloud Functions may start multiple new instances to handle requests. This automatic scaling behavior allows Cloud Functions to handle many requests in parallel, each using a different instance of your function.

To handle HTTP, Cloud Functions uses a particular HTTP framework in each runtime:

![console](img/29_HTTPFrameworks.png?raw=true)

The example below shows how to read HTTP requests in Go:

```go
// Package p contains an HTTP Cloud Function.
package p

import (
  "encoding/json"
  "fmt"
  "html"
  "net/http"
)

// HelloWorld prints the JSON encoded "message" field in the body
// of the request or "Hello, World!" if there isn't one.
func HelloWorld(w http.ResponseWriter, r *http.Request) {
  var d struct {
    Message string `json:"message"`
  }
  if err := json.NewDecoder(r.Body).Decode(&d); err != nil {
    fmt.Fprint(w, "Hello World!")
    return
  }
  if d.Message == "" {
    fmt.Fprint(w, "Hello World!")
    return
  }
  fmt.Fprint(w, html.EscapeString(d.Message))
}
```

This example shows a Cloud Function triggered by Cloud Pub/Sub events:

```go
// Package helloworld provides a set of Cloud Functions samples.
package helloworld

import (
        "context"
        "log"
)

// PubSubMessage is the payload of a Pub/Sub event.
type PubSubMessage struct {
        Data []byte `json:"data"`
}

// HelloPubSub consumes a Pub/Sub message.
func HelloPubSub(ctx context.Context, m PubSubMessage) error {
        name := string(m.Data)
        if name == "" {
                name = "World"
        }
        log.Printf("Hello, %s!", name)
        return nil
}
```

### Cold Starts

Starting a new function instance involves loading the runtime and your code. Requests that include function instance startup (cold starts) can be slower than requests hitting existing function instances.

### Function Instance Lifespan

A function instance is typically resilient and reused by subsequent function invocations, unless the number of instances is being scaled down (due to lack of ongoing traffic), or your function crashes. So that when one function execution ends, another function invocation can be handled by the same function instance.

### Function Scopes

A single function invocation results in executing only the body of the function declared as the entry point. The global scope in the function file, which is expected to contain the function definition, is executed on every cold start, but not if the instance has already been initialized.

```javascript
// Global (instance-wide) scope
// This computation runs at instance cold-start
const instanceVar = heavyComputation();

/**
 * HTTP function that declares a variable.
 *
 * @param {Object} req request context.
 * @param {Object} res response context.
 */
exports.scopeDemo = (req, res) => {
  // Per-function scope
  // This computation runs every time this function is called
  const functionVar = lightComputation();

  res.send(`Per instance: ${instanceVar}, per function: ${functionVar}`);
};
```

### Execution Timeline

A function has access to the resources requested (CPU and memory) only for the duration of function execution. Code run outside of the execution period is not guaranteed to execute, and it can be stopped at any time.

```javascript
/**
 * HTTP Cloud Function that may not completely
 * execute due to early HTTP response
 *
 * @param {Object} req Cloud Function request context.
 * @param {Object} res Cloud Function response context.
 */
exports.afterResponse = (req, res) => {
  res.end();

  // This statement may not execute
  console.log('Function complete!');
};
```

### Execution Guarantees

The maximum or minimum number of times your function is going to be invoked on a single event depends on the type of your function:

- HTTP functions are invoked `at most` once.
- Background functions are invoked `at least` once.

To make sure that your function behaves correctly on retried execution attempts, you should make it idempotent by implementing it so that an event results in the desired results (and side effects) even if it is delivered multiple times.

### Errors

The recommended way for a function to signal an error depends on the function type:

- HTTP functions should return appropriate HTTP status codes which denote an error.
- Background functions should log and return an error message.

If an error is returned the recommended way, then the function instance that returned the error is labelled as behaving normally and can serve future requests if need be.

### Timeout

Function execution time is limited by the timeout duration, which you can specify at function deployment time. By default, a function times out after 1 minute, but you can extend this period up to 9 minutes.your function should avoid timeouts using a combination of the following techniques:

- Set a timeout that is higher than your expected function execution time.
- Track the amount of time left during execution and perform cleanup/exit early.

### File System

The only writeable part of the filesystem is the /tmp directory, which you can use to store temporary files in a function instance. This is a local disk mount point known as a "tmpfs" volume in which data written to the volume is stored in memory. Note that it will consume memory resources provisioned for the function.

## References

- [Google Cloud Functions](https://cloud.google.com/functions/docs/concepts)
- [Cloud OnAir: Build Your First Serverless Application With Google Cloud Functions](https://youtu.be/MCngVRyfrKw)
