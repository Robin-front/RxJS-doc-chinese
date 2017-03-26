## [`Rx.Observable.case(selector, sources, [elseSource|scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/case.js)

![case](http://reactivex.io/documentation/operators/images/case.png)

使用选择器来决定要使用源列表的哪个源。IE9浏览器以下有一个别名`switchCase`。

#### 参数
1. `selector` *(`Function`)*: 其提取用于在case语句来测试值的函数。
2. `sources` *(`Object`)*: 一个包含 key-value 的源列表的对象。
3. `[elseSource|scheduler]` *(`Observable` | `Scheduler`)*: 如果源列表不匹配时，将要运行的可观察序列。如果不提供，则默认为`Rx.Observabe.empty`。

#### 返回值
*(`Observable`)*: 这是由一个case语句确定的可观察序列。

#### [Example](http://jsbin.com/ladamu/2/edit?js,console)

```js
var sources = {
    'foo': Rx.Observable.return(42),
    'bar': Rx.Observable.return(56)
};

var defaultSource = Rx.Observable.empty();

var source = Rx.Observable.case(
    () => 'foo',
    sources,
    defaultSource);

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

//=> onNext: 42
//=> onCompleted
```

#### 联想与应用

case 用于判断，可以用于客服系统，如淘宝店小二，微信机器人自动分类客服。

```
var config = {
    "国际机票客服": Observable.return("国际机票客服"),
    "国内机票客服": Observable.return("国际机票客服"),
    "其他客服": Observable.return("其他客服")
};
Observable.case(()=>'国际机票客服', config, Observable.return("默认客服"))
    .subscribe(() => {
        // 连接国际机票客服
    })
Observable.case(()=>'国内机票客服', config, Observable.empty())
    .subscribe(() => {
        // 连线国内机票客服
    })
```
