## [`Rx.Observable.ofWithScheduler([scheduler], ...args)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/of.js)

将一系列参数转换成 observable 序列, 并使用指定的调度器枚举参数。

#### 参数
1. `[scheduler]` *(Scheduler)*: 用于枚举参数的可选调度程序。
2. `args` *(Arguments)*: 将参数转换为可观察序列的参数列表.

#### 返回值
*(`Observable`)*: 一个将参数值作为返回值的observable。

#### 例

[](http://jsbin.com/jabup/1/embed?js,console)
