## Objects and functions
peferred approach to use the dot operator than the computed number access operator. Reasons being - unless you need to access a object with a dynamic string

* objects can have primitive type property, another collection of KVP and methods(functions)
* creating new objects - adding properties - accessing properties **computed member access operator** ( [] )
* dot operator **member access operator** for accessing properties - looking for members in an object

---

## Objects and object literals
Object literals are = {} (not an operator)
javascript engine parsing the syntax, comes across this, assumes that I'm creating an object.

* Object literal synatx - name and values seperated by colons

---

## Namespaces
* Container for variables and functions
* typically to keep variables and functions with the same name seperate

We can fake namespaces by using object literals as such
```js
var english = {}
var spanish = {}

english.greet = 'hello'
spanish.greet = 'hola'

english.greetings = {
  greet: {
    basic: 'hello'
  }
}

console.log(english.greetings.greet.basic)
```

---

## JSON 
Javascript object notation - inspired by object literal notations.
Sending data through the internet was using XML, people looked at javascript object synatax
Now we send data via JSON! Difference though: 
* properties need to wrapped in JSON
* JSON is a subset of javascript object notation

```js
JSON.stringify // changes javascript objects into JSON
JSON.parse // parses strings into objects
```

---

## First class functions
**Everything you can do with other types you can do with functions. Assign them to variables, pass them around, create them on the fly.**

This is why callback functions are possible. As you can pass functions into parameters of other fuctions.

Functions are special type of object. Can attach properties and methods to it: Primitive, object, function. 

Function property: name(can be optional), code(invocable)

I can add properties to a function like this:
```js
function greet () {
  console.log('hi');
}

greet.language = 'English'
greet() // 'hi'
// I can attach properties to functions!
console.log(greet.language) // 'English'
```

---

## Expression
A unit of code that results in a value, it doesn't have to save to a variable
```js
a = 3 // equals operator returns a value
1 + 2 // would return a value
```

##Function statement and function expressions
Function statement:
```js
function greet () {
  console.log('hi')
}
```
It's put into memory, it's been 'hoisted'

Function expression:
```js
var greet = function () {
  console.log('hi')
}
```
Added to the variable's address.
Function expression are anonymous functions(no name property)
Function expressions will run into problems with hoisting as JS engine sets variables to undefined when in creation phase.
The bottom example will not work:
```js
greet()

var greet = function () {
  console.log('hi')
}
```
*Read more in this [note]([Function expressions vs Function declarations](quiver:///notes/4BC8B622-DFF4-4774-9ADA-909B4BD9AB36)..*

---

## By Value and By reference
Primitive types(numbers, strings..) are BY VALUE and Objects are BY REFERENCE
Setting a primitive type to a variable(a) then pointing another variable(b) to it would create a copy of the variable(a)
```js
var a = 3
var b = a
a = 2 // b = 3, a = 2
```
While, setting an object to a variable and another, both would point to the same value. They are pointing at the same location in memory. Basically, these variables are aliases.
```js
var c = { greeting: 'hi' }
var d = c
c.greeting = 'hello'// c = { greeting: 'hello' }, d = { greeting: 'hello' }
```
Passing an object to function parameters, they are passed by reference as well!

Equals operator sets up new memory space!

---

## Objects, functions and 'this'
Objects sitting in memory has a code property with a code list. Execution context created and added to the execution stack.

An function when run has 3 things:
1. Variable environment where function variables live. 
2. Outer lexical environments
3. `this` - it would pointing to different objects depending on how the function is being invoked.

Whenever you create a function(be it function statements or function expressions), the `this` keyword points to the global object.

In a case the function is attached to an object(a object method), the `this` keyword refers to the object it is sitting inside of. By default, `this` keyword calls the global window object.

**ES5 problem with `this`**: if I run the code(object method) as show below, I know that the `this` keyword would be appended to the global window object
```js
var c = {
  name: 'hello',
  log: function () {
    // this keyword now points to the object
    this.name = 'Update sia'
    console.log(this)

    // this internal function, when it's execution context is created, it's pointed to the GLOBAL OBJECT. 
    var setname = function (newname) {
      this.name = newname
    }
    setname('update sia 2')
    console.log(this)
  }
}

c.log()
```
To deal with this, we can define a variable with the value `this` above the function within the method. What happens below is javascript looks for the keyword `self` and doesn't find it. Then moves up the lexical environments to find a definition. There it finds the variable self with the value of `this`. 
```js
var c = {
  name: 'hello',
  log: function () {
    var self = this
    self.name = 'Update sia'
    // runs the function and appends the name to the global window object.
    var setname = function (name) {
      self.name = name
    }
    setname('update sia 2').bind(log)
    console.log(self)
  }
}

c.log()
```

---

## Arrays in javascript
Javascript is dynamically typed and you're allowed to place various types in an array.

---

## Arguments and spread
*spread is an es6 feature*
When you run functions in javascript it has 4 things: 
1. Variable environment where function variables live. 
2. Outer lexical environments
3. `this` - it would pointing to different objects depending on how the function is being invoked.
4. Arguments **

When a function is created, hoisting sets up the arguments as undefined first. Therefore, we can skip various arguments in the function. 

Javascript has a reserved keyword called `arguments` if called within a function, gives back all the available parameters in the function. However it's going to become deprecated as it's not the best way to find parameters.
```js
function a (arg1, arg2, arg3) {
  console.log(arguments) // gives me ['hello', '2', 3] (an array like object)
}
a('hello', '2', 3)
```

---


## Immediately invoked function expressions
Immediately invoking the function: what is does basically is create a function and invoke it straight away. 
```js
var greeting = function (name) {
  console.log('hello' + name)
}()

// below is a function expression because the syntax parser doesn't anticipate the anon function to have a name
(function(a) {
  return 'hello'
})
```
By wrapping my code in the paratheses, I wrap my code in an IIFE to make sure I'm not colliding with the global execution context. However, I can pass in a global object to purposefully affect the execution context. As such: 
```js
var greeting = 'Hola';

(function (name) {
  var greeting = 'hello'
  console.log(greeting + name)
})('john')

console.log(greeting)
// prints: hello john
// prints: hola
```
As we can see, it prints `hello john` in the IIFE while `hola` as the global execution context. 
```js
var greeting = 'Hola';

(function (global, name) {
  var greeting = 'hello'
  global.greeting = greeting
  console.log(greeting + name)
})(window, 'john')

console.log(greeting)
// prints: hello john
// prints: hello
```
Now I'm purposefully forcing the global namespace of `greeting` to be `"hello"`. 