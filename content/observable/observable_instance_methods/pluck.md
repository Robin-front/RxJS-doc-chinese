## [`Rx.Observable.prototype.pluck(property)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/pluck.js)

{% if book.isPdf %}

![pluck](http://reactivex.io/documentation/operators/images/pluck.png)

{% else %}



{% endif %}

Projects each element of an observable sequence into a new form by incorporating the element's index.

#### 参数
1. `property` *(`String`)*: The property to pluck.
 
#### 返回值
*(`Observable`)*: Returns a new Observable sequence of property values.

#### 例

[](http://jsbin.com/wigiy/1/embed?js,console)
