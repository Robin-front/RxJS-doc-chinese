### <a id="rxdomrequestposturl-body"></a>`Rx.DOM.Request.post(url, [body])`
<a href="#rxdomrequestposturl-body">#</a>[&#x24C8;](https://github.com/Reactive-Extensions/RxJS-DOM/blob/master/rx.dom.js#L238-L240 "View in source") [&#x24C9;][1]

Creates an observable sequence from an Ajax POST Request with the body.  This method is just shorthand for the `Rx.DOM.Request.ajax` method with the POST method.

#### Syntax
```js
Rx.DOM.Request.post(url, body);
```
#### 参数
1. `url` *(String)*: A string of the URL to make the Ajax call.
2. `[body]` *(Object)*: The body to post

#### 返回值
*(Observable)*: The observable sequence which contains the response from the Ajax POST.

#### 例
```js
Rx.DOM.Request.post('/test')
	.subscribe(
		function (xhr) {
			console.log(xhr.responseText);
		},
		function (err) {
			// Log the error
		}
	);
```