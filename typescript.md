[Home](README.md)

# TypeScript

## Modules and Module Loaders
Modules are executed within their own scope, not in the global scope; this means that variables, functions, classes, etc. declared in a module are not visible outside the module unless they are explicitly exported.  
Any declaration (such as a variable, function, class, type alias, or interface) can be exported by adding the `export` keyword.

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
Note: Only `amd` and `system` can be used in conjunction with `--outFile`. `es6` and `es2015` may be used when targeting `es5` or lower.

### [SystemJS](https://github.com/systemjs/systemjs)

WIP

```
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/systemjs/0.20.17/system.js"></script>
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


## API specifications

### [AMD](https://github.com/amdjs/amdjs-api)
Asynchronous Module Definition

### [UMD](https://github.com/umdjs/umd)
Universal Module Definition
