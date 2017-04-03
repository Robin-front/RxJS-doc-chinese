## [`Rx.Observable.prototype.throttleWithSelector(throttleSelector)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/timeoutwithselector.js)

{% if book.isPdf %}

![debounceWithSelector](http://reactivex.io/documentation/operators/images/debounceWithSelector.png)

{% else %}



{% endif %}

Ignores values from an observable sequence which are followed by another value before dueTime.

#### 参数
1. `dueTime` *(`Number`)*: Selector function to retrieve a sequence indicating the throttle duration for each given element.

#### 返回值
*(`Observable`)*: The throttled sequence. 
    
#### 例

[](http://jsbin.com/gazesu/1/embed?js,console)
