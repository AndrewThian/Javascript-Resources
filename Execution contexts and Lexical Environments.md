Javascript's engine has a creation phase that creates an execution context and sets up values for hoisting!

javascript engine has many other arms, the rendering engine, the HTTP requests and they are all running async

Besides the exec stack, the event queue another stack listening for events.

javascript looks at the event queue after the exec stack. Event queue would only be looked at after the execution context

## syntax parsers
* a program that reads yuor code and determines what it is and if the grammar is valid. 
* compiler has a syntax parsers to read the code you're writing

## execution contexts
* there are many lexical environments
* it which one is currently running is managed via execution contexts
* when runs, creates a global object and creates a 'this'

## lexical environments
* where something sits physically in your code
* where you write your code and what's around it

## Objects
* collection of name value pairs

## Global execution
global means not 'inside a function'

* execution context in global context
* global object created, with a global variable called 'this'
* if running in browser, global object is the window object

## Hoisting
* setup memory space for variables and functions via 'hoisting'
* javascript engine has set aside memory space for functions and variables
* function and entirity is added to memory
* variables are assigned a placeholder initially ('undefined')

## errors
* having undefined !== undefined error
* never set a variable to undefined, it's a little dangerous

## Single threaded
* one command is executed one at a time
* javascript behaves in single threaded, but not exactly in the browser because many things are running at the same time

## Synchronous
* excecuted one at a time

## function invocation
* running the function

## Execution stack
* anytime we invoke a function it's placed on the execution stack that runs the execution context
* after the function is run, the execution context is 'popped' off the top of the stack

## variable environments
* each variable is defined in it's own scope

## Scope
* Where a variable is available in your code
* if it's truly the same variable or a new copy

## Scope chain
* searching for outer environments to find anything
* it would keep moving down the execution contexts to find lexical environments

## Asynchronous callbacks
* more than one at a time
* dealing with code that execute other code at the same time
