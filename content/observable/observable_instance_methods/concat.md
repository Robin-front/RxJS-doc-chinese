## [`Rx.Observable.prototype.concat(...args)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/concatproto.js)

<rx-marbles key="concat"></rx-marbles>

![concat](http://reactivex.io/documentation/operators/images/concat.png)

拼接所有指定可观察序列，创建一个输出Observable，它会按拼接顺序发出所有值。

#### 参数
1. `args` *(`arguments` | `Array`)*: 接受一个数组或单个`Observable`参数.

#### 返回值
*(`Observable`)*: 返回一个按拼接顺序发出所有值的`Observable`

#### 例

[](http://jsbin.com/coyapo/1/embed?js,console)

```js
var timer1 = Rx.Observable.interval(1000).take(10);
var timer2 = Rx.Observable.interval(2000).take(6);
var timer3 = Rx.Observable.interval(500).take(10);
var result = timer1.concat(timer2, timer3);
result.subscribe(x => console.log(x));

// 结果如下:
// -1000ms-> 0 -1000ms-> 1 -1000ms-> ... 9
// -2000ms-> 0 -2000ms-> 1 -2000ms-> ... 5
// -500ms-> 0 -500ms-> 1 -500ms-> ... 9
```
