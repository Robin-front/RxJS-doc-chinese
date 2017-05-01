## [`Rx.Observable.pairs(obj, [scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/pairs.js)

将一系列键值对转换成 observable 序列，并使用可选的调度器来枚举对象。

#### 参数
1. `obj` *(Object)*: 需要转换的对象
2. `[scheduler]` *(`Scheduler`)*: 执行枚举输入序列的调度器，如果没有提供，则默认为 `Rx.Scheduler.currentThread`

#### 返回值
*(`Observable`)*: 返回一个 observable

#### 例
```js
// Using Standard JavaScript
var obj = {
  foo: 42,
  bar: 56,
  baz: 78
};

var source = Rx.Observable.pairs(obj);

var subscription = source.subscribe(
  function (x) {
    console.log('Next: %s', x);
  },
  function (err) {
    console.log('Error: %s', err);
  },
  function () {
    console.log('Completed');
  });

// => Next: ['foo', 42]
// => Next: ['bar', 56]
// => Next: ['baz', 78]
// => Completed
  ```

ES6 makes for an even nicer experience such as:
```js
let obj = {
  foo: 42,
  bar: 56,
  baz: 78
};

let source = Rx.Observable.pairs(obj);

let subscription = source.subscribe(
  x => {
    var [key, value] = x;
    console.log('Key:', key, 'Value:', value);
  },
  err => {
    console.log('Error: %s', err);
  },
  => () {
    console.log('Completed');
  });

// => Key: 'foo' Value: 42
// => Key: 'bar' Value: 56
// => Key: 'baz' Value: 78
// => Completed
```
