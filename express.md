## Routing
   * GET
   * POST
   * PUT
   * DELETE


simple routes: 

```JavaScript
app.get('/', function (req, res) {
  res.render('index.jade', { name: names });
});

app.post('/', function (req, res) {
  names.push(req.body.name);
  res.redirect('/');
});;
```

it doesn't parse the body of the http requests
```Shell
npm install body-parse
```
and then in server.js
```JavaScript
bodyParser = require('body-parser');
app.use(bodyParser.urlencoded());
```    

`app.all` will run on all routes
```JavaScript
app.all('/', function (req, res, next) {
  console.log('from all');
  next();
});
```

each route can take multiple callbacks
```JavaScript
function log(req, res, next) {
  console.log('logging');
  next();
}

app.get('/', log, function (req, res) {
  res.render('index.jade', { name: names });
});
```    

## App settings
```JavaScript
app.set();
app.enable();
app.disable();

app.enabled();
app.disabled();
```    

environment
```JavaScript
app.set('env', process.env.NODE_ENV || 'development');
```    

jsonp
```JavaScript
app.set('jsonp callback name', 'cb');
```
    
    
json replacer  (run when using JSON.sringify)
```JavaScript
app.set('json replacer', function (attr, val) {
  if (attr === 'passwordHash') {
    return undefined;
  }
});
```    
    
case sensitive routing
```JavaScript
app.enable('case sensitive routing'); // /hello /Hello
```
cache
```JavaScript
app.enable('view cache');
```    
view engine
```JavaScript
app.set('view engine', 'jade'); // or ejs
res.render('index') // don't need extension
```    
powered by
```JavaScript
app.enable('x-powered-by'); // or disable
```    
## middleware and request flow

log or change the request object as it goes thorugh our application
```JavaScript
// no longer of built-in middleware, have to require it
app.use(bodyParser.urlencoded());
```    
connect is no longer a dependency

requests now flow top through bottom
```JavaScript
// 1. third party middleware
app.use(bodyParser.urlencoded());

// 2. custom middleware
app.use(function (req, res, next) {
  console.log('this will log on every request');
  next()
});

// 3. route functions
app.get()
app.post()
...

// 4. built-in middleware
app.use(express.static('./public'));
```

https://github.com/senchalabs/connect/blob/master/Readme.md#middleware

https://github.com/senchalabs/connect/wiki

https://github.com/expressjs/


## custom middleware based on route params
```JavaScript
app.get('/name/:name', function (req, res) {
  res.send('your name is ' + req.name);
});

app.param('name', function (req, res, next, name) {
  req.name = name[0].toUpperCase() + name.substring(1);
  
  Users.findOne({username: name}, function (err, user) {
    req.ueser = user;
    next();
  });
  
  next();
});
```
## grouping routes
```JavaScript    
app.route('/')
    .all('/', function (req, res, next) {
       console.log('from all');
       next();
     });
    .get('/', log, function (req, res) {
      res.render('index', { names: names });
    })
    .post('/', function (req, res) {
      names.push(req.body.name);
      res.redirect('/'); // get request
    });
```

## router objects
so far, we can use these:
  * `use`
  * `param`
  * `get/post/put/delete/all`
  * `route`
  
router methods allow us to use all these methods in a contained environment.

```JavaScript
var router = express.Router();

router.use(function (req, res, next) {
  console.log('router specific middleware');
  next();
});

router.get('/', function (req, res) {
  res.send('router home route');
});

router.route('/test', function (req, res) {
  res.send('router test route');
});

// need to apply it to the app
app.use('/api', router);

```

or use it like this:

in `api.js`
```JavaScript
var api = require('express').Router();

api.get('/names', function (req, res) {
  res.send('names from api');
});

module.exports = api;
```

and in `app.js`
```JavaScript
var express = require('express'),
    bodyParser = require('body-parser'),
    app = express(),
    api = require('./api');

app.use('/api', api);

app.listen(3000);
```
## the request object

```JavaScript
app.get('/:userId?var=2', function (req, res) {
  req.params.userId;
  req.query.var;
  req.body.ATTR;
  
  // if not sure, then use req.param
  req.param(attr_name);
  
  req.route;
  req.originalUrl;
  
  req.get('header');
  req.accepts('text/html');
  
});
```
## the response object
```JavaScript
app.get('/:userId?var=2', function (req, res) {
  res.status(200);
  res.set(header, value);
  res.direct(status, path);
  res.send(status, text);
  res.json(status, object);
  res.download(file);
  res.render(file, props, function (err, html) {
    // do something with html
    res.send(html);
  });
});
```

## formatting requests
```JavaScript
app.get('/', function (req, res) {
  res.format({
    'text/plain': function () {
      res.send('text response');
    },
    'text/html': function () {
      res.render('index.jade');
    },
    'application/json': function () {
      res.json({topic: 'Express'});
    }
  });
})
```
