## [`Rx.Observable.prototype.throttle(dueTime, [scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/debounce.js)

{% if book.isPdf %}

![debounce](http://reactivex.io/documentation/operators/images/debounce.png)

{% else %}



{% endif %}

Ignores values from an observable sequence which are followed by another value before dueTime.

#### 参数
1. `dueTime` *(`Number`)*: Duration of the throttle period for each value (specified as an integer denoting milliseconds).
2. `[scheduler=Rx.Scheduler.timeout]` *(`Any`)*: Scheduler to run the throttle timers on. If not specified, the timeout scheduler is used.

#### 返回值
*(`Observable`)*: The throttled sequence. 
    
#### 例

[](http://jsbin.com/zatav/1/embed?js,console)