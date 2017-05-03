# 实现你自己的 Observable 操作符 #

你可以通过添加新的操作符来拓展 RxJS 库中并没有提供的方法，或者通过创建实现你自己的标准操作符去提高性能以及可读性。当您想对内存中的对象进行操作时，或当定制的自定义操作不需要对查询进行全面查看时，编写标准运算符的自定义版本是非常有用的。

## 创建新的操作符 ##

RxJS 提供了比较全面的操作符，覆盖了大部分高频操作。但是，您可能需要一个运算符向查询中添加特定语义含义，特别是如果您可以在代码中多次重复使用相同的运算符。添加新操作符是扩展 RxJS 能力的一种方式。但是，你也可以通过封装现有的操作符来提高代码的可读性以及适用性。

举个例子， 让我们看看我们会如何通过封装 [Lo-Dash](http://lodash.com/) 或 [Underscore](http://underscorejs.org/) 实现 [_.where](http://lodash.com/docs#where) 方法，它需要一组属性并且对等式进行深入的比较。我们可以试着从零开始使用 `Rx.Observable.create` 方法去实现，像下面这样：

```js
Rx.Observable.prototype.filterByProperties = properties => {
	var source = this,
		comparer = Rx.internals.isEqual;

	return Rx.Observable.create(observer => {
		// Our disposable is the subscription from the parent
		return source.subscribe(
			data => {

				try {
					var shouldRun = true;

					// Iterate the properties for deep equality
					for (var prop in properties) {
						if (!comparer(properties[prop], data[prop])) {
							shouldRun = false;
							break;
						}
					}
				} catch (e) {
					observer.onError(e);
				}

				if (shouldRun) {
					observer.onNext(data);
				}
			},
			observer.onError.bind(observer),
			observer.onCompleted.bind(observer)
		);
	});
};
```

许多已存在的操作符，像这个，相反，可以使用其他基本运算符，例如在这种情况下， `filter` 或 `where`。事实上，许多已存在的操作符是通过其他更底层的操作符实现的。举个例子， `flatMap` 或 `selectMany` 操作符是通过组合 `map` 或 `select` 以及 `mergeObservable` 操作符实现的，像如下代码展示那样：

```js
Rx.Observable.prototype.flatMap = selector => this.map(selector).mergeObservable();
```

我们可以重写它以充分利用现有的操作符。

```js
Rx.Observable.prototype.filterByProperties = properties => {
	var comparer = Rx.internals.isEqual;

	return this.filter(data => {

		// Iterate the properties for deep equality
		for (var prop in properties) {
			if (!comparer(properties[prop], data[prop])) {
				return false;
			}
		}

		return true;
	});
};
```

当你创建一个新操作符时复用现有的操作符， 你可以利用 RxJS 库现有的性能和异常处理能力。编写一个自定义的操作符时，这是很好的做法，不留下任何一次性代码；否则，你可以会发现资源实际上可能泄露或取消没有正常工作。

## 测试你的新操作符 ##

只实现你的新操作符并不意味着你的工作已经完成。 现在，让我们写一些在 [测试与调试](testing.md) 文章里学到的测试来验证一下它们的表现。我们会复用 `collectionAssert.assertEqual` 从之前的文章中，所以这里就不再重复了。

```js
var onNext = Rx.ReactiveTest.onNext,
    onCompleted = Rx.ReactiveTest.onCompleted,
    subscribe = Rx.ReactiveTest.subscribe;

test('filterProperties should yield with match', () => {

    var scheduler = new Rx.TestScheduler();

    var input = scheduler.createHotObservable(
        onNext(210, { 'name': 'curly', 'age': 30, 'quotes': ['Oh, a wise guy, eh?', 'Poifect!'] }),
        onNext(220, { 'name': 'moe', 'age': 40, 'quotes': ['Spread out!', 'You knucklehead!'] }),
        onCompleted(230)
    );

    var results = scheduler.startWithCreate(
        () => input.filterByProperties({ 'age': 40 })
    );

    collectionAssert.assertEqual(results.messages, [
        onNext(220, { 'name': 'moe', 'age': 40, 'quotes': ['Spread out!', 'You knucklehead!'] }),
        onCompleted(230)
    ]);

    collectionAssert.assertEqual(input.subscriptions, [
    	subscribe(200, 230)
    ]);
});
```

为了让测试成功通过， 我们应该检测没有数据，空，单一匹配，多个匹配等情况。

## 相关内容 ##

**资源**
- [测试与调试](testing.md)
