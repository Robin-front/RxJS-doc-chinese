## [`Rx.Observable.catch(...args)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/catch.js)

![catch](http://reactivex.io/documentation/operators/images/Catch.png)

继续一个可观察到的序列，该序列由下一个观察序列的结束。

#### 参数
1. `args` *(`Array` | `arguments`)*: 捕捉异常的可观察序列。

#### 返回值
*(`Observable`)*: 包含连续源序列的元素的观察序列，直到源序列成功结束。


#### [Example](http://jsbin.com/qagidu/2/edit?js,console)

```js
var obs1 = Rx.Observable.throw(new Error('error'));
var obs2 = Rx.Observable.return(42);

var source = Rx.Observable.catch(obs1, obs2);

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: 42
// => onCompleted
```
