## [`Rx.Observable.prototype.controlled([enableQueue])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/backpressure/controlled.js)

{% if book.isPdf %}

![controlled](http://reactivex.io/documentation/operators/images/bp.stopAndWait.png)

{% else %}



{% endif %}

Attaches a controller to the observable sequence with the ability to queue.

#### 参数
1. `[enableQueue]` *(Boolean)*: Whether to enable queueing.  If not specified, defaults to true.

#### 返回值
*(`Observable`)*: An observable sequence which can be used to request values from the sequence.

#### 例

[](http://jsbin.com/liraw/1/embed?js,console)