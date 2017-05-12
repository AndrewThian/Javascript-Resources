Promise libraries such as q and bluebird

##Two concepts for promises
* Deferred Object - interface for the progress bar
* Promise Object - progress bar

##Deferred object
* The deferred object has a property on it called promise.
* The promise property has two properties -> 1) status (default: pending) 2) value (default: undefined)

##Promised object
* It's kinda like a progress bar. The task has not finished yet but you want to write logic around the object.
* How to see the promise object in chrome console: 

```js 
function delay () {
	return new Promise( (resolve, reject) => {
		setTimeout( () => {
			resolve(10)
		},3000)
	})
}
// when you call delay -> it runs resolve and after it's done the value of resolve will get passed to the method `then`
delay().then((v) => {
	console.log(v)
})
```