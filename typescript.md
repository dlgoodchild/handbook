[Home](README.md)

# TypeScript

## Modules and Module Loaders
Modules are executed within their own scope, not in the global scope; this means that variables, functions, classes, etc. declared in a module are not visible outside the module unless they are explicitly exported.
Any declaration (such as a variable, function, class, type alias, or interface) can be exported by adding the `export` keyword.

Depending on the module target specified during compilation, the compiler will generate appropriate code for Node.js (CommonJS), require.js (AMD/Asynchronous Module Definition), isomorphic (UMD), SystemJS, or ECMAScript 2015 native modules (ES6) module-loading systems.

Both CommonJS and AMD generally have the concept of an `exports` object which contains all exports from a module (and using `module = "none"` has a similar result).
e.g. `exports.MyClass = MyClass;`

In the `tsconfig.json` the `module` property can have any of the following values: `none`, `amd` `commonjs`, `system`, `umd`, `es6`, `es2015`, or `esnext`
Note: Only `amd` and `system` can be used in conjunction with `--outFile`. `es6` and `es2015` may be used when targeting `es5` or lower.

### [SystemJS](https://github.com/systemjs/systemjs)

### [RequireJS](http://requirejs.org/)
RequireJS is a JavaScript file and module loader. It is optimized for in-browser use, but it can be used in other JavaScript environments such as Node.js.  

RequireJS tries to keep with the spirit of CommonJS, with using string names to refer to dependencies, and to avoid modules defining global objects, but still allow coding a module format that works well natively in the browser. RequireJS implements the Asynchronous Module Definition (formerly Transport/C) proposal.

If you wish to use requirejs, then you should use `module = "amd"` in your `tsconfig.json`.

### [CommonJS](http://www.commonjs.org/)
A loader specifically for Node.js (i.e. outside the browser). This can be used in the browser with the correct tooling (TODO).

### [AMD](https://github.com/amdjs/amdjs-api)

## Loading Files
You can use `tsc` to compile your `.ts` files to `.js` files, however you have to pay attention to the `module` compiler option.
