# 查询可观测序列 #

在[事件桥接](bridging_to_events.md) 一文中，我们将现有的DOM和Node.js事件转换成可观察的序列以订阅它们。在本主题中，我们将把可观察序列的父级class视为IObservable对象，其中Rx组件提供通用LINQ操作符来操作这些对象。大多数操作符获取可观察的序列并对其执行一些逻辑并输出另一个可观测序列。另外，从代码示例可以看出，甚至可以在源序列上使用多个运算符，最终将结果序列调整到您的确切需求。

## 使用不同的运算符 ##

我们已经在以前的主题中使用[`create`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservablecreatesubscribe)和[`range`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservablerangestart-count-scheduler)运算符来创建和返回简单的序列。我们还使用[`fromEvent`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservablefromeventelement-eventname)和[`fromEventPattern`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservablefromeventpatternaddhandler-removehandler)运算符将现有事件转换成可观察的序列。在本主题中，我们将使用其他`Observable`类型的运算符，以便可以过滤，分组和转换数据。这些运算符将可观察到的序列作为输入，并生成输出另一个可观察序列。

> We have already used the [`create`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservablecreatesubscribe) and [`range`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservablerangestart-count-scheduler) operators in previous topics to create and return simple sequences. We have also used the [`fromEvent`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservablefromeventelement-eventname) and [`fromEventPattern`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservablefromeventpatternaddhandler-removehandler) operators to convert existing 】 events into observable sequences. In this topic, we will use other operators of the `Observable` type so that you can filter, group and transform data. Such operators take observable sequence(s) as input, and produce observable sequence(s) as output.

## 组合不同序列 ##

在本节中，我们会研究将各种可观察序列组合成单个可观察序列的操作符。请注意，当我们组合序列时，数据不会被转换。在以下示例中，我们使用Concat运算符将两个序列组合成一个序列并订阅它。为了说明的目的，我们将使用非常简单的`range(x, y)`运算符创建一个从x开始的整数序列，然后产生y个序列数字。

> In this section, we will examine some of the operators that combine various observable sequences into a single observable sequence. Notice that data is not transformed when we combine sequences.
In the following sample, we use the Concat operator to combine two sequences into a single sequence and subscribe to it. For illustration purpose, we will use the very simple `range(x, y)` operator to create a sequence of integers that starts with x and produces y sequential numbers afterwards.

```js
var source1 = Rx.Observable.range(1, 3);
var source2 = Rx.Observable.range(1, 3);

source1.concat(source2)
   .subscribe(console.log.bind(console));

// => 1
// => 2
// => 3
// => 1
// => 2
// => 3
```

注意，结果序列是1,2,3,1,2,3。这是因为当您使用[`concat`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypeconcatargs)运算符时，第二个序列（source2）将在第一个序列（source1）完成推送其所有值之后才会激活。只有在source1完成之后，source2才会将值推送到最后的序列。然后，订阅者将从得到的序列中获取所有值。

> Notice that the resultant sequence is 1,2,3,1,2,3. This is because when you use the [`concat`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypeconcatargs) operator, the 2nd sequence (source2) will not be active until after the 1st sequence (source1) has finished pushing all its values. It is only after source1 has completed, then source2 will start to push values to the resultant sequence. The subscriber will then get all the values from the resultant sequence.

与[`merge`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypemergemaxconcurrent--other) 操作符进行比较。如果运行以下示例代码，您将获得1,1,2,2,3,3。这是因为两个序列同时处于活动状态，并且值在数据源中发生时被推出。结果序列仅在最后一个数据源完成推送值时完成。

> Compare this with the [`merge`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypemergemaxconcurrent--other) operator. If you run the following sample code, you will get 1,1,2,2,3,3. This is because the two sequences are active at the same time and values are pushed out as they occur in the sources. The resultant sequence only completes when the last source sequence has finished pushing values.

```js
var source1 = Rx.Observable.range(1, 3);
var source2 = Rx.Observable.range(1, 3);

source1.merge(source2)
   .subscribe(console.log.bind(console));

// => 1
// => 1
// => 2
// => 2
// => 3
// => 3
```

