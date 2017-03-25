## [`Rx.Observable.prototype.retry([retryCount])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/retry.js)

{% if book.isPdf %}

![retry](http://reactivex.io/documentation/operators/images/retry.png)

{% else %}



{% endif %}

Projects each element of an observable sequence into a new form by incorporating the element's index.  This is an alias for the `select` method.

#### 参数
1. `[retryCount]` *(`Number`)*:  Number of times to retry the sequence. If not provided, retry the sequence indefinitely.
 
#### 返回值
*(`Observable`)*: An observable sequence producing the elements of the given sequence repeatedly until it terminates successfully. 

#### 例

[](http://jsbin.com/hixenu/1/embed?js,console)