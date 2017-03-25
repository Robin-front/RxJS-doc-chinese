## [`Rx.Observable.delayWithSelector([subscriptionDelay], delayDurationSelector)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/delaywithselector.js)

{% if book.isPdf %}

![delayWithSelector](http://reactivex.io/documentation/operators/images/delayWithSelector.png)

{% else %}

<rx-marbles key="delayWithSelector"></rx-marbles>

{% endif %}

Time shifts the observable sequence by dueTime. The relative time intervals between the values are preserved.

#### 参数
1. `[subscriptionDelay]` *(`Observable`)*: Sequence indicating the delay for the subscription to the source. 
2. `delayDurationSelector` *(`Function`)*: Selector function to retrieve a sequence indicating the delay for each given element.

#### 返回值
*(`Observable`)*: Time-shifted sequence.
  
#### 例

##### With subscriptionDelay

[](http://jsbin.com/buwaxe/1/embed?js,console)

##### Without subscriptionDelay

[](http://jsbin.com/soheg/1/embed?js,console)