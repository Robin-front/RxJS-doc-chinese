## [`Rx.Observable.return(value, [scheduler])` | `Rx.Observable.just(value, [scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/return.js)

{% if book.isPdf %}

![return](http://reactivex.io/documentation/operators/images/just.png)

{% else %}



{% endif %}

Returns an observable sequence that contains a single element, using the specified scheduler to send out observer messages.

This is an alias for `just`.

#### 参数
1. `value` *(`Any`)*: Single element in the resulting observable sequence.
2. `[scheduler=Rx.Scheduler.immediate]` *(`Scheduler`)*: Scheduler to send the single element on. If not specified, defaults to Scheduler.immediate.

#### 返回值
*(`Observable`)*: An observable sequence with the single element.

#### 例

[](http://jsbin.com/yupil/1/embed?js,console)
