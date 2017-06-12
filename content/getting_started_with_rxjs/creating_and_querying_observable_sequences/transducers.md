# 可观察序列的 Transducers  #

就像语言集成查询（LINQ），转换器是可组合的算法转换。他们像LINQ，独立于它们的输入和输出源的上下文和本质上指定只转换一个单独的元素。因为转换器从输入或输出源解耦，它们可以在许多不同的过程中使用-集合，流，可观察等。转换器直接组合，没有输入或中间件的生成约定。目前有两个主要的类库在那里，Cognitect的[`transducer-js`](https://github.com/cognitect-labs/transducers-js)和詹姆斯·隆恩的[`transducers.js`](https://github.com/jlongster/transducers.js)这两个的高性能是获得了大量数据验证的。因为它的集合类型的中性，它是RxJS做变换大集合最合适的。

> Much like Language Integrated Query (LINQ), Transducers are composable algorithmic transformations. They, like LINQ, are independent from the context of their input and output sources and specify only the essence of the transformation in terms of an individual element. Because transducers are decoupled from input or output sources, they can be used in many different processes - collections, streams, observables, etc. Transducers compose directly, without awareness of input or creation of intermediate aggregates.  There are two major libraries currently out there, Cognitect's [`transducer-js`](https://github.com/cognitect-labs/transducers-js) and James Long's [`transducers.js`](https://github.com/jlongster/transducers.js) which are both great for getting high performance over large amounts of data.  Because it is collection type neutral, it is a perfect fit for RxJS to do transformations over large collections.

这个词`transduce`只是`transform`和`reduce` 的一个组合。reduce 函数是transformation 的基础; 任何其他的变换可以用它来表示（`map`，`filter`等等）。

> The word `transduce` is just a combination of `transform` and `reduce`. The reduce function is the base transformation; any other transformation can be expressed in terms of it (`map`, `filter`, etc).

```js
var arr = [1, 2, 3, 4];

arr.reduce((result, x) => result.concat(x + 1), []);

// => [ 2, 3, 4, 5 ]
```

使用传感器，我们可以模拟下面的表现，当破坏 map 中 concat操作 加1的部分，然后再加各部分“收集”起来放入转换器。

> Using transducers, we can model the following behavior while breaking apart the map aspect of adding 1 to the concat operation, adding the seed and then the "collection" to transduce.

```js
var arr = [1, 2, 3, 4];

function increment(x) { return x + 1; }
function concatItem(acc, x) { return acc.concat(x); }

transduce(map(increment), concatItem, [], arr);

// => [ 2, 3, 4, 5 ]
```

使用Cognitect的[`transducers-js`](https://github.com/cognitect-labs/transducers-js) 库，我们可以很轻松地完成我们上面做的事情。

> Using Cognitect's [`transducers-js`](https://github.com/cognitect-labs/transducers-js) library, we can easily accomplish what we had above.  

```js
var t = transducers;

var arr = [1, 2, 3, 4];

function increment(x) { return x + 1; }

into([], t.comp(t.map(increment)), arr);

// => [ 2, 3, 4, 5 ]
```

我们可以更进一步，添加过滤以及只获取偶数值。

> We can go a step further and add filtering as well to get only even values.

```js
var t = transducers;

var arr = [1, 2, 3, 4];

function increment(x) { return x + 1; }
function isEven(x) { return x % 2 === 0; }

into([], t.comp(t.map(increment), t.filter(isEven)), arr);

// => [ 2, 4 ]
```

由于它能数组操作十分友好，没有理由不能与可观察序列配合正常工作。为此，我们已经介绍了[`transduce`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/transduce.md)，其作用完全像它为数组设计的，但除了可观察序列。再次，让我们过一遍上面的例子，这次我们使用可观察序列。

> Since it works so well using Arrays, there's no reason why it cannot work for Observable sequences as well.  To that end, we have introduced the [`transduce`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/transduce.md) method which acts exactly like it does for Arrays, but for Observable sequences.  Once again, let's go over the above example, this time using an Observable sequence.

```js
var t = transducers;

var source = Rx.Observable.range(1, 4);

function increment(x) { return x + 1; }
function isEven(x) { return x % 2 === 0; }

var transduced = source.transduce(t.comp(t.map(increment), t.filter(isEven)));

transduced.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// => Next: 2
// => Next: 4
// => Completed
```

请注意，这上面的例子同样使用`transducers.js`工作得很好，几乎没有修改。这个例子实际上将跑得比目前使用的大部分传统的LINQ风格更快（截至目前）。

> Note that this above example also works the same with `transducers.js` as well with little to no modification.  This example will in fact work faster than the traditional LINQ style (as of now) which most use currently.

```js
var source = Rx.Observable.range(1, 4);

function increment(x) { return x + 1; }
function isEven(x) { return x % 2 === 0; }

var transduced = source.map(increment).filter(isEven);

transduced.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// => Next: 2
// => Next: 4
// => Completed
```

这开辟了新的可能性，使RxJS比以前更快地遍历大数据，没有中间的可观察序列。

> This opens up a wide new set of possibilities making RxJS even faster over large collections with no intermediate Observable sequences.
