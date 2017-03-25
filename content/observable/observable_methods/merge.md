## [`Rx.Observable.merge([scheduler], ...args)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/merge.js)

{% if book.isPdf %}

![merge](http://reactivex.io/documentation/operators/images/merge.png)

{% else %}



{% endif %}

Merges all the observable sequences and Promises into a single observable sequence.  

#### 参数
1. `[scheduler]` *(Scheduler=Rx.Scheduler.timeout)*: Scheduler to run the timer on. If not specified, Rx.Scheduler.immediate is used.
1. `args` *(Array|arguments)*: Observable sequences to merge into a single sequence.

#### 返回值
*(`Observable`)*: An observable sequence that produces a value after each period.

#### 例

[](http://jsbin.com/yicit/1/embed?js,console)
