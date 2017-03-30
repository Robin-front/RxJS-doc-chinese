## [`Rx.Observable.interval(period, [scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/interval.js)


![interval](http://reactivex.io/documentation/operators/images/interval.png)

Returns an observable sequence that produces a value after each period.

#### 参数
1. `period` *(`Number`)*: Period for producing the values in the resulting sequence (specified as an integer denoting milliseconds).
2. `[scheduler]` *(Scheduler=Rx.Scheduler.timeout)*: Scheduler to run the timer on. If not specified, Rx.Scheduler.timeout is used.

#### 返回值
*(`Observable`)*: An observable sequence that produces a value after each period.

#### 例

[](http://jsbin.com/lozay/1/embed?js,console)
