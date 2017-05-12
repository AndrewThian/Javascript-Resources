Creating modules in javascript by exporting them. Below we are exporting the formSubmit function and requiring it in another javascript file. 

```js
module.exports.submit = formSubmit

function formSubmit (submitEvent) {
  var name = document.querySelector('input').value
  request({
    uri: "http://example.com/upload",
    body: name,
    method: "POST"
  }, postResponse)
}

function postResponse (err, response, body) {
  var statusMessage = document.querySelector('.status')
  if (err) return statusMessage.value = err
  statusMessage.value = body
}
```
```js
var formUploader = require('formuploader')
document.querySelector('form').onsubmit = formUploader.submit
```