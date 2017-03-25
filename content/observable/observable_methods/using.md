## [`Rx.Observable.using(resourceFactory, observableFactory)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/using.js)

{% if book.isPdf %}

![using](http://reactivex.io/documentation/operators/images/using.png)

{% else %}



{% endif %}

 Constructs an observable sequence that depends on a resource object, whose lifetime is tied to the resulting observable sequence's lifetime.

#### 参数
1. `resourceFactory` *(`Function`)*: Factory function to obtain a resource object.
2. `observableFactory` *(`Scheduler`)*: Factory function to obtain an observable sequence that depends on the obtained resource.

#### 返回值
*(`Function`)*: An observable sequence whose lifetime controls the lifetime of the dependent resource object.

#### 例

[](http://jsbin.com/yewaf/1/embed?js,console)
