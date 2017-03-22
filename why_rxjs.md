## Why RxJS? ##

<!-- toc -->

你可能会问，为什么选择 RxJS?  为什么不是 Promises? Promises 可以很好地解决异步操作，像使用 [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) 去查询服务器, 它预期会返回值并且最终完成请求。  `The Reactive Extensions` 统一了 `JavaScript` 中的 `Promises`, `callbacks` 以及事件数据，比如 DOM输入, `Web Workers`, `Web Sockets`. 一旦我们统一了这些概念，就可以进行各种各样的组合.

为了让您了解丰富的组合，我们可以创建一个自动完成功能，它从文本输入中接收用户输入，然后查询服务，确保不会对每个键盘输入的进行泛滥地调用，而是以一种更自然的方式调用。

首先，我们将引用JavaScript文件，包括jQuery，尽管RxJS没有依赖于jQuery ...
```html
<script src="http://code.jquery.com/jquery.js"></script>
<script src="rx.lite.js"></script>
```
接下来，我们将从输入框获取用户输入，使用[`Rx.Observable.fromEvent`](content/observable/observable_methods/fromevent.html) 方法监听 `keyup` 事件.  如果 [jQuery](http://jquery.com), [Zepto](http://zeptojs.com/), [AngularJS](https://angularjs.org/) and [Ember.js](http://emberjs.com/) 可用，将会使用它们来绑定事件, 否则将使用原生事件绑定. 这跟您的框架思考事件的一致方式，因此没有任何惊喜。
```js
var $input = $('#input'),
    $results = $('#results');

/* 只从 keyup 事件获得输入值 */
var keyups = Rx.Observable.fromEvent($input, 'keyup')
    .map(e => e.target.value)
    .filter(text => text.length > 2);

/* 函数节流输出设置为 500ms */
var throttled = keyups.throttle(500 /* ms */);

/* 现在判断值是否有改变，只获取不同的值 */
var distinct = throttled.distinctUntilChanged();
```
现在，让我们来查询维基百科！在RxJS中，我们可以立即通过`Rx.Observable.fromPromise`方法绑定到任何[Promises A+](https://github.com/promises-aplus/promises-spec)的实现上，或者直接返回它，并将其封装。

```js
function searchWikipedia (term) {
    return $.ajax({
        url: 'http://en.wikipedia.org/w/api.php',
        dataType: 'jsonp',
        data: {
            action: 'opensearch',
            format: 'json',
            search: term
        }
    }).promise();
}
```
一旦创建，现在我们可以将不同的节流输入绑定在一起，然后查询服务。在这种情况下，我们将调用`flatMapLatest`获取该值，并确保我们不会有任何混乱的调用。

```js
var suggestions = distinct.flatMapLatest(searchWikipedia);
```
最后，我们将在可观察对象上调用`subscribe`方法开始拉数据。

```js
suggestions.subscribe(data => {
    var res = data[1];

    /* Do something 像数据绑定 */
    $results.empty();

    $.each(res, (_, value) => $('<li>' + value + '</li>').appendTo($results));
}, error => {
    /* handle any errors */
    $results.empty();

    $('<li>Error: ' + error + '</li>').appendTo($results);
});
```
