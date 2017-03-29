## [`Rx.Observable.fromEvent(element, eventName, [selector])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/fromevent.js)

![fromEvent](http://reactivex.io/documentation/operators/images/fromEvent.png)

从DOM事件或Node EventEmitter事件或其他事件创建一个Observable。
Creates an observable sequence by adding an event listener to the matching DOMElement, jQuery element, Zepto Element, Angular element, Ember.js element or EventEmitter.

请注意，这里使用jQuery，Zepto，Backbone.Marionette，AngularJS和Ember.js库的方法，如果不存在，将会自动使用原生事件绑定。如果使用的是AMD可能需要包含这些库在你requirejs配置文件RxJs的依赖。RxJs将在决定使用哪个库时，试图检测到它们的存在。

#### 参数
1. `element` *(`Any`)*: DOMElement，事件目标，Node.js EventEmitter，NodeList或HTMLCollection来附加事件处理程序。
2. `eventName` *(`String`)*: 事件名
3. `[selector]` *(`Function`)*: 后期处理结果的可选功能。它接受来自事件处理程序的参数，并返回一个值。

#### 返回值
*(`Observable`)*: observable

#### 例

通过 [jQuery](http://jquery.com)封装事件

[](http://jsbin.com/kemudu/1/embed?js,console)

使用Node.js的`EventEmitter`作为选择器函数（这不是必须的）。

```js
var EventEmitter = require('events').EventEmitter,
    Rx = require('rx');

var eventEmitter = new EventEmitter();

var source = Rx.Observable.fromEvent(
    eventEmitter,
    'data',
    function (args) {
        return { foo: args[0], bar: args[1] };
    });

var subscription = source.subscribe(
    function (x) {
        console.log('Next: foo -' x.foo + ', bar -' + x.bar);
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

eventEmitter.emit('data', 'baz', 'quux');
// => Next: foo - baz, bar - quux
```
