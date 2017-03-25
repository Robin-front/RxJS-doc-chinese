## [`Rx.Observable.prototype.min([comparer])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/min.js)

{% if book.isPdf %}

![min](http://reactivex.io/documentation/operators/images/min.png)

{% else %}

<rx-marbles key="min"></rx-marbles>

{% endif %}

Returns the minimum element in an observable sequence according to the optional comparer else a default greater than less than check.

#### 参数
1. `[comparer]` *(`Function`)*:  Comparer used to compare elements.
 
#### 返回值
*(`Observable`)*: An observable sequence containing a single element with the minimum element in the source sequence.
 
#### 例

##### Without comparer

[](http://jsbin.com/gerap/1/embed?js,console)

#### With a comparer

[](http://jsbin.com/cayun/1/embed?js,console)
