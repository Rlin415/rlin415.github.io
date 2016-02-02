---
layout: post
title: Composing a Hapi server
---

If you're a Node developer you're probably already familiar with Express, but what is Hapi?
Hapi is another server-side framework just like Express for Node. It distinguishes itself
from Express through it's ease of maintainability and extensibility. It is a configuration-centric
framework that has added features on top of Node, like input validation, caching, authentication,
and many more. It is best used for bigger or more complex applications.

Before we get started we've got to install some dependencies. Run this in your CLI:

```
npm install --save hapi glue
```

Now that we have Hapi installed we can start setting up our application. We'll get into what Glue does later.

### Setting up the server

```
/
|_ server
  |_ server.js*
```

```
"use strict";

const Hapi = require('hapi');

const server = new Hapi.server();

server.connection({
  host: 'localhost',
  port: 3000
});

server.route({
  method: 'GET',
  path: '/',
  handler: (request, reply) => {
    return reply('Hello, world!');
  }
});

server.start((err) => {
  if (err) throw err;
  console.log('Server running at:', server.info.uri);
});
```

This is a basic way of setting up a server in Hapi, but there's a much cleaner and modular way of doing it. What we can do is wrap all these options into one file called manifest.json. In this file we will define all our server's configurations and dependencies.

### Setting up the manifest

```
/
|_ server
  |_ manifest.json*
  |_ server.js
```

```
{
  "connections": [
    {
      "port": 3003,
      "labels": ["api"],
      "routes": {
        "cors": true
      }   
    }
  ]
}
```
The connections property hold all the setting we want for our server. We want it listen to port 3003, give it a label of "api", and allow CORS. Alright, so how do we add in our routes in here too? Well, let's set up a file for our routes first.

### Setting up the routes

```
/
|_ server
  |_ manifest.json
  |_ routes.js*
  |_ server.js
```

```
"use strict"

exports.register = (server, options, next) => {
  server.route([
    {method: 'GET', path: '/', handler: (request, reply) => {
      return reply('Hello, world!');
    }}
  ]);
  next();
};

exports.register.attributes = {
  name: 'routes'
};
```

This syntax might seem confusing at first, especially if you're coming from an Express background. Hapi calls what we defined here, a plugin. An easy way to understand this is, think of plugins in Hapi as middlewares in Express. It's a way of splitting up the server logic and organizing application code. You can attach and manipulate properties on the request object just like with middlewares. Something to keep in mind here is that the 'next' function does not work like the 'next' function in Express middlewares. The 'next' in plugins
returns control back to Hapi to complete the registration process when composing the server.

Ok, so how do we register this plugin when composing our server? Let's look back at our manifest.json file.

```
/
|_ server
  |_ manifest.json*
  |_ routes.js
  |_ server.js
```

```
{
  "connections": [
    {
      "port": 3003,
      "labels": ["api"],
      "routes": {
        "cors": true
      }   
    }
  ],
  "registrations": [
    {
      "plugin": "./routes"
    }
  ]
}
```   

This is how we would specify what kind of plugins we want registered in our Hapi server. Now
that we have our manifest.json built out, how do we compose our server? This is where Glue comes in to
help us.

### Composing the server

```
/
|_ server
  |_ manifest.json
  |_ routes.js
  |_ server.js*
```

```
"use strict";

const Hapi = require('hapi');
const Glue = require('glue');
const manifest = require('./manifest');

Glue.compose(manifest, { relativeTo: __dirname }, (err, server) => {

  if (err) console.error('server.register err:', err);

  server.start(() => {
    console.log("Server running at:", server.info.uri);
  });
});
```

Glue composes our Hapi server from an object passed into it containing all the options we described in manifest.json. We also define an options property called relativeTo passed into Glue as the second argument which has a value of the current directory's path. This path is used to resolve loading the plugins in our manifest.json.

### Conclusion

Hapi could be quite challenging to work with and understand in the beginning. It is fairly new and so there aren't many examples of creating a Hapi application on the Internet yet. My team and I ran into a ton of obstacles while creating our product using Hapi, but it was well worth it at the end. We engineered an outstanding product that is easily maintainable and extensible. It is a framework that is well worth your time and effort to learn!
