##Dynamic typing
* you don't tell the engine what type of data a variable holds, it figures it out during execution
* static typing - is telling the engine what data you decide to put into a variable
* dynamic typing - allows your use any kind of data in it's data

##Primitive type
* type of data that represents a single value
* 6 types of primitive types = undefined, null(user set non-existent value), boolean, number, string, symbol(used in es6)

##operators
* a special function that is written syntactically differently
* generally take two parameters and returns a result
* 3 + 4 is a infix notation, 3 4+ is a postfix notation, +3 4 is a prefix notation
* so essentially, + - / > are functions that the js engine provides for us

##Precedence
MDN [link](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

* which operator function gets called first
* functions are called in order of precedence(higher precedence wins)

##Associativity
* what order operator functions get called in:
* left-to-right or right-to-lef

```js
var a = 1, b = 2, c = 3

// = operator associativity is right to left.
// so it sets b to equals to c
// then a = b
a = b = c

console.log(a) // 3
console.log(b) // 3
console.log(c) // 3
```

##Coersion
MDN [link](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness) for equality comparison

* javascript engine forces a primitive type into another one
* happens under the hood of the javascript engine due to dynamic types
* equality or inequality (== || !=) tries to coerce values into similar primitive types to equate them
* strict equality or inequality (=== || !==) doesn't try to coerce them and it compares values as the primitive types they are

##Default values
By using operator precedence we can set a function's parameter as a default value if assignment operator returns false

the || operator doesn't just return true or false. It returns the value that can be coersed into true. E.g `'hello' || undefined` would return `'hello'`
```js
function greet(name) {
  name = name || 'Your name here'
  console.log('Hello' + name)
}

greet('Andrew') // hello andrew
greet() // hello your name here
```
using frameworks by setting the global object the name of our library or a string. So for this example, what's happening is that I've already set a variable called libraryName and I'm setting the variable to be the first libraryName variable or a new library
```js
// library 2 file 
window.libraryName = window.libraryName || 'Library 2'
```