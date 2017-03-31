## [`Rx.Observable.repeat(value, [repeatCount], [scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/repeat.js)

![repeat](http://reactivex.io/documentation/operators/images/repeat.png)

使用指定的调度程序发送观察者消息，生成一个按给定的次数重复给定元素的可观察序列

#### 参数
1. `value` *(`Any`)*: 重复的元素.
2. `[repeatCount=-1]` *(`Number`)*: 重复元素的次数，如果没有指定，则无限重复
3. `[scheduler=Rx.Scheduler.immediate]` *(`Scheduler`)*: 生产者循环的调度程序，如果没有指定，默认为 `Scheduler.immediate`.

#### 返回值
*(`Observable`)*: 按给定的次数重复给定元素的可观察序列

#### 例

```js
var source = Rx.Observable.repeat(42, 3);

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

//=> Next: 42
// => Next: 42
// => Next: 42
// => Completed
```
[](http://jsbin.com/hezux/1/embed?js,console)
