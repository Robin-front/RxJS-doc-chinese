## [`Rx.Observable.when(...args)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/when.js)

![when](http://reactivex.io/documentation/operators/images/and_then_when.C.png)

通过使用Then运算符在模式上创建的一系列计划（指定为数组或一系列参数）

#### 参数
1. `args` *(`arguments`|`Array`)*: 通过使用Then运算符在模式上创建的一系列计划（指定为数组或一系列参数）

#### 返回值
*(`Observable`)*: Observable结果序列与几种模式匹配

#### 例

```js
// Fire each plan when both are ready
var source = Rx.Observable.when(
  Rx.Observable.timer(100).and(Rx.Observable.timer(500)).then(function (x, y) { return 'first'; }),
  Rx.Observable.timer(400).and(Rx.Observable.timer(300)).then(function (x, y) { return 'second'; })
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


// => Next: second
// => Next: first
// => Completed
```

[](http://jsbin.com/vobuh/1/embed?js,console)

```js
var chopsticks = [new Rx.Subject(), new Rx.Subject(), new Rx.Subject()];

var hungry = [new Rx.Subject(), new Rx.Subject(), new Rx.Subject()];

var eat = i => {
  return () => {
    setTimeout(() => {
      console.log('Done');
      chopsticks[i].onNext({});
      chopsticks[(i+1) % 3].onNext({});
    }, 1000);
    return 'philosopher ' + i + ' eating';
  };
};

var dining = Rx.Observable.when(
  hungry[0].and(chopsticks[0]).and(chopsticks[1]).thenDo(eat(0)),
  hungry[1].and(chopsticks[1]).and(chopsticks[2]).thenDo(eat(1)),
  hungry[2].and(chopsticks[2]).and(chopsticks[0]).thenDo(eat(2))
);

dining.subscribe(console.log.bind(console));

chopsticks[0].onNext({}); chopsticks[1].onNext({}); chopsticks[2].onNext({});

for (var i = 0; i < 3; i++) {
  hungry[0].onNext({}); hungry[1].onNext({}); hungry[2].onNext({});
}
```
