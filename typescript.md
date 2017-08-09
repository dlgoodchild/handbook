[Home](README.md)

# TypeScript

## Modules and Module Loaders
Modules are executed within their own scope, not in the global scope; this means that variables, functions, classes, etc. declared in a module are not visible outside the module unless they are explicitly exported.  
Any declaration (such as a variable, function, class, type alias, or interface) can be exported by adding the `export` keyword.

> In TypeScript, just as in ECMAScript 2015, any file containing a top-level `import` or `export` is considered a module.

Depending on the module target specified during compilation, the compiler will generate appropriate code for
* Node.js (CommonJS)  
_Suited only to server-side applications_
* Require.js (AMD)  
_Suited to client-side applications_
* Isomorphic (UMD)  
_JavaScript applications that can run both client-side and server-side_
* SystemJS  
_Suited to client-side applications_
* ECMAScript 2015 native modules (ES6) module-loading systems.

Both CommonJS and AMD generally have the concept of an `exports` object which contains all exports from a module (and using `module = "none"` has a similar result).  
e.g. `exports.MyClass = MyClass;`

In the `tsconfig.json` the `module` property can have any of the following values: `none`, `amd` `commonjs`, `system`, `umd`, `es6`, `es2015`, or `esnext`  
  
Note: Only `amd` and `system` can be used in conjunction with `--outFile`. `es6` and `es2015` may be used when targeting `es5` or lower. I also found a good article that covers many reasons why using an "outfile" can be potentially bad, this is not to say you shouldn't use it, but just be aware of the potential problems ahead if you do.