[`catch`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypecatchsecond--handler) 操作符可以进行另一个比较。在这种情况下，如果source1完成没有任何错误，那么source2将不会启动。因此，如果运行以下示例代码，则获得1,2,3因为source2（产生4,5,6））被忽略。

> Another comparison can be done with the [`catch`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypecatchsecond--handler) operator. In this case, if source1 completes without any error, then source2 will not start. Therefore, if you run the following sample code, you will get 1,2,3 only since source2 (which produces 4,5,6) is ignored.

```js
var source1 = Rx.Observable.range(1, 3);
var source2 = Rx.Observable.range(4, 3);

source1.catch(source2)
   .subscribe(console.log.bind(console));

// => 1
// => 2
// => 3
```

最后，我们来看看[`onErrorResumeNext`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypeonerrorresumenextsecond)。即使由于错误导致source1无法完成，该操作符也将移动到source2。在以下示例中，即使source1表示通过使用[`throw`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservablethrowexception-scheduler)运算符终止异常的序列，用户将接收source2发布的值（1,2,3）。因此，如果您预期到任何一个源序列产生任何错误，那么使用它`onErrorResumeNext`来保证用户仍然会收到一些值是更安全的。

> Finally, let’s look at [`onErrorResumeNext`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypeonerrorresumenextsecond). This operator will move on to source2 even if source1 cannot be completed due to an error. In the following example, even though source1 represents a sequence that terminates with an exception by using the [`throw`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservablethrowexception-scheduler) operator, the subscriber will receive values (1,2,3) published by source2. Therefore, if you expect either source sequence to produce any error, it is a safer bet to use [`onErrorResumeNext`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypeonerrorresumenextsecond) to guarantee that the subscriber will still receive some values.

```js
var source1 = Rx.Observable.throw(new Error('An error has occurred.'));
var source2 = Rx.Observable.range(1, 3);

source1.onErrorResumeNext(source2)
   .subscribe(console.log.bind(console));

// => 1
// => 2
// => 3
```

## 映射 ##

[`select`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypeselectselector-thisarg)或 [`map`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypemapselector-thisarg) 操作符将可观察到的一个序列的每个元素转换成另一种形式。

> The [`select`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypeselectselector-thisarg) or [`map`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypemapselector-thisarg) operator can translate each element of an observable sequence into another form.

在下面的示例中，我们将一系列字符串映射到一系列表示长度的整数中。

> In the following example, we project a sequence of strings into a series of integers representing the length.

```js
var array = ['Reactive', 'Extensions', 'RxJS'];

var seqString = Rx.Observable.from(array);

var seqNum = seqString.map(x => x.length);

seqNum
   .subscribe(console.log.bind(console));

// => 8
// => 10
// => 4
```

在以下示例中，我们在“[桥接现有事件](bridging_to_events.md)”主题中看到的事件转换示例的扩展，我们使用`select`或`map`运算符将事件参数投影到x和y点。这样，我们将鼠标移动事件序列转换为可以进一步解析和操作的数据类型，如下一个“过滤”部分所示。

> In the following sample, which is an extension of the event conversion example we saw in the [Bridging with Existing Events](bridging_to_events.md) topic, we use the `select` or `map` operator to project the event arguments to a point of x and y. This way, we are transforming a mouse move event sequence into a data type that can be parsed and manipulated further, as can be seen in the next "Filtering" section.

```js
var move = Rx.Observable.fromEvent(document, 'mousemove');

var points = move.map(e => ({x: e.clientX, y: e.clientY }));

points.subscribe(
	pos => console.log('Mouse at point ' + pos.x + ', ' + pos.y));
```

最后，我们来看看[`selectMany`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypeselectmanyselector-resultselector) or [`flatMap`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypeflatmapselector-resultselector)运算符。`selectMany`或`flatMap`操作符具有许多重载，其中一个就是需要选择器函数作为参数。这个选择器函数是由数据源推出的每个值去调用的。对于每一个值，选择器将其映射成一个迷你的可观察序列。最后，`selectMany`或者`flatMap`操作符将所有这些迷你序列扁平化成单个结果序列，然后将其推送到用户。

