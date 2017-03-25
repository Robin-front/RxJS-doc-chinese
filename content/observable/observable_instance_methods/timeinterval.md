## [`Rx.Observable.prototype.timeInterval([scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/timeinterval.js)

{% if book.isPdf %}

![timeInterval](http://reactivex.io/documentation/operators/images/timeInterval.png)

{% else %}



{% endif %}

Records the time interval between consecutive values in an observable sequence.

#### 参数
1. `[scheduler=Rx.Observable.timeout]` *(`Scheduler`)*: Scheduler used to compute time intervals. If not specified, the timeout scheduler is used.

#### 返回值
*(`Observable`)*: An observable sequence with time interval information on values.

#### 例

[](http://jsbin.com/ragoq/1/embed?js,console)
