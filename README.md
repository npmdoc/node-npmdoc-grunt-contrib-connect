# api documentation for  [grunt-contrib-connect (v1.0.2)](https://github.com/gruntjs/grunt-contrib-connect#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-grunt-contrib-connect.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-grunt-contrib-connect) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-grunt-contrib-connect.svg)](https://travis-ci.org/npmdoc/node-npmdoc-grunt-contrib-connect)
#### Start a connect web server

[![NPM](https://nodei.co/npm/grunt-contrib-connect.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/grunt-contrib-connect)

[![apidoc](https://npmdoc.github.io/node-npmdoc-grunt-contrib-connect/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-grunt-contrib-connect/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-grunt-contrib-connect/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-grunt-contrib-connect/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "appveyor_id": "3bp93hbs2rd5lwfd",
    "author": {
        "name": "Grunt Team",
        "url": "http://gruntjs.com/"
    },
    "bugs": {
        "url": "https://github.com/gruntjs/grunt-contrib-connect/issues"
    },
    "contributors": [
        {
            "name": "\"Cowboy\" Ben Alman",
            "url": "http://benalman.com"
        },
        {
            "name": "Tyler Kellen",
            "url": "http://goingslowly.com"
        },
        {
            "name": "Sindre Sorhus",
            "url": "http://sindresorhus.com"
        },
        {
            "name": "Kyle Robinson Young"
        },
        {
            "name": "Jared Stehler"
        },
        {
            "name": "Vlad Filippov"
        },
        {
            "name": "Stepan Stolyarov"
        },
        {
            "name": "Samori Gorse"
        },
        {
            "name": "Nicolas Gryman"
        },
        {
            "name": "Lachèze Alexandre"
        },
        {
            "name": "Jubal Mabaquiao"
        },
        {
            "name": "Eddie Monge Jr"
        },
        {
            "name": "Christopher Joslyn"
        },
        {
            "name": "Ates Goral"
        },
        {
            "name": "Alex Treppass"
        },
        {
            "name": "Aaron Lampros"
        },
        {
            "name": "Philipp Söhnlein"
        }
    ],
    "dependencies": {
        "async": "^1.5.2",
        "connect": "^3.4.0",
        "connect-livereload": "^0.5.0",
        "http2": "^3.3.4",
        "morgan": "^1.6.1",
        "opn": "^4.0.0",
        "portscanner": "^1.0.0",
        "serve-index": "^1.7.1",
        "serve-static": "^1.10.0"
    },
    "description": "Start a connect web server",
    "devDependencies": {
        "grunt": "^1.0.0",
        "grunt-contrib-internal": "^1.1.0",
        "grunt-contrib-jshint": "^1.0.0",
        "grunt-contrib-nodeunit": "^1.0.0"
    },
    "directories": {},
    "dist": {
        "shasum": "5cf933b91a67386044273c0b2444603cd98879ba",
        "tarball": "https://registry.npmjs.org/grunt-contrib-connect/-/grunt-contrib-connect-1.0.2.tgz"
    },
    "engines": {
        "node": ">=0.10.0"
    },
    "files": [
        "tasks"
    ],
    "gitHead": "03fa06ecc22a285c1ef458ca2222e85a880051d4",
    "homepage": "https://github.com/gruntjs/grunt-contrib-connect#readme",
    "keywords": [
        "gruntplugin",
        "server",
        "connect",
        "http"
    ],
    "license": "MIT",
    "main": "tasks/connect.js",
    "maintainers": [
        {
            "name": "cowboy"
        },
        {
            "name": "tkellen"
        },
        {
            "name": "shama"
        },
        {
            "name": "sindresorhus"
        },
        {
            "name": "vladikoff"
        },
        {
            "name": "jmeas"
        }
    ],
    "name": "grunt-contrib-connect",
    "optionalDependencies": {},
    "peerDependencies": {
        "grunt": ">=0.4.0"
    },
    "repository": {
        "type": "git",
        "url": "git+https://github.com/gruntjs/grunt-contrib-connect.git"
    },
    "scripts": {
        "test": "grunt test --verbose"
    },
    "version": "1.0.2"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module grunt-contrib-connect](#apidoc.module.grunt-contrib-connect)
1.  [function <span class="apidocSignatureSpan"></span>grunt-contrib-connect (grunt)](#apidoc.element.grunt-contrib-connect.grunt-contrib-connect)
1.  [function <span class="apidocSignatureSpan">grunt-contrib-connect.</span>toString ()](#apidoc.element.grunt-contrib-connect.toString)



# <a name="apidoc.module.grunt-contrib-connect"></a>[module grunt-contrib-connect](#apidoc.module.grunt-contrib-connect)

#### <a name="apidoc.element.grunt-contrib-connect.grunt-contrib-connect"></a>[function <span class="apidocSignatureSpan"></span>grunt-contrib-connect (grunt)](#apidoc.element.grunt-contrib-connect.grunt-contrib-connect)
- description and source-code
```javascript
grunt-contrib-connect = function (grunt) {
  var path = require('path');
  var connect = require('connect');
  var morgan = require('morgan');
  var serveStatic = require('serve-static');
  var serveIndex = require('serve-index');
  var http = require('http');
  var https = require('https');
  var http2 = require('http2');
  var injectLiveReload = require('connect-livereload');
  var open = require('opn');
  var portscanner = require('portscanner');
  var async = require('async');
  var util = require('util');

  var MAX_PORTS = 30; // Maximum available ports to check after the specified port

  var createDefaultMiddleware = function createDefaultMiddleware(connect, options) {
    var middlewares = [];
    if (!Array.isArray(options.base)) {
      options.base = [options.base];
    }
    // Options for serve-static module. See https://www.npmjs.com/package/serve-static
    var defaultStaticOptions = {};
    var directory = options.directory || options.base[options.base.length - 1];
    options.base.forEach(function(base) {
      // Serve static files.
      var path = base.path || base;
      var staticOptions = base.options || defaultStaticOptions;
      middlewares.push(serveStatic(path, staticOptions));
    });
    // Make directory browse-able.
    middlewares.push(serveIndex(directory.path || directory));
    return middlewares;
  };

  grunt.registerMultiTask('connect', 'Start a connect web server.', function() {
    var done = this.async();
    // Merge task-specific options with these defaults.
    var options = this.options({
      protocol: 'http',
      port: 8000,
      hostname: '0.0.0.0',
      base: '.',
      directory: null,
      keepalive: false,
      debug: false,
      livereload: false,
      open: false,
      useAvailablePort: false,
      onCreateServer: null,
      // if nothing passed, then is set below 'middleware = createDefaultMiddleware.call(this, connect, options);'
      middleware: null
    });

    if (options.protocol !== 'http' && options.protocol !== 'https' && options.protocol !== 'http2') {
      grunt.fatal('protocol option must be \'http\', \'https\' or \'http2\'');
    }

    if (options.protocol === 'http2' && /^0.(?:1|2|3|4|5|6|7|8|9|10|11)\./.test(process.versions.node)) {
      grunt.fatal('can\'t use http2 with node < 0.12, see https://github.com/molnarg/node-http2/issues/101');
    }

    // Connect requires the base path to be absolute.
    if (Array.isArray(options.base)) {
      options.base = options.base.map(function(base) {
        if (base.path) {
          base.path = path.resolve(base.path);
          return base;
        }
        return path.resolve(base);
      });
    } else {
      if (options.base.path) {
        options.base.path = path.resolve(options.base.path);
      } else {
        options.base = path.resolve(options.base);
      }
    }

    // Connect will listen to all interfaces if hostname is null.
    if (options.hostname === '*') {
      options.hostname = '';
    }

    // Connect will listen to ephemeral port if asked
    if (options.port === '?') {
      options.port = 0;
    }

    if (options.onCreateServer && !Array.isArray(options.onCreateServer)) {
      options.onCreateServer = [options.onCreateServer];
    }

    //  The middleware options may be null, an array of middleware objects,
    //  or a factory function that creates an array of middleware objects.
    //  * For a null value, use the default array of middleware
    //  * For a function, include the default array of middleware as the last arg
    //    which enables the function to patch the default middleware without needing to know
    //    the implementation of the default middleware factory function
    var middleware;
    if (options.middleware instanceof Array) {
      middleware = options.middleware;
    } else {
      middleware = createDefaultMiddleware.call(this, connect, options);

      if (typeof options.middleware === 'function') {
        middleware = options.middleware.call(this, connect, options, middleware);
      }
    }

    // If --debug was specified, enable logging.
    if (gr ...
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.grunt-contrib-connect.toString"></a>[function <span class="apidocSignatureSpan">grunt-contrib-connect.</span>toString ()](#apidoc.element.grunt-contrib-connect.toString)
- description and source-code
```javascript
toString = function () {
    return toString;
}
```
- example usage
```shell
...
// Project configuration.
grunt.initConfig({
  connect: {
    server: {
      options: {
        protocol: 'https', // or 'http2'
        port: 8443,
        key: grunt.file.read('server.key').toString(),
        cert: grunt.file.read('server.crt').toString(),
        ca: grunt.file.read('ca.crt').toString()
      },
    },
  },
});
'''
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
