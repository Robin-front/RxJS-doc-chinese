## [`Rx.Observable.create(subscribe)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/create.js)

![create](https://github.com/Netflix/RxJava/wiki/images/rx-operators/create.png)

创建一个新的Observable，当Observer订阅它时，它将执行指定的函数。这是`createWithDisposable`方法的别称。

#### 参数
1. `subscribe` *(`Function`)*: 执行所得到的可观察序列的订阅方法，可选地返回将被包装在一次性对象中的函数（如： `onNext`、`onCompleted`、`onError`）。这也可以是一次性的对象。

#### 返回值
*(`Observable`)*: 返回 Observable，拥有约定实现的订阅方法。

#### 例

##### [Using a function](http://jsbin.com/luweq/2/edit?js,console)

```js
/* Using a function */
var source = Rx.Observable.create(observer => {
    observer.onNext(42);
    observer.onCompleted();

    // 这是可选项, 如果不需要清除它，可以不必返回。
    return () => console.log('disposed')
});

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: 42
// => onCompleted

subscription.dispose();

// => disposed
```

##### [Using a disposable](http://jsbin.com/puveyi/2/edit?js,console)

```js
/* Using a disposable */
var source = Rx.Observable.create(observer => {
    observer.onNext(42);
    observer.onCompleted();

    // 这是可选项, 如果不需要清除它，可以不必返回
    return Rx.Disposable.create(() => console.log('disposed'));
});

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: 42
// => onCompleted
```
