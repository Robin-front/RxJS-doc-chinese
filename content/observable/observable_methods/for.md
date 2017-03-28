## [`Rx.Observable.for(sources, resultSelector, [thisArg])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/for.js)

![for](http://reactivex.io/documentation/operators/images/for.png)

将`observable`或`Promises`通过运行指定的结果选择为每个元素获得源。这种方法在浏览器IE9以下有一个别名叫做`forIn`。

#### 参数
1. `sources` *(Array)*: 将值转换为可观察序列的数组。
2. `resultSelector` *(`Function`)*: 函数，用于将源数组中的每个项将其转换为可观察序列。`resultselector`提供以下信息：
      - `元素的值`
      - `元素下标`
      - `被订阅的可观测对象`

3. `[thisArg]` *(`Any`)*: 当`resultSelector`函数执行时，使用该对象参数作为 `this`.

#### 返回值
*(`Observable`)*: Observable

#### 例


##### [Using Observables](http://jsbin.com/bocec/2/edit?js,console)

```js
/* Using Observables */
var array = [1, 2, 3];

var source = Rx.Observable.for(
    array,
    x => Rx.Observable.returnValue(x)
);

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: 1
// => onNext: 2
// => onNext: 3
// => onCompleted
```

##### [Using Promises](http://jsbin.com/febuc/2/edit?js,console)

```js
/* Using Promises */
var array = [1, 2, 3];

var source = Rx.Observable.for(
    array,
    x => RSVP.Promise.resolve(x)
);

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: 1
// => onNext: 2
// => onNext: 3
// => onCompleted
```
