## [`Rx.Observable.fromIterable(iterable, [scheduler])`]()

将ES6的可迭代对象转换为Observable。

#### 参数
1. `iterable` *(Iterable)*: generator函数或可迭代对象，比如 Set, Map, 等。
2. `[scheduler=Rx.Scheduler.currentThread]` *(`Scheduler`)*: 执行函数的调度器， 如果没有指定，则默认为 `Rx.Scheduler.currentThread`.

#### 返回值
*(`Function`)*: observable（此处应该返回 observable，而不是function）

#### 例
```js
// Using a Set
var source = Rx.Observable.fromIterable(new Set([1,2,3]));

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

// Using a generator function
var source = Rx.Observable.fromIterable(function* () { yield 42; });

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
// => Completed
```
