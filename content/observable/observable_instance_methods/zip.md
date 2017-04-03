## [`Rx.Observable.prototype.zip(...args, [resultSelector])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/zipproto.js)

![zip](http://reactivex.io/documentation/operators/images/zip.png)

<rx-marbles key="zip"></rx-marbles>


当所有可观察的序列或数组在相应的索引上产生一个元素时，使用选择器函数将指定的可观察的序列或承诺合并到一个可观察的序列中。

参数中的最后一个元素必须是调用源中相应索引中的一系列元素的函数。

#### 参数
1. `args` *(`Arguments` | `Array`)*: 参数或可观察序列的数组.
2. `[resultSelector]` *(`Any`)*: 函数调用源中相应索引中的每个系列元素，仅当第一个参数不是数组时使用。

#### 返回值
*(`Observable`)*: 一个可观察的序列，包含使用指定的结果选择器函数组合源元素的结果。

#### 例

##### Using arguments

```js
var range = Rx.Observable.range(0, 5);

var source = range.zip(
    range.skip(1),
    range.skip(2),
    function (s1, s2, s3) {
        return s1 + ':' + s2 + ':' + s3;
    }
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

// => Next: 0:1:2
// => Next: 1:2:3
// => Next: 2:3:4
// => Completed
```

[](http://jsbin.com/pijaho/1/embed?js,console)

##### Using an array

```js
var array = [3, 4, 5];

var source = Rx.Observable.range(0, 3)
    .zip(
        array,
        function (s1, s2) {
            return s1 + ':' + s2;
        });

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

// => Next: 0:3
// => Next: 1:4
// => Next: 2:5
// => Completed
```

[](http://jsbin.com/wazuha/1/embed?js,console)
