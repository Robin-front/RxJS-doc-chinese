## [`Rx.Notification.prototype.value`]()

Gets the value from the OnNext notification.

#### 返回值
*(Any)*: The value from the OnNext notification.

{% if book.isPdf %}

#### [Example](http://jsbin.com/cubive/2/edit?js,console)

```js
var notification = Rx.Notification.createOnNext(42);
console.log(notification.value);

// => 42
```

{% else %}

#### 例

[](http://jsbin.com/cubive/2/embed?js,console)

{% endif %}

### Location

- rx.js