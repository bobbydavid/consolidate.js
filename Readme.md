# Consolidate.js

  Template engine consolidation library.

## Installation

    $ npm install consolidate

## Supported template engines

  - [dust](https://github.com/akdubya/dustjs) [(website)](http://akdubya.github.com/dustjs/)
  - [eco](https://github.com/sstephenson/eco)
  - [ejs](https://github.com/visionmedia/ejs)
  - [haml](https://github.com/visionmedia/haml.js) [(website)](http://haml-lang.com/)
  - [haml-coffee](https://github.com/9elements/haml-coffee) [(website)](http://haml-lang.com/)
  - [handlebars](https://github.com/wycats/handlebars.js/) [(website)](http://handlebarsjs.com/)
  - [hogan](https://github.com/twitter/hogan.js) [(website)](http://twitter.github.com/hogan.js/)
  - [jade](https://github.com/visionmedia/jade) [(website)](http://jade-lang.com/)
  - [jazz](https://github.com/shinetech/jazz)
  - [jqtpl](https://github.com/kof/node-jqtpl) [(website)](http://api.jquery.com/category/plugins/templates/)
  - [kernel](https://github.com/c9/kernel) 
  - [liquor](https://github.com/chjj/liquor)
  - [swig](https://github.com/paularmstrong/swig) [(website)](http://paularmstrong.github.com/swig/)
  - [whiskers](https://github.com/gsf/whiskers.js/tree/)

__NOTE__: you must still install the engines you wish to use, add them to your package.json dependencies.

## API

  All templates supported by this library may be rendered using the signature `(path[, locals], callback)` as shown below, which happens to be the signature that Express 3.x supports so any of these engines may be used within Express.

```js
var cons = require('consolidate');
cons.swig('views/page.html', { user: 'tobi' }, function(err, html){
  if (err) throw err;
  console.log(html);
});
```

  Or without options / local variables:

```js
var cons = require('consolidate');
cons.swig('views/page.html', function(err, html){
  if (err) throw err;
  console.log(html);
});
```

  To dynamically pass the engine, simply use the subscript operator and a variable:

```js
var cons = require('consolidate')
  , name = 'swig';

cons[name]('views/page.html', { user: 'tobi' }, function(err, html){
  if (err) throw err;
  console.log(html);
});
```

## Caching

 To enable caching simply pass `{ cache: true }`. Engines _may_ use this option to cache things reading the file contents, compiled `Function`s etc. Engines which do _not_ support this may simply ignore it. All engines that consolidate.js implements I/O for will cache the file contents, ideal for production environments.

```js
var cons = require('consolidate');
cons.swig('views/page.html', { user: 'tobi' }, function(err, html){
  if (err) throw err;
  console.log(html);
});
```

## Express 3.x example

```js
var express = require('express')
  , cons = require('consolidate')
  , app = express();

// assign the swig engine to .html files
app.engine('html', cons.swig);

// set .html as the default extension 
app.set('view engine', 'html');
app.set('views', __dirname + '/views');

var users = [];
users.push({ name: 'tobi' });
users.push({ name: 'loki' });
users.push({ name: 'jane' });

app.get('/', function(req, res){
  res.render('index', {
    title: 'Consolidate.js'
  });
});

app.get('/users', function(req, res){
  res.render('users', {
    title: 'Users',
    users: users
  });
});

app.listen(3000);
console.log('Express server listening on port 3000');
```

## Running tests

  Install dev deps:

    $ npm install -d

  Run the tests:

    $ make test

## Template engine errata

Some template engines have little quirks that don't quite work with the
express framework as it is.

###Partials in hogan / whiskers:

Globally register a partial using the `app.settings('view partials')`
setting. An individual call to a template can also have its own
specific partials.

```js
// in app.js
app.settings('view partials') = {
    'base': '_base.mustache'
  , 'footer': '_footer.mustache'
};

// in routes.index
exports.index = function(req, res){
  /* This has access to 'base' and 'footer' partials */
  res.render('index', { title: 'Express' });
};
exports.foo = function(req, res){
  /* This also has access to 'header' partial */
  res.render('foo', { partials: { 'header': '_header.mustache' } });
};
exports.bar = function(req, res){
  /* This overrides the footer partial */
  res.render('bar', { partials: { 'footer': '_footer2.mustache' } });
};
```




## License

(The MIT License)

Copyright (c) 2011 TJ Holowaychuk &lt;tj@vision-media.ca&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
