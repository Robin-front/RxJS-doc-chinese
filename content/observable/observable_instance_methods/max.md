## [`Rx.Observable.prototype.max([comparer])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/max.js)

{% if book.isPdf %}

![max](http://reactivex.io/documentation/operators/images/max.png)

{% else %}

<rx-marbles key="max"></rx-marbles>

{% endif %}

Returns the maximum value in an observable sequence according to the specified comparer.

#### 参数
1. `[comparer]` *(`Function`)*:  Comparer used to compare elements.
 
#### 返回值
*(`Observable`)*: An observable sequence containing a single element with the maximum element in the source sequence.

#### 例

##### Without comparer

[](http://jsbin.com/zages/1/embed?js,console)

##### With a comparer

[](http://jsbin.com/kejay/1/embed?js,console)
