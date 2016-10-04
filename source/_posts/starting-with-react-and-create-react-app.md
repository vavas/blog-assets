title: Starting With React And create-react-app
date: 2016-10-04 15:03:53
tags:
  - react
---

![Starting with React #1 - tutorial + create-react-app](http://i.imgur.com/ZquITPl.png)

In this article we will start a series of blog posts about [React](https://facebook.github.io/react/).

The first application that we are going to create is a comment system, where you will be able to `GET` (retrieve) and `POST` (create) comments through a REST API. Our server will be built with Node.js and our database will be a simple JSON file.

To bootstrap our app we will use the amazing and really helpful tool [create-react-app](https://github.com/facebookincubator/create-react-app). It's the official tool from Facebook for generating React apps with **no build configuration**.

This article is an "updated version" of the [Tutorial](https://facebook.github.io/react/docs/tutorial.html) from React's docs. Here you will see *How* to create the same application explained in the article mentioned using ES6 and [axios](https://github.com/mzabriskie/axios) to make our AJAX calls, instead of ES5 and jQuery in the original article. For the *Whys*, you should read the original article, and I'll point you there for your convenience when necessary.

Let's start!

## Source Code
You can find the source code [here](https://github.com/ericdouglas/es6-react-tutorial).

## Bootstrap
To bootstrap our React application, let's first `npm install -g create-react-app`.

After this, you should run the following commands in your terminal:

```sh
create-react-app comments-app
cd comments-app
npm start
```

Now your browser will open and for each modification in your code, the browser will be automatically updated. You will also get lint/debug information in the terminal and in the browser's console.

## Running a Server
Let's create a Node.js server that will serve as our API endpoint. It will runs in the port 4000.

In the root folder, create a file called `server.js`.

```js
var fs = require('fs');
var path = require('path');
var express = require('express');
var bodyParser = require('body-parser');
var app = express();

var COMMENTS_FILE = path.join(__dirname, 'comments.json');

app.set('port', (process.env.PORT || 4000));

app.use('/', express.static(path.join(__dirname, 'public')));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({extended: true}));

// Additional middleware which will set headers that we need on each request.
app.use(function(req, res, next) {
    // Set permissive CORS header - this allows this server to be used only as
    // an API server in conjunction with something like webpack-dev-server.
    res.setHeader('Access-Control-Allow-Origin', '*');

    // Disable caching so we'll always get the latest comments.
    res.setHeader('Cache-Control', 'no-cache');

    // Other stuff
    res.setHeader("Access-Control-Allow-Credentials", "true");
    res.setHeader("Access-Control-Allow-Methods", "GET,HEAD,OPTIONS,POST,PUT");
    res.setHeader("Access-Control-Allow-Headers", "Access-Control-Allow-Headers, Origin,Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Access-Control-Request-Headers");

    next();
});

app.get('/api/comments', function(req, res) {
  fs.readFile(COMMENTS_FILE, function(err, data) {
    if (err) {
      console.error(err);
      process.exit(1);
    }
    res.json(JSON.parse(data));
  });
});

app.post('/api/comments', function(req, res) {
  fs.readFile(COMMENTS_FILE, function(err, data) {
    if (err) {
      console.error(err);
      process.exit(1);
    }
    var comments = JSON.parse(data);
    // NOTE: In a real implementation, we would likely rely on a database or
    // some other approach (e.g. UUIDs) to ensure a globally unique id. We'll
    // treat Date.now() as unique-enough for our purposes.
    var newComment = {
      id: Date.now(),
      author: req.body.author,
      text: req.body.text,
    };
    comments.push(newComment);
    fs.writeFile(COMMENTS_FILE, JSON.stringify(comments, null, 4), function(err) {
      if (err) {
        console.error(err);
        process.exit(1);
      }
      res.json(comments);
    });
  });
});


app.listen(app.get('port'), function() {
  console.log('Server started: http://localhost:' + app.get('port') + '/');
});
```

## Your first component
We are going to create 4 components in our application:

```sh
- CommentBox
    - CommentList
        - Comment
    - CommentForm
```

In our scaffold project we already have one component called `<App />` that we will use as the entry point for our application.

Now, let's create our first component `<CommentBox />` and import it into the `<App />` component.

Inside the `src` folder, create a folder called `components` and a file inside that folder called `CommentBox.js`.

```sh
mkdir src/components
touch src/components/CommentBox.js
```

Now open the `CommentBox.js` file and add the following code:

```js
// CommentBox.js
import React, { Component } from 'react';

class CommentBox extends Component {}

export default CommentBox;
```

Great! This is the basic structure that all our components will have. Let's fulfill this component with some basic message to test if our setup is working correctly.

```js
// CommentBox.js
import React, { Component } from 'react';

class CommentBox extends Component {
  render() {
    return (
      <div className="commentBox">
        Hello, world! I am a CommentBox.
      </div>
    );
  }
}

export default CommentBox;
```

Now we need to import `CommentBox` into the `<App />` component. The `App.js` file located in the `src` folder should have the following code:

```js
// App.js
import React, { Component } from 'react';

import CommentBox from './components/CommentBox';

class App extends Component {
  render() {
    return (
      <CommentBox />
    );
  }
}

export default App;
```
