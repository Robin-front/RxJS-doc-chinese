## [`Rx.Observable.from(iterable, [mapFn], [thisArg], [scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/from.js)

![from](http://reactivex.io/documentation/operators/images/from.png)

从Array创建一个Observable，参数也可以是一个类数组的对象，一个Promise，一个可迭代对象或一个Observable类对象。

#### 参数
1. `iterable` *(`Array` | `Arguments` | `Iterable`)*: 类数组或可迭代对象
2. `[mapFn]` *(`Function`)*: 应用到第一个参数的映射函数
3. `[thisArg]` *(`Any`)*: 提供映射函数上下文
4. `[scheduler=Rx.Scheduler.currentThread]` *(`Scheduler`)*: 枚举输入序列的调度器

#### 返回值
*(`Observable`)*: 通过给定的可迭代序列生成的`observable`.

#### 例

##### [Array-like object (arguments) to Observable](http://jsbin.com/dekire/2/edit?js,console)

```js
function f() {
  return Rx.Observable.from(arguments);
}

f(1, 2, 3).subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: 1
// => onNext: 2
// => onNext: 3
// => onCompleted
```

##### [Set](http://jsbin.com/dapoju/4/edit?js,console)

```js
var s = new Set(["foo", window]);

Rx.Observable.from(s).subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: foo
// => onNext: window
// => onCompleted
```

##### [Map](http://jsbin.com/yukiyu/4/edit?js,console)

```js
var m = new Map([[1, 2], [2, 4], [4, 8]]);

Rx.Observable.from(m).subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: [1, 2]
// => onNext: [2, 4]
// => onNext: [4, 8]
// => onCompleted
```

##### [String](http://jsbin.com/bemuqa/3/edit?js,console)

```js
Rx.Observable.from("foo").subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: f
// => onNext: o
// => onNext: o
// => onCompleted
```

##### [Array](http://jsbin.com/tiluno/2/edit?js,console)

```js
Rx.Observable.from([1, 2, 3], x => x + x).subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: 2
// => onNext: 4
// => onNext: 6
// => onCompleted
```

##### [Generate a sequence of numbers](http://jsbin.com/piyehe/2/edit?js,console)

```js
Rx.Observable.from({length: 5}, (v, k) => k).subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: 0
// => onNext: 1
// => onNext: 2
// => onNext: 3
// => onNext: 4
// => onCompleted
```
