## [`Rx.Observable.prototype.timeoutwithselector([firstTimeout], timeoutDurationSelector, [other])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/timeoutwithselector.js)

{% if book.isPdf %}

![timeoutWithSelector](http://reactivex.io/documentation/operators/images/timeoutWithSelector3.png)

{% else %}



{% endif %}

Returns the source observable sequence, switching to the other observable sequence if a timeout is signaled.

#### 参数
1. `[firstTimeout=Rx.Observable.never()]` *(`Observable`)*: Observable sequence that represents the timeout for the first element. If not provided, this defaults to `Rx.Observable.never()`.
2. `timeoutDurationSelector` *(`Function`)*: Selector to retrieve an observable sequence that represents the timeout between the current element and the next element.
3. `[other=Rx.Observable.throw]` *(`Scheduler`)*:Sequence to return in case of a timeout. If not provided, this is set to `Observable.throw`

#### 返回值
*(`Observable`)*: The source sequence switching to the other sequence in case of a timeout.

#### 例

##### Without a first timeout

[](http://jsbin.com/jayez/1/embed?js,console)

##### With no other

[](http://jsbin.com/lezet/1/embed?js,console)

##### With other

[](http://jsbin.com/cuced/1/embed?js,console)
