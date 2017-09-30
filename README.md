![Seneca](http://senecajs.org/files/assets/seneca-logo.png)

> A [Seneca.js][] data storage plugin for AWS S3

# seneca-jsonfile-store
[![npm version][npm-badge]][npm-url]

## Description

**_Basically has the exact same features as 'seneca-jsonfile-store' but it connects to amazon S3 buckets!_**

This module is a plugin for [Seneca.js][]. It provides a storage engine that uses JSON files to
persist data. This module is not appropriate for production usage, it is intended for very low
workloads, and as a example of a storage plugin code base.

For a gentle introduction to Seneca itself, see the [senecajs.org][seneca.js] site.

### Seneca compatibility
Supports Seneca versions **1.x** - **3.x**

### Supported functionality
All Seneca data store supported functionality is implemented in [seneca-store-test](https://github.com/senecajs/seneca-store-test) as a test suite. The tests represent the store functionality specifications.

## Install
To install, simply use npm. Remember you will need to install [Seneca.js][] separately.

```sh
npm install seneca
npm install seneca-jsonfile-store
```

## Configure AWS connection with S3

Load the plugin with `seneca` using `.use()` and giving the following configuration:

```js
.use('seneca-aws-s3-store', {
  folder: 'my-s3-bucket-name',
  aws: {
    region: 'aws-region',
    accessKeyId: 'aws-access-key-id',
    secretAccessKey: 'aws-secret-access-key'
  }
})
```

## Usage
You don't use this module directly. It provides an underlying data storage engine
for the Seneca entity API:

```js
var entity = seneca.make$('typename')
entity.someproperty = "something"
entity.anotherproperty = 100

entity.save$(function (err, entity) { ... })
entity.load$({ id: ... }, function (err, entity) { ... })
entity.list$({ property: ... }, function (err, entity) { ... })
entity.remove$({ id: ... }, function (err, entity) { ... })
```

## Quick Example
```js
var seneca = require('seneca')()
seneca.use('jsonfile-store', {
  folder:'/path/to/my-db-folder'
})
.use('entity')

var apple = seneca.make$('fruit')
apple.name  = 'Pink Lady'
apple.price = 0.99
apple.save$(function (err, apple) {
  console.log("apple.id = " + apple.id )
})
```

### Query Support
The standard Seneca query format is supported:

- `.list$({f1:v1, f2:v2, ...})` implies pseudo-query `f1==v1 AND f2==v2, ...`.

- `.list$({f1:v1,...}, {sort$:{field1:1}})` means sort by f1, ascending.

- `.list$({f1:v1,...}, {sort$:{field1:-1}})` means sort by f1, descending.

- `.list$({f1:v1,...}, {limit$:10})` means only return 10 results.

- `.list$({f1:v1,...}, {skip$:5})` means skip the first 5.

- `.list$({f1:v1,...}, {fields$:['fd1','f2']})` means only return the listed fields.

Note: you can use `sort$`, `limit$`, `skip$` and `fields$` together.

## Contributing
The [Senecajs org][] encourages open participation. If you feel you
can help in any way, be it with documentation, examples, extra
testing, or new features please get in touch.

## Test
To run tests, simply use npm:

```sh
npm run test
```

## License
Copyright (c) 2012-2016, Richard Rodger and other contributors.
Licensed under [MIT][].

[MIT]: ./LICENSE
[Senecajs org]: https://github.com/senecajs/
[Seneca.js]: https://www.npmjs.com/package/seneca
[npm-badge]: https://img.shields.io/npm/v/seneca-aws-s3-store.svg
[npm-url]: https://www.npmjs.com/package/seneca-aws-s3-store
