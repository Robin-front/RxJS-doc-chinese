## [`Rx.Observable.prototype.bufferWithTime(timeSpan, [timeShift | scheduler], [scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/bufferwithtime.js)

{% if book.isPdf %}

![bufferWithTime](http://reactivex.io/documentation/operators/images/bufferWithTime5.png)

{% else %}



{% endif %}

Projects each element of an observable sequence into zero or more buffers which are produced based on timing information.

#### 参数
1. `timeSpan` *(`Number`)*: Length of each buffer (specified as an integer denoting milliseconds).
2. `[timeShift]` *(`Number`)*: Interval between creation of consecutive buffers (specified as an integer denoting milliseconds).
3. `[scheduler=Rx.Scheduler.timeout]` *(`Scheduler`)*: Scheduler to run buffer timers on. If not specified, the timeout scheduler is used.

#### 返回值
*(`Observable`)*: An observable sequence of buffers. 

#### 例

##### Without a skip

[](http://jsbin.com/zokej/1/embed?js,console)

##### Using a skip

[](http://jsbin.com/rafay/1/embed?js,console)