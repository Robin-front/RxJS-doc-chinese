## [`Rx.Observable.prototype.onErrorResumeNext(second)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/onerrorresumenextproto.js)

{% if book.isPdf %}

![onErrorResumeNext](http://reactivex.io/documentation/operators/images/onErrorResumeNext.png)

{% else %}



{% endif %}

Continues an observable sequence that is terminated normally or by an exception with the next observable sequence or Promise.

#### 参数
1. `second` *(`Observable` | `Promise`)*:  Second observable sequence used to produce results after the first sequence terminates.

#### 返回值
*(`Observable`)*: An observable sequence that concatenates the first and second sequence, even if the first sequence terminates exceptionally.

#### 例

[](http://jsbin.com/jutum/1/embed?js,console)
