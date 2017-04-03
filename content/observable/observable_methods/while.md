## [`Rx.Observable.while(condition, source)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/while.js)

![while](http://reactivex.io/documentation/operators/images/while.png)

重复源只要条件满足模拟的while循环。低于IE9浏览器有一个别名叫做`whiledo`。

#### 参数
1. `condition` *(`Function`)*: 决定是否继续循环的条件
2. `source` *(`Observable`)*: 如果条件返回true,就会执行。

#### 返回值
*(`Observable`)*: 只要条件满足就会一直重复的observalbe

#### 例

```js
var i = 0;

// Repeat until condition no longer holds
var source = Rx.Observable.while(
    function () { return i++ < 3 },
    Rx.Observable.return(42)
);

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

// => Next: 42
// => Next: 42
// => Next: 42
// => Completed
```
[](http://jsbin.com/serat/1/embed?js,console)
