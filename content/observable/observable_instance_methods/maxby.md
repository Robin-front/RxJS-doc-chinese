## [`Rx.Observable.prototype.maxBy(keySelector, [comparer])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/maxby.js)

{% if book.isPdf %}

![maxBy](http://reactivex.io/documentation/operators/images/maxBy.png)

{% else %}



{% endif %}

Returns the maximum value in an observable sequence according to the specified comparer.

#### 参数
1. `keySelector` *(`Function`)*: Key selector function.
2. `[comparer]` *(`Function`)*:  Comparer used to compare elements.
 
#### 返回值
*(`Observable`)*: An observable sequence containing a list of zero or more elements that have a maximum key value.
 
#### 例

[](http://jsbin.com/zupib/1/embed?js,console)
