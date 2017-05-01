## [`Rx.Observable.generate(initialState, condition, iterate, resultSelector, [scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/generate.js)

![generate](http://reactivex.io/documentation/operators/images/generate.png)


将一个数组(事实上，初始值可以是任何类型)转换为observable，使用可选的调度器枚举数组。

#### 参数
1. `initialState` *(`Any`)*: 初始值，可以是任何类型
2. `condition` *(`Function`)*: 终止条件（取决于该函数何时返回false）
3. `iterate` *(`Function`)*: 进行迭代的函数
4. `resultSelector` *(`Function`)*: 结果处理函数，返回最终的结果
5. `[scheduler=Rx.Scheduler.currentThread]` *(`Scheduler`)*: generator循环的调度器，如果没有提供，默认为`Scheduler.currentThread`.

#### 返回值
*(`Observable`)*: The generated sequence.

#### 例

```js
var source = Rx.Observable.generate(
    0,
    function (x) { return x < 3; }, // x = 3时，该函数返回false,终止
    function (x) { return x + 1; }, // 迭代的函数
    function (x) { return x; } // 结果处理函数
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

// => Next: 0
// => Next: 1
// => Next: 2
// => Completed
```

[](http://jsbin.com/vemibe/1/embed?js,console)
