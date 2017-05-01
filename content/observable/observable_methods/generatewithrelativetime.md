## [`Rx.Observable.generateWithRelativeTime(initialState, condition, iterate, resultSelector, timeSelector, [scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/generatewithrelativetime.js)

![generateWithRelativeTime](http://reactivex.io/documentation/operators/images/generateWithRelativeTime.png)

生成一个带相对时间控制的Observable,通过迭代一个初始值，直到判定条件返回false.Generates an observable sequence by iterating a state from an initial state until the condition fails.

#### 参数
1. `initialState` *(`Any`)*: 初始值.
2. `condition` *(`Function`)*: 生成判定条件的函数（返回false时终止循环）
3. `iterate` *(`Function`)*: 迭代函数
4. `resultSelector` *(`Function`)*: 处理迭代函数的值，返回最终结果
5. `timeSelector` *(`Function`)*: 间隔时间函数来控制每次迭代产生的值的速度，返回指示毫秒的整数值（相对当前的时间差值）
6. `[scheduler=Rx.Scheduler.timeout]` *(`Scheduler`)*: generator循环的调度器，如果没有提供，默认为 `Scheduler.timeout`.

#### 返回值
*(`Observable`)*: Observable

#### 例

```js
// Generate a value with an absolute time with an offset of 100ms multipled by value
var source = Rx.Observable.generateWithRelativeTime(
    1,
    function (x) { return x < 4; },
    function (x) { return x + 1; },
    function (x) { return x; },
    function (x) { return 100 * x; }
).timeInterval();

var subscription = source.subscribe(
    function (x) {
        console.log('Next:', x);
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

// => Next: {value: 1, interval: 100}
// => Next: {value: 2, interval: 200}
// => Next: {value: 3, interval: 300}
// => Completed
```
[](http://jsbin.com/jisopo/1/embed?js,console)
