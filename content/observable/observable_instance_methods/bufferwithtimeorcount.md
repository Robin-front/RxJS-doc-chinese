## [`Rx.Observable.prototype.bufferWithTimeOrCount(timeSpan, count, [scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/bufferwithtimeorcount.js)

{% if book.isPdf %}

![bufferWithTimeOrCount](http://reactivex.io/documentation/operators/images/bufferWithTimeOrCount6.png)

{% else %}



{% endif %}

Projects each element of an observable sequence into a buffer that is completed when either it's full or a given amount of time has elapsed.

#### 参数
1. `timeSpan` *(`Number`)*: Maximum time length of a buffer.
2. `count` *(`Number`)*: Maximum element count of a buffer.
3. `[scheduler=Rx.Scheduler.timeout]` *(`Scheduler`)*: Scheduler to run buffer timers on. If not specified, the timeout scheduler is used.

#### 返回值
*(`Observable`)*: An observable sequence of buffers. 

#### 例

[](http://jsbin.com/qaxid/1/embed?js,console)