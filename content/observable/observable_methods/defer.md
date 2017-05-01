## [`Rx.Observable.defer(observableFactory)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/defer.js)

![defer](http://reactivex.io/documentation/operators/images/defer.png)

当一个新的观察者订阅时，返回一个可观察序列调用的特殊的工厂函数。(注意，是订阅时才生成`observable`,它是惰性的)。

`defer`允许您仅在Observer订阅时创建Observable，并为每个Observer创建一个新的Observable。它等待观察者订阅它，然后它产生一个Observable，通常具有Observable工厂功能。它为每个用户重新启动，所以虽然每个用户可能会认为它正在订阅同一个Observable，实际上每个用户都有自己的Observable。

#### 参数
1. `observableFactory` *(`Function`)*: Observable工厂函数每次调用观察者订阅时输出Observable。也可以返回一个`Promise`

#### 返回值
*(`Observable`)*: 可观察的序列，其观察者触发给定的可观察的工厂函数的调用。

#### 例


##### [Using an observable sequence](http://jsbin.com/vigitu/2/edit?js,console)

```js
/* Using an observable sequence */
var source = Rx.Observable.defer(() => Rx.Observable.return(42));

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: 42
// => onCompleted
```

##### [Using a promise](http://jsbin.com/memuf/2/edit?js,console)

```js
/* Using a promise */
var source = Rx.Observable.defer(() => RSVP.Promise.resolve(42));

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: 42
// => onCompleted
```
