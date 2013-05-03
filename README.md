# Node Chrome Logger

[![Build Status](https://secure.travis-ci.org/yannickcr/node-chromelogger.png)](http://travis-ci.org/yannickcr/node-chromelogger) [![Dependency Status](https://gemnasium.com/yannickcr/node-chromelogger.png)](https://gemnasium.com/yannickcr/node-chromelogger)

[Chrome Logger](http://craig.is/writing/chrome-logger) is a Google Chrome extension for debugging server side applications in the Chrome console.
This module is an implementation of the Chrome Logger protocol for Node.js, it allows you to log and inspect your server-side code directly in the Chrome console.

# Installation

    $ npm install chromelogger

and install the [Chrome Logger extension](https://chrome.google.com/webstore/detail/chromephp/noaneddfkdjfnfdakjjmocngnfkfehhd) in your Chrome browser.

# Usage

```javascript
var chromelogger = require('chromelogger');
var http = require('http');

var server = http.createServer();

server.on('request', chromelogger.middleware);

server.on('request', function(req, res) {
  res.chrome.log('Message from Node.js %s', process.version);
  res.end('Hello World');
});

server.listen(7357);
```

Node Chrome Logger provide several logging methods on the ServerResponse (res) object:
 * `res.chrome.log`
 * `res.chrome.warn`
 * `res.chrome.error`
 * `res.chrome.info`
 * `res.chrome.group`
 * `res.chrome.groupEnd`
 * `res.chrome.groupCollapse`

These methods matches the [Console API of the Chrome Developer Tools](https://developers.google.com/chrome-developer-tools/docs/console-api).

## Usage as an Express middleware

```javascript
var chromelogger = require('chromelogger');
var express = require('express');

var app = express();

app.use(chromelogger.middleware);

app.get('/', function(req, res) {
  res.chrome.log('Message from Express.js %s', express.version);
  res.end('Hello World');
});

app.listen(7357);
```

# Events

`error`: if an error occur (problem during JSON serialization, logging after the headers were already sent, etc.)

```javascript
chromelogger.on('error', function(message) {
  console.log(message);
});
```

# License

Node Chrome Logger is licensed under the [MIT License](http://www.opensource.org/licenses/mit-license.php).
