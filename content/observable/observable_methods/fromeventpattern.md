## [`Rx.Observable.fromEventPattern(addHandler, [removeHandler], [selector])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/fromeventpattern.js)

通过创建使用AddHandler和removeHandler功能添加和删除处理，具有可选的选择功能项目事件参数的观察到的序列。

#### 参数
1. `addHandler` *(`Function`)*: 一个DOMElement，节点列表或EventEmitter附加的监听器。
2. `removeHandler` *(`Function`)*: 可选的功能，以从事件发射端移除的处理程序。
3. `[selector]` *(`Function`)*: 一个选择器，它从事件处理程序中获取参数，以便在下一个项目上生成一个项目。

#### 返回值
*(`Observable`)*: 来自指定元素和指定事件的可观察事件序列。

#### 例

Wrapping an event from [jQuery](http://jquery.com)

[](http://jsbin.com/wihiw/1/embed?js,console)

[](codepen://Lingyucoder/AsFJh?height=800&theme=0)

Wrapping an event from the [Dojo Toolkit](http://dojotoolkit.org)

```js
require(['dojo/on', 'dojo/dom', 'rx', 'rx.async', 'rx.binding'], function (on, dom, rx) {

    var input = dom.byId('input');

    var source = Rx.Observable.fromEventPattern(
        function add (h) {
            return on(input, 'click', h);
        },
        function remove (_, signal) {
            signal.remove();
        }
    );

    var subscription = source.subscribe(
        function (x) {
            console.log('Next: Clicked!');
        },
        function (err) {
            console.log('Error: ' + err);   
        },
        function () {
            console.log('Completed');   
        });

    on.emit(input, 'click');
    // => Next: Clicked!
});
```

Using in Node.js with using an `EventEmitter`.

```js
var EventEmitter = require('events').EventEmitter,
    Rx = require('rx');

var e = new EventEmitter();

// Wrap EventEmitter
var source = Rx.Observable.fromEventPattern(
    function add (h) {
        e.addListener('data', h);
    },
    function remove (h) {
        e.removeListener('data', h);
    },
    function (arr) {
        return arr[0] + ',' + arr[1];
    }
);

var subscription = source.subscribe(
    function (result) {
        console.log('Next: ' + result);
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });


e.emit('data', 'foo', 'bar');
// => Next: foo,bar
```
