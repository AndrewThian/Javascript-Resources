## Closures
Below is a code snippet of how closures work:
```js
function greet (a) {
  return function (name) {
    console.log(a + ' ' + name)
  }
}
let sayHi = greet('hi')
sayHi('Andrew')
```
what happens here is a couple of things:
1. A global execution context is created
2. the function `greet` is run with its own execution context and popped off the execution stack.
3. the argument `a` is still store somewhere in memory
4. `sayHi` variable is run and it's execution context is created. 
5. javascript goes up the outer lexical environment to find the variable `a`. The `sayHi` still has access to the previous `greet` variables. 

Closures are a feature that javascript has to ensure the current execution context has access to lexcial environments that affect it.

Here's another example of closures:
```js
function buildFunctions () {
  var arr = []
  for (var i = 0; i < 3; i++) {
    arr.push(function () {
      console.log(i)
    })
  }

  return arr
}

var fs = buildFunctions()

fs[0]() // 3
fs[1]() // 3
fs[2]() // 3
```
what happens here is:
1. global execution context of `buildFunctions`
2. `buildFunctions` runs and creates all the functions
3. the memory of `i === 3` in memory
4. when we execute `fs[0]()` function it calls the memory of `i` which at the point of calling, is `3`

An analogy would be like; **3 children being asked what their father's age is. They wouldn't tell you what the age of their father was at when they were born but the CURRENT age of the father now.**

Here's another example of how to use ES5 to save the value of `i`, I need a seperate execution context with a IIFE(immediately invoked function expression)!
```js
function buildFunctions () {
  var arr = []
  for (var i = 0; i < 3; i++) {
    arr.push((function (j) {
      return function () {
        console.log(j)
      }
    }(i))
    )
  }

  return arr
}

var fs = buildFunctions()

fs[0]()
fs[1]()
fs[2]()
```
Each time the for loop is run, I'm creating a new execution context for `j` which I know would persist after the function the popped of the execution stack.

## Function factories
By making use of closures like before, the following example:
```js
function makeGreeting (language) {
  return function (firstname, lastname) {
    if (language === 'en') {
      console.log('Hello ' + firstname + ' ' + lastname)
    }
    if (language === 'es') {
      console.log('Hola ' + firstname + ' ' + lastname)
    }
  }
}

var greetEnglish = makeGreeting('en')
var greetSpanish = makeGreeting('es')

greetEnglish('John', 'Doe')
greetSpanish('John', 'Doe')
```
Things to note what's actually happening in the javascript engine is: 
1. the `makeGreeting` function is making a new execution context with the variable language of the passed in parameters('en' or 'es')
2. when calling the `greetEnglish` or `greetSpanish` functions, it looks for the variable langugage based on the closures of the function factory of `makeGreeting`.
3. The javascript engine knows the returned function with the language variable that was it was created with.
4. The key is realising that each function called will get it's own execution and each function called WITHIN would point to that previous execution context. 

## Closures and Callbacks
```js
function sayHiLater () {
  var greeting = 'Hi!'
  setTimeout(function () {
    console.log(greeting)
  },3000)
}

sayHiLater()
```
Things to take note what's happening here: 
1. We're using closures here because `sayHiLater` runs and finishes before `setTimeout` runs. Javascript then knows what the previously defined `greeting` variable due to closures.
2. During the event loop, it finds the `greeting` variable then it runs. 

** CALLBACK - A function you give to another function to be run when the other function is finished. So the function you execute, calls back the other function.**