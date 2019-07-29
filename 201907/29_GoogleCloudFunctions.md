# Google Cloud Functions

[Home](../README.md)

## What are Cloud Functions

Cloud Functions is a serverless execution environment for building and connecting cloud services.

### Events and triggers

You write simple, single-purpose functions that are attached to events emitted from your cloud infrastructure and services. An your function is triggered when an event being watched is fired.

Cloud events are things that happen in your cloud environment. These might be things like changes to data in a database, files added to a storage system, or a new virtual machine instance being created.

A trigger is a declaration that you are interested in a certain event or set of events. Binding a function to a trigger allows you to capture and act on events.

### Use cases

![console](img/gcf_usecases.png?raw=true)

## Execution Environment

Cloud Functions can be written using JavaScript, Python 3, or Go. They run in a fully-managed, serverless environment where Google handles infrastructure, operating systems, and runtime environments completely on your behalf. Each Cloud Function runs in its own isolated secure execution context, scales automatically, and has a lifecycle independent from other functions.

![console](img/gcf_runtimes.png?raw=true)

To allow Google to automatically manage and scale the functions, they must be stateless—one function invocation should not rely on in-memory state set by a previous invocation.

When the request volume exceeds the number of existing instances, Cloud Functions may start multiple new instances to handle requests. This automatic scaling behavior allows Cloud Functions to handle many requests in parallel, each using a different instance of your function.

*Cold starts*: starting a new function instance involves loading the runtime and your code. Requests that include function instance startup (cold starts) can be slower than requests hitting existing function instances.

*Function instance lifespan*: a function instance is typically resilient and reused by subsequent function invocations, unless the number of instances is being scaled down (due to lack of ongoing traffic), or your function crashes. So that when one function execution ends, another function invocation can be handled by the same function instance.

*Function scopes*: a single function invocation results in executing only the body of the function declared as the entry point. The global scope in the function file, which is expected to contain the function definition, is executed on every cold start, but not if the instance has already been initialized.

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

*Execution timeline*: a function has access to the resources requested (CPU and memory) only for the duration of function execution. Code run outside of the execution period is not guaranteed to execute, and it can be stopped at any time.

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

*Execution guarantees*: the maximum or minimum number of times your function is going to be invoked on a single event depends on the type of your function:

- HTTP functions are invoked `at most` once.
- Background functions are invoked `at least` once.

To make sure that your function behaves correctly on retried execution attempts, you should make it idempotent by implementing it so that an event results in the desired results (and side effects) even if it is delivered multiple times.

*Errors*: the recommended way for a function to signal an error depends on the function type:

- HTTP functions should return appropriate HTTP status codes which denote an error.
- Background functions should log and return an error message.

If an error is returned the recommended way, then the function instance that returned the error is labelled as behaving normally and can serve future requests if need be.

*Timeout*:
