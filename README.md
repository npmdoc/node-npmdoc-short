# api documentation for  [short (v2.6.0)](http://edwardhotchkiss.github.com/short)  [![npm package](https://img.shields.io/npm/v/npmdoc-short.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-short) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-short.svg)](https://travis-ci.org/npmdoc/node-npmdoc-short)
#### Node.js URL Shortener backed by Mongoose.js

[![NPM](https://nodei.co/npm/short.png?downloads=true)](https://www.npmjs.com/package/short)

[![apidoc](https://npmdoc.github.io/node-npmdoc-short/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-short_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-short/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-short/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-short/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Edward Hotchkiss",
        "email": "edward@edwardhotchkiss.com"
    },
    "bugs": {
        "url": "https://github.com/edwardhotchkiss/short/issues"
    },
    "contributors": [
        {
            "name": "Edward Hotchkiss",
            "email": "edward@edwardhotchkiss.com"
        },
        {
            "name": "Kevin Roth"
        },
        {
            "name": "Chase Brammer"
        },
        {
            "name": "Chris Lynch"
        },
        {
            "name": "CashLee"
        },
        {
            "name": "Patrick Williams"
        },
        {
            "name": "Niklas Nihlén"
        },
        {
            "name": "Matt Roman"
        },
        {
            "name": "Jan Paul Erkelens"
        },
        {
            "name": "Tarang Shah"
        }
    ],
    "dependencies": {
        "mongoose": "4.0.6",
        "node-promise": "0.5.8",
        "short-id": "0.1.0-1"
    },
    "description": "Node.js URL Shortener backed by Mongoose.js",
    "devDependencies": {
        "vows": "0.7.0"
    },
    "directories": {},
    "dist": {
        "shasum": "cf6b6a6a4091e95b4542dbcf7fdd7ec0c1a12773",
        "tarball": "https://registry.npmjs.org/short/-/short-2.6.0.tgz"
    },
    "engines": {
        "node": ">=0.10.0"
    },
    "gitHead": "cea11c40e1137c8ebd8d5186e88c3f3f4f6a9df0",
    "homepage": "http://edwardhotchkiss.github.com/short",
    "keywords": [
        "short",
        "url",
        "shortener",
        "tiny",
        "uri",
        "vanity",
        "bit.ly"
    ],
    "license": {
        "type": "MIT",
        "url": "http://github.com/edwardhotchkiss/short/LICENSE"
    },
    "main": "./lib/short",
    "maintainers": [
        {
            "name": "edwardhotchkiss",
            "email": "e@ingk.com"
        }
    ],
    "name": "short",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/edwardhotchkiss/short.git"
    },
    "scripts": {
        "test": "vows test/*.test.js --spec"
    },
    "version": "2.6.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module short](#apidoc.module.short)
1.  [function <span class="apidocSignatureSpan">short.</span>connect (mongodb)](#apidoc.element.short.connect)
1.  [function <span class="apidocSignatureSpan">short.</span>generate (document)](#apidoc.element.short.generate)
1.  [function <span class="apidocSignatureSpan">short.</span>hits (hash)](#apidoc.element.short.hits)
1.  [function <span class="apidocSignatureSpan">short.</span>list ()](#apidoc.element.short.list)
1.  [function <span class="apidocSignatureSpan">short.</span>retrieve (hash)](#apidoc.element.short.retrieve)
1.  [function <span class="apidocSignatureSpan">short.</span>update (hash, updates)](#apidoc.element.short.update)
1.  object <span class="apidocSignatureSpan">short.</span>prototype

#### [module short.prototype](#apidoc.module.short.prototype)
1.  [function <span class="apidocSignatureSpan">short.prototype.</span>Model (mongooseModel)](#apidoc.element.short.prototype.Model)



# <a name="apidoc.module.short"></a>[module short](#apidoc.module.short)

#### <a name="apidoc.element.short.connect"></a>[function <span class="apidocSignatureSpan">short.</span>connect (mongodb)](#apidoc.element.short.connect)
- description and source-code
```javascript
connect = function (mongodb) {
  if (mongoose.connection.readyState === 0)
    mongoose.connect(mongodb);

  exports.connection = mongoose.connection;
}
```
- example usage
```shell
...
**Generates a Shortened URL Doc, then retrieves it for demo:**

'''javascript
var shortURLPromise
  , short = require('../lib/short');

// connect to mongodb
short.connect('mongodb://localhost/short');

short.connection.on('error', function(error) {
  throw new Error(error);
});

// promise to generate a shortened URL.
var shortURLPromise = short.generate({
...
```

#### <a name="apidoc.element.short.generate"></a>[function <span class="apidocSignatureSpan">short.</span>generate (document)](#apidoc.element.short.generate)
- description and source-code
```javascript
generate = function (document) {
  var generatePromise
    , promise = new Promise();

  document['data'] = document.data || null;

  // hash was specified, so we should always honor it
  if (document.hasOwnProperty('hash')) {
    generatePromise = ShortURL.create(document);
  } else {
    document['hash'] = ID.store(document.URL);
    generatePromise = ShortURL.findOrCreate({URL : document.URL}, document, {});
  }

  generatePromise.then(function(ShortURLObject) {
    promise.resolve(ShortURLObject);
  }, function(error) {
    promise.reject(error, true);
  });

  return promise;
}
```
- example usage
```shell
...
short.connect('mongodb://localhost/short');

short.connection.on('error', function(error) {
throw new Error(error);
});

// promise to generate a shortened URL.
var shortURLPromise = short.generate({
URL : 'http://nodejs.org/'
});

// gets back the short url document, and then retrieves it
shortURLPromise.then(function(mongodbDoc) {
console.log('>> created short URL:');
console.log(mongodbDoc);
...
```

#### <a name="apidoc.element.short.hits"></a>[function <span class="apidocSignatureSpan">short.</span>hits (hash)](#apidoc.element.short.hits)
- description and source-code
```javascript
hits = function (hash) {
  var promise = new Promise();
  var query = { hash : hash }
    , options = { multi: true };
  var retrievePromise = ShortURL.findOne(query);
  retrievePromise.then(function(ShortURLObject) {
    if (ShortURLObject && ShortURLObject !== null) {
      promise.resolve(ShortURLObject.hits);
    } else {
      promise.reject(new Error('MongoDB - Cannot find Document'), true);
    };
  }, function(error) {
    promise.reject(error, true);
  });
  return promise;
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.short.list"></a>[function <span class="apidocSignatureSpan">short.</span>list ()](#apidoc.element.short.list)
- description and source-code
```javascript
list = function () {
  return ShortURL.find({});
}
```
- example usage
```shell
...
short.connect('mongodb://localhost/short');

short.connection.on('error', function(error) {
  throw new Error(error);
});

// promise to retrieve all shortened URLs
listURLsPromise = short.list();

// output all resulting shortened url db docs
listURLsPromise.then(function(URLsDocument) {
  console.log('>> listing (%d) Shortened URLS:', URLsDocument.length);
  console.log(URLsDocument);
  process.exit(0);
}, function(error) {
...
```

#### <a name="apidoc.element.short.retrieve"></a>[function <span class="apidocSignatureSpan">short.</span>retrieve (hash)](#apidoc.element.short.retrieve)
- description and source-code
```javascript
retrieve = function (hash) {
  var promise = new Promise();
  var query = { hash : hash }
    , update = { $inc: { hits: 1 } }
    , options = { multi: true };
  var retrievePromise = ShortURL.findOne(query);
  ShortURL.update( query, update , options , function (){ } );
  retrievePromise.then(function(ShortURLObject) {
    if (ShortURLObject && ShortURLObject !== null) {
      promise.resolve(ShortURLObject);
    } else {
      promise.reject(new Error('MongoDB - Cannot find Document'), true);
    };
  }, function(error) {
    promise.reject(error, true);
  });
  return promise;
}
```
- example usage
```shell
...
});

// gets back the short url document, and then retrieves it
shortURLPromise.then(function(mongodbDoc) {
console.log('>> created short URL:');
console.log(mongodbDoc);
console.log('>> retrieving short URL: %s', mongodbDoc.hash);
short.retrieve(mongodbDoc.hash).then(function(result) {
  console.log('>> retrieve result:');
  console.log(result);
  process.exit(0);
}, function(error) {
  if (error) {
    throw new Error(error);
  }
...
```

#### <a name="apidoc.element.short.update"></a>[function <span class="apidocSignatureSpan">short.</span>update (hash, updates)](#apidoc.element.short.update)
- description and source-code
```javascript
update = function (hash, updates) {
  var promise = new Promise();
  ShortURL.findOne({hash: hash}, function(err, doc) {
    if (updates.URL) {
      doc.URL = updates.URL;
    }
    if (updates.data) {
      doc.data = extend(doc.data, updates.data);
      doc.markModified('data'); //Required by mongoose, as data is of Mixed type
    }
    doc.save(function(err, updatedObj, numAffected) {
      if (err) {
        promise.reject(new Error('MongoDB - Cannot save updates'), true);
      } else {
        promise.resolve(updatedObj);
      }
    });
  });
  return promise;
}
```
- example usage
```shell
...
});
'''

**Updating the URL or the data fields of an existing Short URL hash**

'''javascript
// Basically, update works like this
var updatePromise = short.update(hash, updateData);
// hash => Short url hashcode generated using short.generate()
// updateData => An object consisting of the new URL and/or the new data object.
//               If a key already exists in the current data object, it's value is updated,
//               otherwise, it is added and saved to the data object

//This function returns a promise which on resolution returns the new updated object as an argument.
'''
...
```



# <a name="apidoc.module.short.prototype"></a>[module short.prototype](#apidoc.module.short.prototype)

#### <a name="apidoc.element.short.prototype.Model"></a>[function <span class="apidocSignatureSpan">short.prototype.</span>Model (mongooseModel)](#apidoc.element.short.prototype.Model)
- description and source-code
```javascript
Model = function (mongooseModel) {
  this.baseModel = mongooseModel;
}
```
- example usage
```shell
...
  URL        : { type : String, unique: false },
  hash       : { type : String, unique: true },
  hits       : { type : Number, default: 0 },
  data       : { type : Schema.Types.Mixed },
  created_at : { type : Date, default: Date.now },
}, options);

exports.ShortURL = new wrapper.Model(mongoose.model('ShortURL', ShortURLSchema));
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
