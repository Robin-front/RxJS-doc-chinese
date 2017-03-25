## [`Rx.Observable.prototype.repeat(repeatCount)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/repeatproto.js)

{% if book.isPdf %}

![repeat](http://reactivex.io/documentation/operators/images/repeat.png)

{% else %}



{% endif %}

Repeats the observable sequence a specified number of times. If the repeat count is not specified, the sequence repeats indefinitely.
 
#### 参数
1. `repeatCount` *(`Number`)*:  Number of times to repeat the sequence. If not provided, repeats the sequence indefinitely.
 
#### 返回值
*(`Observable`)*: The observable sequence producing the elements of the given sequence repeatedly.  

#### 例

[](http://jsbin.com/raqico/1/embed?js,console)
