## [`Rx.Observable.prototype.startWith([scheduler] ...args)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/startwith.js)

{% if book.isPdf %}

![startWith](http://reactivex.io/documentation/operators/images/startWith.png)

{% else %}

<rx-marbles key="startWith"></rx-marbles>

{% endif %}

Prepends a sequence of values to an observable sequence with an optional scheduler and an argument list of values to prepend.

#### 参数
1. `[scheduler]` *(`Scheduler`)*: Scheduler to execute the function.
2. `args` *(arguments)*: Values to prepend to the observable sequence.

#### 返回值
*(`Observable`)*: The source sequence prepended with the specified values.

#### 例

[](http://jsbin.com/beqot/1/embed?js,console)
