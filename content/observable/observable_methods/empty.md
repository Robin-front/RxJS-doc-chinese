## [`Rx.Observable.empty([scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/empty.js)

![empty](http://reactivex.io/documentation/operators/images/empty.png)

创建一个Observable，不会向Observer发送任何项目，并立即发出完成（`OnCompleted`）的通知。

#### 参数
1. `[scheduler=Rx.Scheduler.immediate]` *(`Scheduler`)*: 调度程序发送终止调用。

#### 返回值
*(`Observable`)*: 空的可观察序列

#### [Example](http://jsbin.com/kizosi/2/edit?js,console)

```js
var source = Rx.Observable.empty();

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onCompleted
```

```js
// 发出数字7，然后完成。
var result = Rx.Observable.empty().startWith(7);
result.subscribe(x => console.log(x));
```

```js
// 将序列'a'，'b'，'c'映射并展平奇数，
var interval = Rx.Observable.interval(1000);
var result = interval.mergeMap(x =>
  x % 2 === 1 ? Rx.Observable.of('a', 'b', 'c') : Rx.Observable.empty()
);
result.subscribe(x => console.log(x));

// x is equal to the count on the interval eg(0,1,2,3,...)
// 每1000ms，x会奇偶变换
// x为偶数将输出 abc
// x为奇数将什么也不输出
```
