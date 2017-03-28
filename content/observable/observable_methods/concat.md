## [`Rx.Observable.concat(...args)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/concat.js)

<rx-marbles key="concat"></rx-marbles>

![concat](http://reactivex.io/documentation/operators/images/concat.png)

拼接所有指定可观察序列，创建一个输出Observable，它会按拼接顺序发出所有值。

#### 参数
1. `args` *(`Array` | `arguments`)*: 传入需要拼接的`Observable`(可观察流) 或 `Promises`.

#### 返回值
*(`Observable`)*: 返回一个按拼接顺序发出所有值的`Observable`

#### 例


##### [拼接可观察对象 `Observable` ](http://jsbin.com/sitiko/2/edit?js,console)

```js
/* Using Observable sequences */
var source1 = Rx.Observable.return(42);
var source2 = Rx.Observable.return(56);

var source = Rx.Observable.concat(source1, source2);

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: 42
// => onNext: 56
// => onCompleted
```

#### [拼接 `Promises` 和 `Observable`](http://jsbin.com/topor/2/edit?js,console)

```js
/* Using Promises and Observable sequences */
var source1 = Rx.Observable.return(42);
var source2 = RSVP.Promise.resolve(56);

var source = Rx.Observable.concat(source1, source2);

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: 42
// => onNext: 56
// => onCompleted
```
