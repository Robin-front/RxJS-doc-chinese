## [`Rx.Notification.prototype.exception`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/notification.js#L8)

Gets the exception from the OnError notification.

#### 返回值
*(Any)*: The Exception from the OnError notification.

{% if book.isPdf %}

#### [Example](http://jsbin.com/labura/2/edit?js,console)

```js
var notification = Rx.Notification.createOnError(new Error('invalid'));
console.log(notification.exception);

// => Error: invalid
```

{% else %}

#### 例

[](http://jsbin.com/labura/2/embed?js,console)

{% endif %}

{% if book.isPdf %}



{% else %}

### Location

- rx.js

{% endif %}