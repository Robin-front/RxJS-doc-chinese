## [`Rx.Observable.if(condition, thenSource, [elseSource])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/if.js)

![if](http://reactivex.io/documentation/operators/images/if.png)

给出一个或两个流，判断Observable应该取哪个流的值。（相当于 if...else）

#### 参数
1. `condition` *(`Function`)*: 判决条件，为`true`则取 `thenSource`, 否则执行 `elseSource`.
2. `thenSource` *(`Observable`)*: Observable, 当`condition`为`true`时执行。
3. `[elseSource]` *(Observable|Scheduler)*:  Observable, 当`condition`为`false`时执行。如果没有提供，则默认为 `Rx.Observabe.Empty`.

#### 返回值
*(`Observable`)*: Observable

#### 例

##### This uses and only then source

```js
// 只传入 then source
var shouldRun = true;

var source = Rx.Observable.if(
    function () { return shouldRun; },
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
// => Completed
```
[](http://jsbin.com/pijusu/1/embed?js,console)

##### The next example uses an elseSource

```js
// 设置 elseSource
var shouldRun = false;

var source = Rx.Observable.if(
    function () { return shouldRun; },
    Rx.Observable.return(42),
    Rx.Observable.return(56)
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

// => Next: 56
// => Completed
```
[](http://jsbin.com/fegak/1/embed?js,console)
