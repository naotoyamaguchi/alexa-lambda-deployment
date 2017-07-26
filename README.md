Alexa Lambda Deployment
===============

To deploy a Node.js sample as a Lambda function, you first need to create a zip file containing the .js files provided in the sample. If the project uses the Alexa Skills Kit SDK for Node.js (which we may have), you need to also include these dependencies in your zip file. If you have not already installed Node.js and NPM, follow these steps first.

In a terminal or command line, navigate to the `src` directory for the sample and enter this command:
bash npm install --save alexa-sdk
This installs a node_modules directory containing the SDK dependencies.

Select all the contents of the src folder, and create a zip file.
The zip should contain the files, but not include the src folder. The index.js file must be at the root of the zip. The zip should include the node_modules directory.


## Creating your new Lambda function and uploading the code.

Once you have either a zip containing the Node.js code, do the following:
- Log in to the AWS Management Console and navigate to AWS Lambda.
-Create a new Lambda function in the US East (N. Virginia) or EU (Ireland) region.
- To quickly set up one of the blueprint samples, select the sample from the list of blueprints. The following samples are available as blueprints:
alexa-skill-kit-sdk-factskill
alexa-skill-kit-sdk-howtoskill
alexa-skill-kit-sdk-triviaskill
alexa-skills-kit-color-expert
Selecting one of these blueprints automatically imports the code into the Lambda console.
- Configure the new function with the following settings, depending on whether your are deploying a Node.js or Java sample:

| Setting       | Value         |
| ------------- | ------------- |
| Triggers      | Click the outlined box select Alexa Skills Kit.  |
| Name          | A descriptive name for the function.  |
| Description   | A description for the function.  |
| Runtime       | Node.js  |
| Lambda Function Code  | 	Select the Upload a .ZIP file option and upload the zip file you created. If you selected one of the blueprints (such as alexa-skill-kit-sdk-factskill), the code is already filled in.|
| Handler       |  index.handler |

## Select the Role for the function. This defines the AWS resources the function can access.

To use an existing role, select the role under `Use existing role`.
<br>
<br>
**otherwise...**
<br>
### Defining a New Role for the Function

The role specifies the AWS resources your function can access. To create a new role while configuring your function:
For Role (under Lambda function handler and role), select Create new role from template(s).
Enter the Role Name.
From the Policy templates list, select Simple Microservice permissions.

#### Make note of the Amazon Resource Name (ARN) for your new Lambda function. The ARN is displayed in the upper-right corner of the function page.

### Creating a New Skill for the Sample on the Developer Portal

- Register a new Alexa skill on the developer portal.
- For details, see Registering and Managing Custom Skills in the Developer Portal.
- Use the following information when registering the new skill:

<br>

| Setting |	Value |
| ------- | ----- |
| Invocation Name	Any valid invocation name. |
| Name |	Any valid name. |
| Endpoint |	Select the AWS Lambda ARN (Amazon Resource Name) option, then either North America or Europe and enter the ARN for your function. |
| Interaction Model |	Each sample includes an interaction model in the speechAssets folder for the sample. |

Copy the JSON from IntentSchema.json into the Intent Schema box and copy the text from SampleUtterances.txt into the Sample Utterances box.

For details about defining the interaction model, see Define the Interaction Model in JSON and Text.













<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
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

