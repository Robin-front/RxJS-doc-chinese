## [`Rx.Observable.of(...args)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/of.js)

![of](http://reactivex.io/documentation/operators/images/of.png)

将一系列参数转换成 observable 序列。

#### 参数
1. `args` *(Arguments)*: 将参数转换为可观察序列的参数列表.

#### 返回值
*(`Observable`)*: 一个将参数值作为返回值的observable。

#### 例

```js
var source = Rx.Observable.of(1,2,3);

var subscription = source.subscribe(
    function (x) {
        console.log('Next: ' + x);
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

// => Next: 1
// => Next: 2
// => Next: 3
// => Completed
```

[](http://jsbin.com/sibiy/1/embed?js,console)
