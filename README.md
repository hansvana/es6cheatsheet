# JavaScript ES6 cheatsheet

*Copying isn't cheating*

## Table of Contents

 1. Compiling
 1. Const and let
 1. Classes
 
 
## Compiling

To use our cool new ES6 features, we need to compile back to ES5, which is safe for your parents' browsers.
Check out [ES6 Boilerplate repo](https://github.com/hansvana/es6boiler) for my current build workflow.
 
I use 
  * `Gulp` as the build system
  * `Browserify` to `require('modules')`
  * `Babelify` for a Babel transform on top of Browserify
  * `vinyl-source-stream` and `vinyl-buffer` to get from browserify to gulp
  * `gulp-uglify` to minify the code
  * `sourcemaps` to keep the client-side code debuggable
  * `browser-sync` to keep the browser synced with the code during development*
  
To install all of these: `npm install --save-dev gulp browserify babelify babel-preset-es2015 gulp gulp-uglify gulp-sourcemaps vinyl-source-stream vinyl-buffer browser-sync`

Optionally I often add `gulp-sass` and `bootstrap` for CSS.

* If you're using an editor without a built-in webserver, I recommend adding `lite-server`, which comes with `browser-sync` out of the box. 

## Const and let

ES6 replaces `var` with two new variable keywords:

  * `const` to create an unchangeable binding to a value. This prevents bugs and improves readability. 
  
      ```javascript
      const a = 1;
      a = 2; //error
      ```

      Note that only the binding is fixed, the value can still change:
      ```javascript
      const b = {foo: 1};
      b.foo = 2; //no error
       
      const c = [1,2,3];
      c.push(4); // no error
      ```

  * `let` for when variables need to be reassigned. In practice you should default to `const`, and only use `let` when absolutely necessary.
  
Both `const` and `let` are block-scoped, compared to `var` which is function-scoped.
 
```javascript
function foo() {
    for (let i = 0; i < 10; i++) {
        //...
    }
    
    console.log(i); // undefined, yay! 
}
```
 
## Classes

[https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)

    ```javascript
    class Foo {
      constructor(a, b) {
        this.a = a;
        this.b = b;
      }
    }
    
    const f = new Foo();
    ```