["Why --outfile is BAD"](https://basarat.gitbooks.io/typescript/content/docs/tips/outFile.html)  
_Keep in mind you can achieve the "bundled" outfile result using a build tool such as gulp_

### [SystemJS](https://github.com/systemjs/systemjs)

WIP

```
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/systemjs/0.20.17/system.js"></script>
```

#### Transpiler
To use SystemJS with the transpiler option, you will need to include typescript as well, that **comes at a cost of 1.4MB**.

**Also, I don't know why, but including, via cdnjs.couldflare.com, typescript 2.4.2 and SystemJS 0.20.17 does not seem to work, but does when I include from code.angularjs.org**

The following would assume that the `.ts` files have not been processed or compiled to `.js` and are available in a `scripts` directory on the root.
```
<script src="https://code.angularjs.org/tools/system.js"></script>
<script src="https://code.angularjs.org/tools/typescript.js"></script>

<!-- Unknown to me, the following doesn't work:
<script src="https://cdnjs.cloudflare.com/ajax/libs/systemjs/0.20.17/system.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/typescript/2.4.2/typescript.js"></script>
-->

<script>
  System.config({
    //baseURL: 'scripts', //for convenience
    map: {
      app: '/some/long/app/path/'
    }
    transpiler: 'typescript',
    packages: {
      scripts: {
        defaultExtension: 'ts'
      }
    }
  });

  System
    .import('scripts/init') // no extension required because we used "defaultExtension"
    .then(function(module) {
        console.log(new module.MyClass)
      }, console.error.bind(console)
    );
</script>
```

#### Without Transpiler
```
<script src="https://cdnjs.cloudflare.com/ajax/libs/systemjs/0.20.17/system.js"></script>

<script>
  System.config({
    packages: {
      scripts: {
        defaultExtension: 'js'
      }
    }
  });

  System
    .import('scripts/init') // no extension required because we used "defaultExtension"
    .then(function(module) {
        console.log(new module.MyClass)
      }, console.error.bind(console)
    );
</script>
```


### [RequireJS](http://requirejs.org/)
_RequireJS is considered old, mostly deprecated. 17.13kB minified_

> RequireJS is a JavaScript file and module loader. It is optimized for in-browser use, but it can be used in other JavaScript environments such as Node.js.  
>
> RequireJS tries to keep with the spirit of CommonJS, with using string names to refer to dependencies, and to avoid modules defining global objects, but still allow coding a module format that works well natively in the browser. RequireJS implements the Asynchronous Module Definition (formerly Transport/C) proposal.

If you wish to use requirejs, then you should use `module = "amd"` in your `tsconfig.json`. You can optionally bundle all the files into a single `.js` file using something like `outFile = "./dist/bundle.js"`.

How you load your files however changes depending on if you use a single output file or not.

#### Bundled
```
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.3.4/require.min.js"></script>
<script type="text/javascript" src="dist/bundle.js"></script>
...
<script type="text/javascript">
  require( ["myclass"], function( export ) {
    console.log( new export.MyClass() ); // indirect access to the loaded class.
  });
</script>
```

#### Individual
```
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.3.4/require.min.js"></script>
...
<script type="text/javascript">
  requirejs(["dist/myclass", "dist/anotherclass"], function(myclass, anotherclass) {
    console.log(new myclass.MyClass()); // again, indirect access to the exported/loaded classes
    console.log(new another.AnotherClass());
  });
</script>
```

#### Loading via Automated Config
First manually create your `require.config.js` file, here's some sample content
```
requirejs.config({
  paths: {
    "dist": "some/long/path/to/compiled/scripts"
  },
  deps: ["dist/baseclass"], // automatically load some dependencies (baseclass.js)
  callback: function() { // remember if you're using es6, can use: () => { ... }
    requirejs(["dist/myclass"], function (myclass) {
      console.log(new myclass.MyClass());
    });
  }
});
```
The idea here is you may have multiple paths and/or that they may be wordy and long, this allows you to configure named paths, and have pre-autoloaded dependencies.

Then, in your `.html` file, you'll use a script tag with the `data-main` attribute pointing to the new `config.js` file:
```
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.3.4/require.min.js" data-main="config.js"></script>
```

### [Almond](https://github.com/requirejs/almond)
From the horses mouth:
> A replacement AMD loader for RequireJS. It provides a minimal AMD API footprint that includes loader plugin support. Only useful for built/bundled AMD modules, does not do dynamic loading.
Why
>
> Some developers like to use the AMD API to code modular JavaScript, but after doing an optimized build, they do not want to include a full AMD loader like RequireJS, since they do not need all that functionality. Some use cases, like mobile, are very sensitive to file sizes.
>
>By including almond in the built file, there is no need for RequireJS. almond is around 1 kilobyte when minified with Closure Compiler and gzipped.

### [CommonJS](http://www.commonjs.org/)
A loader specifically for Node.js (i.e. outside the browser). This can be used in the browser with the correct tooling.

(TODO)

### [Webpack](https://webpack.js.org/)
_Webpack is like the new kid on the block_

(TODO)

### [RollupJS](https://rollupjs.org/)
Yet another module bundler. Rollup can import existing CommonJS modules, but using a plugin.  
I'm not even sure I want to research this one. I can't see what benefit it brings over the others.


## Loading Files
You can use `tsc` to compile your `.ts` files to `.js` files, however you have to pay attention to the `module` compiler option.

## Notes
I have confirmed that by not using top-level `import` statements combined with optionally using `namespace` you can obtain a simple compilation result in the form of:
```
var MyClass;
(function (MyClass) {
  ...
})(MyClass || (MyClass = {}));
```
What this means is, that if you use a single-file output in your pipeline tool (such as gulp) you can have a class collection directly available by simply loading the bundle without a module loader:
```
<script src="script/my-class-bundle-using-namespaces-and-no-imports.js"></script>
```
Lets take the typscript:
```
class MyClass {
  constructor() {
    console.log("Constructed MyClass")
  }
}
```
If you transpile this using any of the `module` configuration options, you will always get a simple result suitable for direct import using a `<script>` tag. E.g.:
```
var MyClass = (function () {
    function MyClass() {
        console.log("Constructed MyClass");
    }
    return MyClass;
}());
```
As soon as you add an `export` or `import` statement like `import { OtherClass } from './otherclass'` the generated source is immediately changed, and similar output occurs:

#### amd
```
define(["require", "exports"], function (require, exports) {
  "use strict";
  Object.defineProperty(exports, "__esModule", { value: true });
```

#### umd
_Craziness._

#### system
```
System.register([], function (exports_1, context_1) {
  "use strict";
  var __moduleName = context_1 && context_1.id;
```

#### commonjs, none
```
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
```
and the `export` statement would also add:
```
exports.MyClass = MyClass
```

## So, what does all this mean?
Well, we need the `import` statments if we want the library to work with `node.js` and being able to include class dependencies just as you would in any other language.

However, for the web browser, it's a different story.

Adding the top-level `import` and `export` statements tends to cause unnecessary complications when you simply want to leverage the collection of classes within a browser-based script. I.e. imagine I just want to inline a piece of script and include my TypeScript library, I should simply be able to run a `gulp` task that compiles all the typescript into simple classes without any of the module loading complexities, much in the same way as we do with jQuery.

So, how do we solve this, and simplify it? Right now, I'm not sure yet, still working on that.

## API specifications

### [AMD](https://github.com/amdjs/amdjs-api)
Asynchronous Module Definition

### [UMD](https://github.com/umdjs/umd)
Universal Module Definition