> Finally, let’s look at the [`selectMany`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypeselectmanyselector-resultselector) or [`flatMap`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypeflatmapselector-resultselector) operator. The `selectMany` or `flatMap` operator has many overloads, one of which takes a selector function argument. This selector function is invoked on every value pushed out by the source observable. For each of these values, the selector projects it into a mini observable sequence. At the end, the `selectMany` or `flatMap` operator flattens all of these mini sequences into a single resultant sequence, which is then pushed to the subscriber.

在数据源和由选择器函数产生的所有迷你可观察序列都已经完成之后，源序列返回`selectMany`或`flatMap`发布的`onCompleted`。当发生源数据流中的错误时触发`onError`，当一个异常被选择函数抛出，或者当在任何迷你观察序列的发生了错误。

> The observable returned from `selectMany` or `flatMap` publishes `onCompleted` after the source sequence and all mini observable sequences produced by the selector have completed. It fires `onError` when an error has occurred in the source stream, when an exception was thrown by the selector function, or when an error occurred in any of the mini observable sequences.

在下面的例子中，我们首先创建一个数据源序列，每5秒产生一个整数，并决定使用生成的前两个值（使用`take`运算符）。然后，我们使用`selectMany`或者`flatMap`对另一个序列{100,101,102}这些整数进行映射。通过这样做，产生两个迷你观察序列{100,101,102}和{100,101,102}。它们最终平坦化成{100,101,102,100,101,102}的单个整数流，并被推送到观察者。

> In the following example, we first create a source sequence which produces an integer every 5 seconds, and decide to just take the first 2 values produced (by using the `take` operator). We then use `selectMany` or `flatMap` to project each of these integers using another sequence of {100, 101, 102}. By doing so, two mini observable sequences are produced, {100, 101, 102} and {100, 101, 102}. These are finally flattened into a single stream of integers of {100, 101, 102, 100, 101, 102} and pushed to the observer.

```js
var source1 = Rx.Observable.interval(5000).take(2);
var proj = Rx.Observable.range(100, 3);
var resultSeq = source1.flatMap(proj);

var subscription = resultSeq.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e.message),
  () => console.log('onCompleted'));

// => onNext: 100
// => onNext: 101
// => onNext: 102
// => onNext: 100
// => onNext: 101
// => onNext: 102
// => onCompleted
```

## 过滤 ##

在下面的例子中，我们使用[`generate`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservablegenerateinitialstate-condition-iterate-resultselector-scheduler) 运算符创建一个简单的可观察数字序列。该`generate`操作符有几个版本，包括有相对和绝对时间调度。在我们的示例中，它需要初始状态（在我们的示例中为0），一个条件函数终止（少于10次），迭代器（+1），结果选择器（当前值的平方函数））和打印只使用小于5的那些使用[`filter`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypefilterpredicate-thisarg)或[`where`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypewherepredicate-thisarg)运算符。

> In the following example, we use the [`generate`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservablegenerateinitialstate-condition-iterate-resultselector-scheduler) operator to create a simple observable sequence of numbers. The `generate` operator has several versions including with relative and absolute time scheduling. In our example, it takes an initial state (0 in our example), a conditional function to terminate (fewer than 10 times), an iterator (+1), a result selector (a square function of the current value), and prints out only those smaller than 5 using the [`filter`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypefilterpredicate-thisarg) or [`where`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypewherepredicate-thisarg) operators.

```js
var seq = Rx.Observable.generate(
	0,
	i => i < 10,
	i => i + 1,
	i => i * i);

var source = seq.filter(n => n < 5);

var subscription = source.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e.message),
  () => console.log('onCompleted'));

// => onNext: 0
// => onNext: 1
// => onNext: 4
// => onCompleted
```

以下示例是本主题前面已经看到的映射示例的扩展。在该示例中，我们使用`select`或`map`运算符将事件参数投影到具有x和y的点。在下面的例子中，我们使用`filter`或`where`和`select`或`map`操作符只挑选那些鼠标移动，我们感兴趣的是，在这种情况下，我们鼠标移动过滤，以找出在第一平分线（其中x和y坐标是相等的）。

> The following example is an extension of the projection example you have seen earlier in this topic. In that sample, we have used the `select` or `map` operator to project the event arguments into a point with x and y. In the following example, we use the `filter` or `where` and `select` or `map` operators to pick only those mouse movements that we are interested in. In this case, we filter the mouse moves to those over the first bisector (where the x and y coordinates are equal).

