Alexa Lambda Deployment
===============

Once you set up a running Hapi server, you are then able to define routes using a method off of the instance of the `Hapi.Server()` called `route`.

```javascript
const Hapi = require('hapi');
const server = new Hapi.Server();

server.route({
	
})
```
---

# Methods

The `route` method from an instance of a Hapi server has the following basic elements: `method`, `path`, `handler`, and `config`. 

</br>

## method
The `method` method of the route takes in various valid HTTP methods, or even an array of methods such as `GET`, `POST`, and `DELETE`. This will define the method of HTTP request that you will called upon the `path` element of the server.

```javascript
server.route({
	method: 'GET'
})
```
#### OR with multiple methods..
```javascript
server.route({
	method: ['GET', 'POST']
})
```


## path
The `path` method of your route provides the location in which the the HTTP method will be called upon. Although the path location is a string, you may also pass in parameters to create a dynamic path using `{}`.

```javascript
server.route({
	method: 'GET',
	path: '/helloWorld'
})
```

Passing in a parameter would look like:

```javascript
server.route({
	method: 'GET',
	path: 'users/{userName}'
})
```
... and you can access/utilize the parameter with the following:
```javascript
server.route({
	method: 'GET',
	path: 'users/{userName}'
	handler: function(request, reply){
		reply(request.params.userName)
	}
})
```
What's going on within the handler method will be explained next but for now, hitting `/users/JohnDoe` would return a response of `JohnDoe`.

## handler
The `handler` method of your route will be using a function where the logic of your HTTP call is defined. The function takes in two parameters: `request`, and `reply`.

- The `request` parameter is an object with details about the end user's request, such as path parameters, an associated payload, authentication information, headers, etc.

- The second parameter, `reply`, is the method used to respond to the request. As you see in the following examples, if you wish to respond with a payload you simply pass the payload as a parameter to reply. The payload may be a string, a buffer, a JSON serializable object, or a stream. The result of reply is a response object, that can be chained with additional methods to alter the response before it is sent. For example reply('created').code(201) will send a payload of created with an HTTP status code of 201. You may also set headers, content type, content length, send a redirection response, and many other things that are documented in the API reference.


```javascript
server.route({
	method: 'GET',
	path: '/helloWorld',
	handler: function(request, reply){
		reply('Hello World!')
	}
})
```

Using a parameter in your path and reply would look like:

```javascript
server.route({
	method: 'GET',
	path: '/hello/{name}',
	handler: function(request, reply){
		reply('Hello ' + encodeURIComponent(request.params.name) + '!');
	}
})
```

You can even do multi-segment parameters like so:
```javascript
server.route({
    method: 'GET',
    path: '/hello/{user*2}',
    handler: function (request, reply) {
        const userParts = request.params.user.split('/');
        reply('Hello ' + encodeURIComponent(userParts[0]) + ' ' + encodeURIComponent(userParts[1]) + '!');
    }
});
```
By requesting `/hello/John/Doe`, the above would return `Hello John Doe!` because it splits the `request.params.user` by the `/`.



## config
Finally, the `config` method of your route is an optional, yet very useful element. This element of your route allows you to configure option such as `validation`, `authentication`, prerequisites, payload processing, and caching options. There is a list of options [here](https://hapijs.com/api#route-options)

```javascript
server.route({
    method: 'GET',
    path: '/hello/{user?}',
    handler: function (request, reply) {
        const user = request.params.user ? encodeURIComponent(request.params.user) : 'stranger';
        reply('Hello ' + user + '!');
    },
    config: {
        description: 'Say hello!',
        notes: 'The user parameter defaults to \'stranger\' if unspecified',
        tags: ['api', 'greeting']
    }
});
```

