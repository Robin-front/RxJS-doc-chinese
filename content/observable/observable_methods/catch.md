## [`Rx.Observable.catch(...args)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/catch.js)

![catch](http://reactivex.io/documentation/operators/images/Catch.png)

一个可观察对象异常时，继续订阅其他的可观察对象的结果。

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

#### 联想与应用

可用于稳定系统，比如pm2, 我们经常会开启多个node进程，结合 nginx， 当一个node进程挂掉重启时，还能保证有另一个node进程被正常访问。

```
var service1 = Observable.create("node进程#1");
var service2 = Observable.create("node进程#2");

Observable.catch(service1, service2).subscribe({
    res =>console.log('succeed'),
    e => console.log('所有服务均不可用')
    ()=>console.log('completed')
})
```
