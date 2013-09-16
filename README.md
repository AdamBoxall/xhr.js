# xhr.js

## A simple, AMD-compatible JavaScript XHR module

### What's that?

Modern browsers make AJAX requests using the Window's XMLHttpRequest object. This is just a very simple wrapper, with some backwards compatibility for older Internet Explorer versions, to make firing off AJAX requests very simple.

### Why not just use jQuery?

In order to use jQuery.ajax you have to include the entire jQuery core into your page. If performance is a big factor in your JS application, then it could be preferable to use a collection of lightweight modules instead, like this. xhr.js weighs in at a tiny 1.14 KB.

### Installation

#### Window

Just download `xhr.min.js` and include the script into your document:

    <script src="/path/to/src/xhr.min.js"></script>

That's it.

#### AMD

Download `xhr.min.js` into your modules directory and configure with RequireJS. Check the [RequireJS documentation](http://requirejs.org/) for more help with AMD.

### Usage

When used without AMD, xhr.js is exposed as a global object called `xhr`. The object contains three methods; get(), post() and request(). The get() and post() methods are just shortcuts for request(), with a little syntax sugar.

#### GET requests

The get() method accepts just two arguments; the URI you want to request and the callback.

    xhr.get('/request', function() {
        // Handle response
    });

#### POST requests

The post() method accepts three arguments; the URI you want to request, the data to send and the callback.

    var data = {
        foo: 'bar'
    }

    xhr.post('/request', data, function() {
        // Handle response
    });

Alternatively if you pass a string as the second argument, it's assumed to be JSON. The request's `Content-Type` header is updated and the data is ultimately sent within the request body instead.

    var data = JSON.stringify({
        foo: 'bar'
    });

    xhr.post('/request', data, function() {
        // Handle response
    });

#### Other requests

The request() method accepts only a single argument, a list of options. The current supported options are `uri`, `method`, `data` and `callback` -- more may be added in the future.

    var data = {
        foo: 'bar'
    };

    xhr.request({
        uri: '/request',
        method: 'POST',
        data: data,
        callback: function() {
            // Handle response
        }
    });

#### Callbacks

The first argument passed to every callback is the error, or null if the request was successful. That means checking for errors can be done with a simple IF condition:

    function(err) {
        if (err) {
            return alert('There was an error: ' + err);
        }
    }

On successful requests, the second argument passed to the callback is the response body.

    function(err, response) {

        if (err) {
            return alert('There was an error: ' + err);
        }

        document.getElementById('output').innerHTML = response;
    }

If the response's `Content-Type` header indicates that it's JSON content, xhr.js will automatically parse it into an object for you.

    // Assume this JSON is returned in the request:
    // {"title": "New title", "content": "<p>New content</p>"}

    function(err, response) {

        if (err) {
            return alert('There was an error: ' + err);
        }

        window.title = response.title;
        document.getElementById('output').innerHTML = response.content;
    }

There's also an optional third argument passed in, which is the instance of XMLHttpRequest for that request. If you need to find specific HTTP response codes or access headers for example, this is the way.

    function(err, response, xhr) {

        if (err) {
            return alert('There was an error: ' + err);
        }

        alert('Response code was ' + xhr.status);
    }

#### Detecting xhr.js requests

xhr.js follows standard practise by adding the `X-Requested-With` header with the value "XMLHttpRequest" to all the requests it makes. To detect requests made with xhr.js at the server-end, you just need to check against that header.

### Suggestions for improvement

If you have any suggestions to improve xhr.js, please raise an issue to let me know. I'm aware that it currently doesn't support JSONP, this will be added shortly.