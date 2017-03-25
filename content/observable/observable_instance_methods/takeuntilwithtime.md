## [`Rx.Observable.prototype.takeUntilWithTime(other)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/takeuntilwithtime.js)

{% if book.isPdf %}

![takeUntilWithTime](http://reactivex.io/documentation/operators/images/takeUntilWithTime.png)

{% else %}



{% endif %}

Returns the values from the source observable sequence until the other observable sequence produces a value.

#### 参数
1. `endTime` *(`Date` | `Number`)*: Time to stop taking elements from the source sequence. If this value is less than or equal to the current time, the result stream will complete immediately.
2. [`scheduler`] *(`Scheduler`)*: Scheduler to run the timer on.

#### 返回值
*(`Observable`)*: An observable sequence with the elements taken until the specified end time.   

#### 例

[](http://jsbin.com/qabuce/1/embed?js,console)
