## [`Rx.Observable.prototype.windowWithCount(count, [skip])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/windowwithcount.js)

{% if book.isPdf %}

![windowWithCount](http://reactivex.io/documentation/operators/images/windowWithCount3.png)

{% else %}



{% endif %}

Projects each element of an observable sequence into zero or more windows which are produced based on element count information.

#### 参数
1. `count` *(`Function`)*: Length of each buffer.
2. `[skip]` *(`Function`)*: Number of elements to skip between creation of consecutive windows. If not provided, defaults to the count.

#### 返回值
*(`Observable`)*: An observable sequence of windows. 

#### 例

##### Without a skip

[](http://jsbin.com/difer/1/embed?js,console)

##### Using a skip

[](http://jsbin.com/javewe/1/embed?js,console)
