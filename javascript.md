[Home](README.md)

# Javascript
This is an up to date summary of the terms (with links and tidbits) that one needs to know to stay up to date in the fast moving world of JavaScript.

## Preparation
You're going to need a few tools before you get started (I'm assuming MacOS). If you don't yet have **[Homebrew](https://brew.sh/)**, you should first install that:
```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
$ brew update
$ brew docker
> Your system is ready to brew.
```
Next you're going to need to use _Homebrew_ (`$ brew`) to install **[Node.js](https://nodejs.org/en/)** and **[npm](https://www.npmjs.com/)**. Installing _Node.js_ will also install _npm_.
```
$ brew install node
```

## Terminology

### ECMAScript
A language standardised by _[ECMA International](https://www.ecma-international.org)_ and overseen by the _[TC39 (Technical Committee)](https://github.com/tc39)_.

### JavaScript
The commonly used name for the implementation of the ECMAScript standard. This term isn’t directly tied to any one version of the ECMAScript standard.

### ES5 (ECMAScript 5)
The 5th edition of ECMAScript, released in 2009. Almost fully implemented across all modern browsers.
[See ES5 browser support](http://kangax.github.io/compat-table/es5/)

### ES6 or ES2015 (ECMAScript 6 or ECMAScript 2015)
The 6th edition of ECMAScript, released in 2015. Partially implemented in modern browsers.
[See ES6 browser support](http://kangax.github.io/compat-table/es6/)

### ES7 or ES2016 (ECMAScript 2016)
The 7th edition of ECMAScript. I'm not actually sure if it will be referred to as ES7.
[See ES2016 browser support](http://kangax.github.io/compat-table/es2016plus/)

### TypeScript
```
$
```

### CoffeeScript
```
$
```

### [Babel](https://babeljs.io/)
Babel has support for the latest version of JavaScript through syntax transformers. These plugins allow you to use new syntax, right now without waiting for browser support.
```
$ npm install --save-dev babel-cli babel-preset-env
```
You will either need a `.babelrc` file or a `package.json`. A good idea is to use the [interactive setup guide](http://babeljs.io/docs/setup/)

### [Grunt](https://gruntjs.com/)
Grunt is an automation tool and task runner for JavaScript. The configuration is written in JavaScript and is usually saved in a file named `Gruntfile`.
```
$ npm install -g grunt-cli
```

### [Gulp](https://gulpjs.com/)
Gulp is a toolkit for automating painful or time-consuming tasks in your development workflow, so you can stop messing around and build something. 
```
$ npm install -g gulp-cli
$ npm install --save-dev gulp gulp-typescript
```
Your gulp configuration is written in JavaScript and will usually be save in a file named `gulpfile.js`.

### Tsify
```
$
```

### [Browserify](http://browserify.org/)
Browserify lets you require('modules') in the browser by bundling up all of your dependencies.
Browsers don't have the require method defined, but _Node.js_ does. With _Browserify_ you can write code that uses require in the same way that you would use it in _Node.js_.
```
$ npm install -g browserify
```

### Watchify
Watchify starts gulp and keeps it running, incrementally compiling whenever you save a file. This lets you keep an edit-save-refresh cycle going in the browser.
```
$ npm install --save-dev watchify gulp-util
```

### [Babelify](https://github.com/babel/babelify)
Babel is a hugely flexible compiler that converts ES6 (ES2015) and beyond into ES5 and ES3. This lets you add extensive and customized transformations that TypeScript doesn’t support.
```
$ npm install --save-dev babelify
```

### [Uglify](http://lisperator.net/uglifyjs/)
Uglify compacts your code so that it takes less time to download. Uglify will make your code unreadable, so to keep sourcemaps working we also need to install [`vinyl-buffer`](https://www.npmjs.com/package/vinyl-buffer) and [`gulp-sourcemaps`](https://www.npmjs.com/package/gulp-sourcemaps)
```
$ npm install --save-dev gulp-uglify vinyl-buffer gulp-sourcemaps
```
Note, installing `gulp-uglify` will result in installing `uglify-js` as it's a dependency, which is why we don't need to install that independently.