```js
var move = Rx.Observable.fromEvent(document, 'mousemove');

var points = move.map(e => ({ x: e.clientX, y: e.clientY }));

var overfirstbisector = points.filter(pos => pos.x === pos.y);

var movesub = overfirstbisector.subscribe(pos => console.log('mouse at ' + pos.x + ', ' pos.y));
```

## 基于时间的操作 ##

您可以使用缓冲区运算符执行基于时间的操作。

> You can use the Buffer operators to perform time-based operations.

缓冲可观察序列意味着可观测序列的值基于指定的时间段或计数阈值被放入缓冲区。这在您预期有大量数据被序列推出的情况下特别有用，并且订阅者没有资源来处理这些值。通过基于时间或计数缓冲结果，并且只有在超过条件时才返回值序列（或者源序列完成时），用户可以按照自己的速度处理`OnNext`调用。

> Buffering an observable sequence means that an observable sequence’s values are put into a buffer based on either a specified timespan or by a count threshold. This is especially helpful in situations when you expect a tremendous amount of data to be pushed out by the sequence, and the subscriber does not have the resource to process these values. By buffering the results based on time or count, and only returning a sequence of values when the criteria is exceeded (or when the source sequence has completed), the subscriber can process OnNext calls at its own pace.

在下面的例子中，我们首先创建一个以每秒为时间单位的简单的整数序列。然后我们使用`bufferWithCount`运算符，并指定每个缓冲区将保存序列中的5个项目。在`onNext`当缓冲区已满被调用。然后我们使用缓冲区的总和`Array.reduce`。缓冲区自动刷新，另一个循环开始。打印输出将为10,35,60 ...，其中10 = 0 + 1 + 2 + 3 + 4,35 = 5 + 6 + 7 + 8 + 9等。

> In the following example, we first create a simple sequence of integers for every second. We then use the `bufferWithCount` operator and specify that each buffer will hold 5 items from the sequence. The `onNext` is called when the buffer is full. We then tally the sum of the buffer using `Array.reduce`. The buffer is automatically flushed and another cycle begins. The printout will be 10, 35, 60… in which 10=0+1+2+3+4, 35=5+6+7+8+9, and so on.

```js
var seq = Rx.Observable.interval(1000);

var bufSeq = seq.bufferWithCount(5);

bufSeq
	.map(arr => arr.reduce((acc, x) => acc + x, 0))
	.subscribe(console.log.bind(console));

// => 10
// => 35
// => 60
...
```

我们还可以创建一个指定时间跨度（以毫秒为单位）的缓冲区。在以下示例中，缓冲区将保存累积3秒钟的项目。打印输出将为3,12,21 ...其中3 = 0 + 1 + 2,12 = 3 + 4 + 5，依此类推。

> We can also create a buffer with a specified time span in milliseconds. In the following example, the buffer will hold items that have accumulated for 3 seconds. The printout will be 3, 12, 21… in which 3=0+1+2, 12=3+4+5, and so on.

```js
var seq = Rx.Observable.interval(1000);

var bufSeq = seq.bufferWithTime(3000);

bufSeq
	.map(arr => arr.reduce((acc, x) => acc + x, 0))
	.subscribe(console.log.bind(console));
```

请注意，如果您使用任何一个`buffer*`或`window*`运算符，则必须确保序列不为空，然后再过滤。

> Note that if you are using any of the `buffer*` or `window*` operators, you have to make sure that the sequence is not empty before filtering on it.

## 按类别操作 ##

按[类别划分的操作符](operators_by_category.md)主题列出了按类别实施的`Observable`的所有主要操作符; 具体来说：创建，转换，合并，功能，数学，时间，异常，杂项，选择和原值。

> The [Operators by Categories](operators_by_category.md) topic lists all of the major operators implemented by the `Observable` type by their categories; specifically: creation, conversion, combine, functional, mathematical, time, exceptions, miscellaneous, selection and primitives.

## 相关阅读 ##

*参考*
 - [Observable](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md)

*概念*
- [Operators by Categories](operators_by_category.md)
