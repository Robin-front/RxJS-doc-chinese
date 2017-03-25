## [`Rx.Observable.prototype.publish([selector])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/publish.js)

{% if book.isPdf %}

![publish](http://reactivex.io/documentation/operators/images/publishConnect.f.png)

{% else %}



{% endif %}

Returns an observable sequence that is the result of invoking the selector on a connectable observable sequence that shares a single subscription to the underlying sequence.

This operator is a specialization of `multicast` using a regular `Rx.Subject`.

#### 参数
1. `[selector]` *(`Function`)*: Selector function which can use the multicasted source sequence as many times as needed, without causing multiple subscriptions to the source sequence. Subscribers to the given source will receive all notifications of the source from the time of the subscription on.
  
#### 返回值
*(ConnectableObservable)*: An observable sequence that contains the elements of a sequence produced by multicasting the source sequence within a selector function.
   
#### 例

##### Without publish

[](http://jsbin.com/yireha/1/embed?js,console)

##### With publish

[](http://jsbin.com/hotexi/1/embed?js,console)
