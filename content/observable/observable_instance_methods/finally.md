## [`Rx.Observable.prototype.finally(action)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/finally.js)

{% if book.isPdf %}

![finally](http://reactivex.io/documentation/operators/images/finally.png)

{% else %}



{% endif %}

Invokes a specified action after the source observable sequence terminates gracefully or exceptionally.

#### 参数
1. `predicate` *(`Function`)*: A function to test each source element for a condition;  The callback is called with the following information:
    1. the value of the element
    2. the index of the element
    3. the Observable object being subscribed
2. `[thisArg]` *(`Any`)*: Object to use as `this` when executing the predicate.

#### 返回值
*(`Observable`)*: An observable sequence that contains elements from the input sequence that satisfy the condition.  

#### 例

[](http://jsbin.com/woture/1/embed?js,console)
