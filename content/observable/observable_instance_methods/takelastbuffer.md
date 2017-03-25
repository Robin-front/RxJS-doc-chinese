## [`Rx.Observable.prototype.takeLastBuffer(count)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/takelastbuffer.js)

{% if book.isPdf %}

![takeLastBuffer](http://reactivex.io/documentation/operators/images/takeLastBuffer.png)

{% else %}



{% endif %}

Returns an array with the specified number of contiguous elements from the end of an observable sequence.

#### 参数
1. `count` *(`Number`)*: Number of elements to bypass at the end of the source sequence.

#### 返回值
*(`Observable`)*: An observable sequence containing a single array with the specified number of elements from the end of the source sequence.
     
#### 例

[](http://jsbin.com/kafiv/1/embed?js,console)
