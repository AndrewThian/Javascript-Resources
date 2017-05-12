As programmers we want to write as little code as possible to minimize errors. Keeping yourself DRY.

### EXAMPLE 1

Example (standard stuff you're used to):
```js
var arr1 = [1,2,3]
var arr2 = []

for (var i = 0; i < arr1.length; i++ ) {
  arr2.push(arr1[i] * 2)
}
```

Example (first class functions):
```js
function mapForEach (arr, fn) {
  var newArr = []
  for (var i = 0; i < arr.length; i++ ) {
    newArr.push(fn(arr[i]))
  }
  return newArr
}

var arr1 = [1,2,3]
var arr2 = mapForEach (arr1, function (ele) {
  return item * 2
})
var arr3 = mapForEach (arr1, function (ele) {
  return item > 2
})

console.log(arr2) // [2,4,6]
console.log(arr3) // [false, false, true]
```

### EXAMPLE 2
Using `.bind` and currying to set the first parameter of the callback function. Remember, `.bind` creates a copy of function, now the new copied function has the preset first parameter of `1`!
```js
var arr1 = [1,2,3]
function mapForEach (arr, fn) {
  var newArr = []
  for (var i = 0; i < arr.length; i++ ) {
    newArr.push(fn(arr[i]))
  }
  return newArr
}

var checkPastLimit = function (limiter, item) {
  /*
  after binding, the function will have a default variable:
  var limiter = 1
  */
  return item > limiter
}

var arr4 = mapForEach(arr1, checkPastLimit.bind(this, 1))
console.log(arr4) // [false, true, true]
```
Lets say I don't want to keep calling `bind` to the functions and I want to simplify my function. All I have to do is create a function factory that return a function for that binds the first parameter.
```js
var checkPastLimitSimplified = function (limiter) {
  return function (limiter, item) {
    return item > limiter
  }.bind(this, limiter) 
}
```
OR I could simple call `.bind` immediately on the function I've set up. But then, the `limiter` parameter is now a hardcoded `1` value!r
```js
var checkPastLimit = function (limiter, item) {
  return item > limiter
}.bind(this, 1)
```