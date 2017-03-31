### `Rx.Observable.return(value, [scheduler])` | `Rx.Observable.just(value, [scheduler])` ###

[&#x24C8;](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/perf/operators/just.js "View in source")

返回一个包含单值的observable，并使用指定的调度程序发送观察者消息。

在低于IE9浏览器下有一个别名`returnValue`，以及有一个正式的别名`just`

#### 参数
1. `value` *(`Any`)*: 单个值
2. `[scheduler=Rx.Scheduler.immediate]` *(`Scheduler`)*: 发送单值的调度程序，如果没有指定，默认为`Scheduler.immediate`.

#### 返回值
*(`Observable`)*: 返回一个包含单值的observable

#### 例
```js
var source = Rx.Observable.just(42);

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

// => Next: 42
// => Completed
```

[](http://jsbin.com/yupil/1/embed?js,console)
