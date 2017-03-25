## [`Rx.Observable.toAsync(func, [scheduler], [context])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/toasync.js)

{% if book.isPdf %}

![toAsync](http://reactivex.io/documentation/operators/images/toAsync.png)

{% else %}



{% endif %}

Converts the function into an asynchronous function. Each invocation of the resulting asynchronous function causes an invocation of the original synchronous function on the specified scheduler.

#### 参数
1. `func` *(`Function`)*: Function to convert to an asynchronous function.
2. `[scheduler=Rx.Scheduler.timeout]` *(`Scheduler`)*: Scheduler to run the function on. If not specified, defaults to Scheduler.timeout.
3. `[context]` *(`Any`)*: The context for the func parameter to be executed.  If not specified, defaults to undefined.

#### 返回值
*(`Function`)*: Asynchronous function.

#### 例

[](http://jsbin.com/zokawu/1/embed?js,console)
