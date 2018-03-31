# Express API

## Create Express App

Install Express generator globally:

`$ sudo yarn global add express-generator`

Create App skeleton:

`$ express express-react`

Install dependences:

`$ yarn`

Enable server restart on file changes: installs Nodemon:

`$ yarn add -D nodemon`

Edit `/express-react/package,json`:
```
  ...
  "scripts": {
    "start": "PORT=3001 node ./bin/www",
    "start:watch": "PORT=3001 nodemon node ./bin/www"
  },
  ...  
```

Start server for development:

`$ yarn start:watch`

Explore Express at [http://localhost:3001/users]()

Edit file `/express-react/routes/users.js` to add some data. Then reload
 the browser to view the data.

## Create React App

Install `create-react-app` globally

`$ sudo yarn global create-react-app`

From inside the `express-react` folder, create the React app:

`~/express-react $ create-react-app client`

## Configure the Proxy

Add to the file `/express-react/client/package.json` for **client ( React)**:

```
 "proxy": "http://localhost:3001"
 ```
The port (3001) in the “proxy” line must match the port that your Express
server is running on.

### How the Proxy works

`package.json` has `"proxy": "http://you-server"`

React App calls `http://localhost:3000/api/users`

Dev server forwards call to `http://you-server/api/users`

## Fetch the Data from React

Start the React development server

`~/express-react/client $ yarn start`

Edit file `/express-react/client/src/App.js`

## Deploy Express & React to production

Way to do this: Express and React files sit on the same machine, and
Express does double duty: it serves the React files, and it also serves
API requests.

### Set Express to serve React App

Edit file `/express-react/app.js`:

```
var express = require('express');
var path = require('path');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');
var users = require('./routes/users');
var app = express();

app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'client/build')));
app.use('/users', users);
app.get('*', (req, res) => {
  res.sendFile(path.join(__dirname+'/client/build/index.html'));
});

module.exports = app;
```

Add `"heroku-postbuild"` command to the `"scripts"` in `/express-react/package.json`

```
...
  "scripts": {
    ...
    "heroku-postbuild": "cd client && yarn --production=false && yarn run build"
  },
...
```

For `npm` use:
```
"heroku-postbuild": "cd client && npm install --only=dev && npm install && npm run build"
```

### Check Heroku-CLI

To deploy used [Heroku](https://dashboard.heroku.com/apps)

```
$ heroku --version
```
Or follow the instructions [here](https://devcenter.heroku.com/articles/heroku-cli) to install it.

### Set Up Heroku

#### Git Init

```
$ git init
$ echo node_modules > .gitignore
$ git add .
$ git commit -m "Initial commit"
```

#### Create Heroku

```
$ heroku create
```




## Sources

[Dave Ceddia](https://daveceddia.com/):

[Create React App with an Express Backend](https://daveceddia.com/create-react-app-express-backend/?utm_campaign=welcome)

[Create React App with Express in Production](https://daveceddia.com/create-react-app-express-production/)

