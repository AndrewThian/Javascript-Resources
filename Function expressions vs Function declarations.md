## Short quiz
```js
// Question 1 ------ 
function foo(){
    function bar() {
        return 3;
    }
    return bar();
    function bar() {
        return 8;
    }
}
console.log(foo());

// Question 2 ------
function foo(){
    var bar = function() {
        return 3;
    };
    return bar();
    var bar = function() {
        return 8;
    };
}
console.log(foo());

// Question 3 ------
console.log(foo());
function foo(){
    var bar = function() {
        return 3;
    };
    return bar();
    var bar = function() {
        return 8;
    };
}

// Question 4 ------
function foo(){
    return bar();
    var bar = function() {
        return 3;
    };
    var bar = function() {
        return 8;
    };
}
console.log(foo());
```
The correct answer is 8, 3, 3, error[bar is not a function]

---

## What is function declaration/statment?
Function declarations are named function variables without requiring variable assignment(`let foo = bar` is variable assignment). It's helpful to think of them as siblings to **Variable Declarations**.

Just as **Variable Declarations** must start with `var`, Function Declarations must begin with `function`.

e.g.
```js
function bar() {
    return 3;
}
```
Function declarations occur as standalone constructs and should not be nested within non-function blocks. What this means is this:
```js
var y = true;
if (y) {
    function example() {
      alert('hi');
      return true;
   }
}
```
Here the function is declared inside a conditional statement, which is fine since `y` is `true`, but if it were false that function would never be declared and when we do want to the call the example function nothing will happen because it was never declared. So it should be: 
```js
function example() {
  "use strict";
  return true;
}
var y = true;
if (y) {
  example();
}
```
Some extra materials:
* Richard Cornford: [Functions Expressions and memory consumption](https://groups.google.com/forum/?hl=en#!msg/comp.lang.javascript/2x5JqxE6oFw/6-ZcylM5tW4J)
* Kangax: [Function statements](https://kangax.github.io/nfe/#function-statements)
* Stackoverflow: [What are the precise semantics of block-level functions in ES6](http://stackoverflow.com/questions/31419897/what-are-the-precise-semantics-of-block-level-functions-in-es6)

---

## What is a Function Expression?
A **Function Expression** defines a function as part of a larger expression syntax(typically a variable assignment). Functions defined via Function Expressions can be named or anonymous. **Function Expression** must not start with `function` (hence the parentheses around IIFE).

e.g.
```js
//anonymous function expression
var a = function() {
    return 3;
}
 
//named function expression
var a = function bar() {
    return 3;
}
 
//immediately invoking function expression || IIFE
(function sayHello() {
    alert("hello!");
})();
```
The function name (if any) is not visible outside of it’s scope (contrast with Function Declarations).

---

## So what's the difference between declaration and statements?
Its sometimes just a pseudonym for a Function Declaration. However as kangax pointed out, in mozilla a Function Statement is an extension of Function Declaration allowing the Function Declaration syntax to be used anywhere a statement is allowed.  It’s as yet non standard so not recommended for production development.

---

## What is Hoisting?
Function declarations are always moved(hoisted) to the top of their javascript scope by the Javascript engine. 

## Question 1
When a function declaration is hoisted the entire function body is "lifted" with it, so after the engine/interpreter has finished with the code in Question 1, it runs like this: 
```js
function foo(){
    //define bar once
    function bar() {
        return 3;
    }
    //redefine it
    function bar() {
        return 8;
    }
    //return its invocation
    return bar(); //8
}
alert(foo());
```

You must be thinking, we were always taught that code after a return statement is unreachable!? WTF is going on here?!

In JavaScript execution there is Context (which ECMA 5 breaks into **LexicalEnvironment**, **VariableEnvironment** and **ThisBinding**) and Process (a set of statements to be invoked in sequence). Declarations contribute to the VariableEnvironment when the execution scope is entered. They are distinct from Statements (such as return) and are not subject to their rules of process.

Simply put, the javascript engine sets up the function `bar` first(hoist) then returns it even if it's after the `return` in its execution context.

---

## Question 2
So this is a function expression:
```js
var bar = function() {
    return 3;
};
```
However, function expressions have a caveat. On the left hand side is a Variable Declaration. Variable Declarations get hoisted but their Assignment Expressions don't. So when bar is hoisted the interpreter initially sets up `var bar = undefined`. The function definition itself will not be hoisted. 

Thus, the code in question 2 runs more like this: 
```js
//**Simulated processing sequence for Question 2**
function foo(){
    //a declaration for each function expression
    var bar = undefined;
    var bar = undefined;
    //first Function Expression is executed
    bar = function() {
        return 3;
    };
    // Function created by first Function Expression is invoked
    return bar();
    // second Function Expression unreachable
}
alert(foo()); //3
```

---

## Question 3
*Small caveat here, running in firebug console doesn't not practice function hoisting when it runs in it's "global" scope. Reasons for qutoes is because firebug is not actually global but a special "Firebug" scope.*

The `foo` function gets hoisted but the second function expression of `bar` is not as we know function expressions aren't hoisted!

---

## Question 4
Almost. If there were no hoisting at all, the TypeError would be “bar not defined” and not “bar not a function”. There’s no function hoisting, however there is variable hoisting. Thus bar gets declared up front but its value is not defined. Everything else runs to order.

Code would be executed like this:
```js
//**Simulated processing sequence for Question 4**
function foo(){
    //a declaration for each function expression
    var bar = undefined;
    var bar = undefined;
    return bar(); //TypeError: "bar not defined"
    //neither Function Expression is reached
}
alert(foo());
```

## Reasons to favour Function Expressions?
a) Function Declarations feel like they were intended to mimic Java style method declarations but Java methods are very different animals. In JavaScript functions are living objects with values. Java methods are just metadata storage. Both the following snippets define functions but only the Function Expression suggests that we are creating an object.

```js
//Function Declaration
function add(a,b) {return a + b};
//Function Expression
var add = function(a,b) {return a + b};
```

b) Function Expressions are more versatile. A Function Declaration can only exist as a “statement” in isolation. All it can do is create an object variable parented by its current scope. In contrast a Function Expression (by definition) is part of a larger construct. If you want to create an anonymous function or assign a function to a prototype or as a property of some other object you need a Function Expression. Whenever you create a new function using a high order application such as curry or compose you are using a Function Expression. Function Expressions and Functional Programming are inseparable.

## Additional reading materials and references
Javascript web blog - [currying](https://javascriptweblog.wordpress.com/2010/04/05/curry-cooking-up-tastier-functions/)
Javascript web blog - [function declarations vs function expressions](https://javascriptweblog.wordpress.com/2010/07/06/function-declarations-vs-function-expressions/)
Javascript web blog - [composing](https://javascriptweblog.wordpress.com/2010/04/14/compose-functions-as-building-blocks/)