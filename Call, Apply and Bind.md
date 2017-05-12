All functions in javascript have access to the `call`, `apply` and `bind` method.

Difference between `call`, `apply` and `bind`. (Object Oriented Javascript)

Basically, if I have OBJECT1(with properties and methods) and OBJECT2(with propertoes and methods). I can use `call`, `apply` or `bind` to add methods to the previously mentioned objects.

## Example of usage
### `.bind`
```js
var person = {
  firstname: 'John',
  lastname: 'Doe',
  getFullName: function () {
    var fullname = this.firstname + ' ' + this.lastname
    return fullname
  }
}

/*
this function is not a method of the person object.
the this keyword points to the global object and I'm unable to invoke the function.
*/
var logName = function (lang1, lang2) {
  console.log('Logged: ' + this.getFullName())
}

/*
Passing the function object an OBJECT I wish to bind it to.
the .bind method returns a NEW function and it makes a copy of logName function.
.bind(person) now points to the person object.
*/
var logPersonName = logName.bind(person)

logPersonName()
```
I can also create the function on the fly and call `.bind` to that function. 
```js
var logName = function (lang1, lang2) {
  console.log('Logged: ' + this.getFullName())
}.bind(person)

logName()
```
It's a copy of the prior function and it only deals with the `this` variable. I can still log my arguments and access them. 
```js
var logName = function (lang1, lang2) {
  console.log('Logged: ' + this.getFullName())
  console.log('Arguments: ' + lang1 + ' ' + lang2)
}.bind(person)

logName('en', 'es') // LOGS Arguments: en es
```

### `.call` and `.apply`
Unlike `.bind` that creates a copy of the function, `.call` actually executes the function as such. `.call` allows me to decide what object to pass into the `this` keyword. 

The difference between `.apply` and `.call`, `.apply` requires an array for your arguments rather than just multiple arguments
```js
var person = {
  firstname: 'John',
  lastname: 'Doe',
  getFullName: function () {
    var fullname = this.firstname + ' ' + this.lastname
    return fullname
  }
}

var logName = function (lang1, lang2) {
  console.log('Logged: ' + this.getFullName())
  console.log('Arguments: ' + lang1 + ' ' + lang2)
}

// .call just executes the function without creating a copy.
// .apply requires an array for arguments.
logName.call(person, 'en', 'es')
logName.apply(person, ['en', 'es'])
```
I can even do this on the fly. By using IIFE(immediately invoked function expressions), I can invoke them with `.apply` or `.call`
```js
(function (lang1, lang2) {
  console.log('Logged: ' + this.getFullName())
  console.log('Arguments: ' + lang1 + ' ' + lang2)
}).apply(person, ['es', 'en'])

(function (lang1, lang2) {
  console.log('Logged: ' + this.getFullName())
  console.log('Arguments: ' + lang1 + ' ' + lang2)
}).call(person, 'es', 'en')
```

## Function borrowing
I can use function borrowing in real life examples! Here I have another person object with pretty much the same properties but I don't have a full name method.
```js
var person2 = {
  firstname: 'Jane',
  lastname: 'Doe'
}

// I take the first object with the getFullName method.
// invoking with .apply and setting the this keyword to the person2 object!
person.getFullName.apply(person2) // LOGS 'Jane Doe'
// I've successfully 'borrowed' a function from person object and passed in data from person2 object. 
// AS LONG AS YOU HAVE SIMILAR PROPERTY NAMES
```

## Currying
Creating a copy of a function but with some preset paramenters. Very good usage in mathematical operations!

with `.bind` you're creating a copy of the function.
It's simple, what we're doing is setting up the permanent first parameter of the function! 

*ANALOGY - You're cloning a human being and setting up their genetic code to always speak in doubles. Now whenever you talk to this cloned human being, they always speak in doubles. Regardless of what you tell them.*
```js
function multiply (a, b) {
  return a * b
}

var multiplyByTwo = multiply.bind(this, 2)

multiplyByTwo(2) // 4
multiplyByTwo(5) // 10
multiplyByTwo(10) // 20
```
1. multiplyByTwo is creating a copy of the multiply function.
2. since I'm not executing the function, giving parameters sets up the permanent value of the first parameter.

Below is what `.bind` is doing behind the scenes.

```js
function multiplyByTwo (b) {
  var a = 2
  return a * b
}
```
So what happens if I pass MORE parameters?
```js
function multiply (a, b) {
  return a * b
}

// passing no parameters, I will have both a & b open.
var multiplyByTwo = multiply.bind(this, 2)
// passing both parameters, I'm essentially telling bind to set both a and b ALWAYS
// if I pass any additional parameters, it will still end up as 4.
var multiplyByTwo = multiply.bind(this, 2, 2)
```
---

## Another example of usage
Here's another example based on a youtube video from techsith.

### `.call`
I'm telling `.call` to use the `obj` and it's values with the function `addToThis`.

```js
var obj = { num: 2 }

var addToThis = function (value1, value2, value3) {
  return this.num + value1 + value2 + value3 
}

// functionName.call(obj, value1, value2, value3)
addToThis.call(obj, 1, 2, 3)
```

### `.apply`
Basically, `.apply` works similar to `.call` but I can pass an array for arguments instead of listing them down one by one.

```js
var obj = { num: 2 }

var addToThis = function (value1, value2, value3) {
  return this.num + value1 + value2 + value3
}

var array = [1,2,3]
// functionName.call(obj, value1, value2, value3)
addToThis.apply(obj, array)
```

### `.bind`
It's different from `.call` or `.apply` as it doesn't execute the function and it returns a function.

What it does is `.bind` is binding an object to the function and now I can execute the bound function from a variable. Shown in the example below.

```js
var obj = { num: 2 }

var addToThis = function (value1, value2, value3) {
  return this.num + value1 + value2 + value3
}

var bound = addToThis.bind(obj)
// Now I can call bound as a function expression.
bound(1,2,3)
```