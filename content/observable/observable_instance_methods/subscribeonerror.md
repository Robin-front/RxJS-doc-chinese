## [`Rx.Observable.prototype.subscribeOnError(onError, [thisArg])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/observable.js)

{% if book.isPdf %}



{% else %}



{% endif %}

Subscribes a function to invoke upon exceptional termination of the observable sequence.

#### 参数
1. `onError` *(`Function`)*: Function to invoke upon exceptional termination of the observable sequence.
2. `[thisArg]` *(`Any`)*: Object to use as this when executing callback.

#### 返回值
*(Disposable)*: The source sequence whose subscriptions and unsubscriptions happen on the specified scheduler.

#### 例

##### Using functions

[](http://jsbin.com/jevipi/1/embed?js,console)

##### With a thisArg

[](http://jsbin.com/wesoba/1/embed?js,console)

