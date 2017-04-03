## [`Rx.Observable.prototype.exclusiveMap(selector, thisArgs)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/exclusivemap.js)

Performs a exclusive map waiting for the first to finish before subscribing to another observable.

Observables that come in between subscriptions will be dropped on the floor.

#### 参数
1. `selector` *(`Function`)*: An function to invoke for every item in the current subscription.
2. `thisArgs` *(Any)*: An optional context to invoke with the selector parameter.

#### 返回值
*(`Observable`)*: An exclusive observable with only the results that happen when subscribed.

#### 例