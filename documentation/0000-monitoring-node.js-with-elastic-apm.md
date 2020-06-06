- **Start Date:** 2018-08-18
- **PR:** (leave this empty)
- **Issue:** [#0000](link-to-issue) (remove if no associated issue)
- **Keywords:** apm, performance, monitoring
- **Summary:** How to monitor a Node.js server with Elastic APM

# Monitoring Node.js with Elastic APM

Getting Elastic APM set up for your Node.js app is easy,
and there are various ways you can tweak it to fit your needs.

## Installation

Add the `elastic-apm-node` module as a dependency to your application:

```sh
npm install elastic-apm-node --save
```

## Initialization

It's important that the agent is started before you require *any* other modules in your Node.js application - i.e. before `http` and before your router etc.

This means that you should probably require and start the agent in your application's main file (usually `index.js`, `server.js` or `app.js`).

Here's a simple example of how Elastic APM is normally required and started:

```js
// Add this to the VERY top of the first file loaded in your app
var apm = require('elastic-apm-node').start({
  // Set required service name (allowed characters: a-z, A-Z, 0-9, -, _, and space)
  serviceName: '',

  // Use if APM Server requires a token
  secretToken: '',

  // Set custom APM Server URL (default: http://localhost:8200)
  serverUrl: '',
})
```

The agent will now monitor the performance of your application and record any uncaught exceptions.

### Advanced configuration

In the above example we initialize the agent by calling the start() function.
This function takes an optional options object used to configure the agent.
Any option not supplied via the options object can instead be configured using environment variables.
So if you prefer, you can set the same configuration options using environment variables:

```sh
ELASTIC_APM_SERVICE_NAME=<service name>
ELASTIC_APM_SECRET_TOKEN=<token>
ELASTIC_APM_SERVER_URL=<server url>
```

And then just start the agent like so:

```js
// Start the agent before any thing else in your app
var apm = require('elastic-apm-node').start()
```

### Full documentation

* [Setup and Configuration](https://www.elastic.co/guide/en/apm/agent/nodejs/current/advanced-setup.html)
* [API Reference](https://www.elastic.co/guide/en/apm/agent/nodejs/current/api.html)

## Performance monitoring

Elastic APM automatically measures the performance of your application.
It records spans for database queries,
external HTTP requests,
and other slow operations that happen during requests to your app.

By default the agent will instrument the most common modules.
To instrument other events,
you can use custom spans.
For information about custom spans,
see the [Custom Spans](https://www.elastic.co/guide/en/apm/agent/nodejs/current/custom-spans.html) section.

Spans are grouped in transactions - by default one for each incoming HTTP request.
But it's possible to create custom transactions not associated with an HTTP request.
See the [Custom Transactions](https://www.elastic.co/guide/en/apm/agent/nodejs/current/custom-transactions.html) section for details.

## Route naming

The Node.js agent tracks incoming HTTP requests to your application in what are called "transactions".
All transactions with the same name are grouped together automatically.

In a normal web application you want to name transactions based on the route that matches the incoming HTTP request.
So say you have a route to display posts on a blog identified by `GET /posts/{id}`.
You want requests `GET /posts/12`, `GET /posts/42` etc to be grouped together under a transaction named `GET /posts/{id}`.

If you are using Express, hapi or koa-router this naming happens automatically based on the names of your routes.
If you use another framework or a custom router you will see that the transactions are simply grouped together in a few big chunks named "unknown route".
In that case,
you will need to help us out a little by supplying a name for each transaction.
You can do that by calling `apm.setTransactionName()` at any time during the request with the name of the transaction as the first argument.

Except of an application using the [patterns](https://github.com/watson/patterns) module for route handling:

```js
var apm = require('elastic-apm-node').start()
var http = require('http')
var patterns = require('patterns')()

// Setup routes and their respective route handlers
patterns.add('GET /', require('./routes/index'))
patterns.add('GET /posts', require('./routes/posts').index)
patterns.add('GET /posts/{id}', require('./routes/posts').show)

http.createServer(function (req, res) {
  // Check if we have a route matching the incoming request
  var match = patterns.match(req.method + ' ' + req.url);

  // If no match is found, respond with a 404. Elastic APM will in
  // this case use the default transaction name "unknown route"
  if (!match) {
    res.writeHead(404)
    res.end()
    return
  }

  // The patterns module exposes the pattern used to match the
  // request on the `pattern` property, e.g. `GET /posts/{id}`
  apm.setTransactionName(match.pattern)

  // Populate the params and call the matching route handler
  var fn = match.value
  req.params = match.params
  fn(req, res)
}).listen(3000)
```

### Unknown routes

When viewing the performance metrics of your application in Elastic APM,
you might see some transactions named "unknown route".
This indicates that the Elastic APM Node.js agent detected an incoming HTTP request to your application,
but didn't know what to name it.

This might simply be 404 requests,
which by definition don't match any route,
or it might be a symptom that the agent wasn't installed correctly.
Make sure we either support your router or that you manually name your routes.

## Error logging

By default the Node.js agent will watch for uncaught exceptions and send them to Elastic APM automatically.
But in most cases errors are not thrown but returned via a callback,
caught by a promise,
or simply manually created.
Those errors will not automatically be sent to Elastic APM.
To manually send an error to Elastic APM,
simply call `apm.captureError()` with the error:

```js
var err = new Error('Ups, something broke!')

apm.captureError(err)
```

### Middleware error handler

If you use the [connect](https://www.npmjs.com/package/connect) module and an error is either thrown synchronously inside one of the middleware functions or is passed as the first argument to the middleware `next()` function,
it will be passed to the [Connect error handler](https://www.npmjs.com/package/connect#error-middleware).

It's recommended that you register the agent as a Connect error handler.
In the case where you have multiple Connect error handlers,
the agent error handler should be the first in the chain to ensure that it will receive the error correctly.

```js
var apm = require('elastic-apm-node').start()
var conncet = require('connect')

var app = connect()

// Your regular middleware and router...
app.use(...)
app.use(...)
app.use(...)

// Add the Elastic APM middleware after your regular middleware
app.use(apm.middleware.connect())

// ...but before any other error handler
app.use(function (err, req, res, next) {
  // Custom error handling goes here
})
```

## Filter sensitive information

By default the Node.js agent will filter common sensitive information before sending errors and metrics to the Elastic APM server.

It's possible for you to tweak these defaults or remove any information you don't want to send to Elastic APM:

* By default the Node.js agent will not log the body of HTTP requests.
To enable this,
use the `captureBody` config option
* By default the Node.js agent will filter certain HTTP headers known to contain sensitive information.
To disable this,
use the `filterHttpHeaders` config option
* To apply custom filters,
use the `apm.addFilter()` function

## Add your own data

The Node.js agent will keep track of the active HTTP request and will link it to errors and recorded transaction metrics when they are sent to the Elastic APM server.
This allows you to see details about which request resulted in a particular error or which requests cause a certain HTTP endpoint to be slow.

But in many cases,
information about the HTTP request itself isn't enough.
To add even more metadata to errors and transactions,
use one of the functions below:

* `apm.setUserContext()` - Call this to enrich collected performance data and errors with information about the user/client
* `apm.setCustomContext()` - Call this to enrich collected performance data and errors with any information that you think will help you debug performance issues and errors (this data is only stored, but not indexed in Elasticsearch)
* `apm.setTag()` - Call this to enrich collected performance data and errors with simple key/value strings that you think will help you debug performance issues and errors (tags are indexed in Elasticsearch)