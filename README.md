# JavaScript ES6 cheatsheet

*Copying isn't cheating*


## Better guides

For more in-depth information

  * [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) The Holy Grail of 'how to do JavaScript', as far as I'm concerned.
  * [ES6-features.org](http://es6-features.org/#Constants) All the new bits, compared to the old bits.

## Table of Contents

 1. [Compiling](#compiling)
 1. [Const and let](#const-and-let)
 1. [Classes](#classes)
 1. [Arrow Functions](#arrow-functions)
 1. [Array Methods](#array-methods)
 
## Compiling

To use our cool new ES6 features, we need to compile back to ES5, which is safe for your parents' browsers.
Check out my [ES6 Boilerplate repo](https://github.com/hansvana/es6boiler) for my current build workflow.
 
I use 
  * `Gulp` as the build system
  * `Browserify` to `require('modules')`
  * `Babelify` for a Babel transform on top of Browserify
  * `vinyl-source-stream` and `vinyl-buffer` to get from browserify to gulp
  * `gulp-uglify` to minify the code
  * `sourcemaps` to keep the client-side code debuggable
  * `browser-sync` to keep the browser synced with the code during development<sup>1</sup>
  
To install all of these: `npm install --save-dev gulp browserify babelify babel-preset-es2015 gulp gulp-uglify gulp-sourcemaps vinyl-source-stream vinyl-buffer browser-sync`

Optionally I often add `gulp-sass` and `bootstrap` for CSS.

<sup>1</sup> If you're using an editor without a built-in webserver, I recommend adding `lite-server`, which comes with `browser-sync` out of the box. 

## Const and let

ES6 replaces `var` with two new variable keywords:

  * [`const`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Statements/const) to create an unchangeable binding to a value. This prevents bugs and improves readability. 
  
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

  * [`let`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Statements/let) for when variables need to be reassigned. In practice you should default to `const`, and only use `let` when absolutely necessary.
  
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

[`Classes`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) are JavaScript objects, with some sugar sprinkled on top to make them behave like classes in other languages.
Internal properties and methods are referred to with `this`.

```javascript
class Foo {
  constructor(a, b) {
    this.a = a;
    this.b = b;
    this.bar();
  }
  
  bar() {
    this.a++;
  }
}

const f = new Foo();
```

Internal properties should not be accessed directly from outside, but rather through methods. Optionally you can use `getters` and `setters`. 

```javascript
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
  
  move(speed) {
    this.x += speed;
  }
  
  get pos() {
    return {x: this.x, y: this.y};
  }
  
  set pos(newPos) {
    this.x = newPos.x;
    this.y = newPos.y;
  }  
}

const p = new Point(12,34);
p.move(10);
const baz = p.pos;
p.pos = {x: 56, y: 78};
```

## Arrow functions

Anonymous functions are great for when you want to create a function that doesn't need to be accessed outside of its immediate declaration.

```javascript
window.addEventListener('load', function() { 
    // I am an anonymous function
});

setTimeout(function() { 
   // I am an anonymous function
}, 1000);

requestAnimationFrame(function() { 
  // I am an anonymous function
});
```

However, they create problems when used alongside `this`. 

```javascript
class Foo {
  constructor() {
    this.a = 1;
    
    window.addEventListener('load', function() { 
        console.log(this) // here `this` refers to window, not the Foo object
    });
  }
}
```

Enter the [`arrow function`](https://www.sitepoint.com/es6-arrow-functions-new-fat-concise-syntax-javascript/), which has a much more intuitive handling of `this`.

```javascript
class Foo {
  constructor() {
    this.a = 1;
    
    window.addEventListener('load', () => { 
        console.log(this) // now `this` refers to the Foo object 
    });
  }
}
```

## Array Methods

Most of these aren't new to ES6, but still awesome to have. As you can see we use arrow functions with most of them.
There are many more cool methods. Check [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) for a complete list.

```javascript
const myArray = [0, 1, 2, 3, 5, 8, 13, 21, 34];
```

#### add and remove items

```javascript
newArray.push( item ); // add a new item to the end of an Array
newArray.unshift( item ); // add a new item to the front of an Array
newArray.pop(); // remove the last item of an Array
newArray.shift(); //remove the first item of an Array
```

#### filter
Filtering out values smaller than 10.

```javascript
const newArray = myArray.filter( item => {
  return item >= 10;
});

// newArray = [13, 21, 34]
```

#### find
Returns the first value that matches your statement, if nothing is found `undefined` is returned.

```javascript
const foundItem = myArray.find( item => {
  return item >= 10;
});

// foundItem = 13
```

#### findIndex
Returns the index of the first value that matches your statement, if nothing is found `-1` is returned.

```javascript
const foundIndex = myArray.findIndex( item => {
  return item >= 10;
});

// findIndex = 6
```

#### forEach
Executes a function for each array element.

```javascript
myArray.forEach( item => {
  console.log(item >= 10);
});

// 0
// 1
// ...
```

#### map
Executes a function for each Array element, and returns a new array with the results.

```javascript
const newArray = myArray.forEach( item => {
  return item + 1;
});

// newArray = [1, 2, 3, 4, 6, 9, 14, 22, 35]
```

