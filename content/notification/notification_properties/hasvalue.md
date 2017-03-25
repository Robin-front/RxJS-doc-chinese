## [`Rx.Notification.prototype.hasValue`]()

Determines whether the Notification has a value.  Returns `true` for OnNext Notifications, and `false` for OnError and OnCompleted Notifications.

#### 返回值
*(Bool)*: Returns `true` for OnNext Notifications, and `false` for OnError and OnCompleted Notifications.

{% if book.isPdf %}

#### [Example](http://jsbin.com/cicixi/3/edit?js,console)

```js
var onNext = Rx.Notification.createOnNext(42);
console.log(onNext.hasValue);

// => true

var onCompleted = Rx.Notification.createOnCompleted();
console.log(onCompleted.hasValue);

// => false
```

{% else %}

#### 例

[](http://jsbin.com/cicixi/3/embed?js,console)

{% endif %}

{% if book.isPdf %}



{% else %}

### Location

- rx.js

{% endif %}