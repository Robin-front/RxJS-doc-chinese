## [`Observable.prototype.thenDo(selector)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/thendo.js)

Matches when the observable sequence has an available value and projects the value.

#### 参数
1. `selector` *(Function)*: A function that will be invoked for values in the source sequence.

#### 返回值
*(Plan)*: Plan that produces the projected values, to be fed (with other plans) to the when operator.