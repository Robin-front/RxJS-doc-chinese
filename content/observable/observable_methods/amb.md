## [`Rx.Observable.amb(...args)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/amb.js)

![amb](http://reactivex.io/documentation/operators/images/amb.png)

<rx-marbles key="amb"></rx-marbles>

传播的可观察序列或Promise的最先返回值。“amb”代表[ambiguous](http://blogs.msdn.com/b/jeffva/archive/2009/11/18/amb-materialize-and-dematerialize.aspx)。
(差点没看明白，就是传入多个可观察对象，谁先返回值，就取谁的值给订阅者，上代码。)

#### 参数
1. `args` *(Array|arguments)*: 多个可观察源或Promises、数组作为参数。

#### 返回值
*(`Observable`)*: 一个可观察的序列，其表面为任何给定的序列，无论哪个首先返回。

#### 例


##### [使用可观察序列](http://jsbin.com/vanaci/3/edit?js,console)

```js
var source = Rx.Observable.amb(
    Rx.Observable.timer(500).select(() => 'foo'),
    Rx.Observable.timer(200).select(() => 'bar')
);

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));
// timer = 200先返回，所以只取第二个可观察对象的返回值
// => onNext: bar
// => onCompleted
```

##### [混合使用Promises和Observables](http://jsbin.com/bukag/2/edit?js,console)

```js
var source = Rx.Observable.amb(
    RSVP.Promise.resolve('foo'),
    Rx.Observable.timer(200).select(() => 'bar')
);

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));
// 由于Promise是立即resolve，所以取Promise的返回值
// => onNext: foo
// => onCompleted
```

> 下面是 jsbin 例子，无法查看可能是因为浏览器阻止
##### Using Observable sequences
[](http://jsbin.com/vanaci/3/embed?js,console)

##### Using Promises and Observables
[](http://jsbin.com/bukag/2/embed?js,console)
