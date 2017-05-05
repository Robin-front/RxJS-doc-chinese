# 事件桥接 #

RxJS 提供工厂方法来桥接 DOM 或 Node.js 中已存在的异步数据源，所以，你可以使用丰富的创作、过滤和资源管理功能对RxJS提供的任何类型的数据流进行操作。这篇文章探讨 [`fromEvent`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/fromevent.md) 和 [`fromEventPattern`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/fromeventpattern.md)操作符，它允许导入一个 DOM 或者普通事件到 RxJS 的数据流。每次引发事件时，一个 `OnNext` 消息将传递到数据流。然后，可以像其他任何数据流一样操作事件数据流。

RxJS 不打算取代现有的异步编程模型如 `Promises` 或 `callbacks`。但是，当你尝试组合事件， RxJS的工厂方法会提供简便的方法给你，你完全感受不到当前使用了何种编程模式。这真的很方便维护（比如取消订阅）和筛选（比如选择合适的数据）数据源。在本节和下节中，你可以尝试 RxJS 的这些特性如何协助你完成异步编程。

自然，RxJS 支持一批库和他们的勾子函数去使用他们的事件系统，比如 [jQuery](http://jquery.com/), [Zepto.js](http://zeptojs.com/), [AngularJS](https://angularjs.org/), [Ember.js](http://emberjs.com/) 和 [Backbone.js](http://backbonejs.org)。这种行为，不管怎样只能重写本地绑定。默认情况下， RxJS 也支持 [Node.js](http://nodejs.org) `EventEmitter` 的事件勾子。

## 将一个 DOM 事件转换成 RxJS 数据流 ##

接下来这个例子为鼠标移动事件创建了一个 DOM 事件操作，并且在页面上打印出鼠标的坐标。

```js
var result = document.getElementById('result');

document.addEventListener('mousemove', e => result.innerHTML = e.clientX + ', ' + e.clientY, false);
```

导入一个事件到 RxJS, 你可以使用 [`fromEvent`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/fromevent.md) 操作符，并且传入被桥接的事件参数。然后它会将事件转换成数据流。

下面这个例子，我们将 DOM 的 `mousemove` 事件流转换成事件流（可观察对象）。每次鼠标移动事件被触发时，订阅都会接收到一个 `onNext` 事件。然后我们可以检查这种通知的事件参数并获得鼠标的坐标。

```js
var result = document.getElementById('result');

var source = Rx.Observable.fromEvent(document, 'mousemove');

var subscription = source.subscribe(e => result.innerHTML = e.clientX + ', ' + e.clientY);
```

在这个例子中要注意，（鼠标）移动变成一个数据流以便我们进一步操作。 [Querying Observable Sequences](querying.md) 这篇文章将会展示如何将该序列投射到点类型集合中并筛选其内容，以便应用程序只接收满足一定条件的值。

事件处理程序的销毁由 `subscribe` 方法返回的 `Disposable` 对象处理。调用 `dispose` 将会释放由该序列所使用的所有资源，包括底层事件处理程序。这本质上是取消订阅事件。

`fromEvent` 方法还支持向多个项目添加事件处理程序，比如一整个 DOM 节点列表。下面这个例子将会给列表中的每个元素添加 'click' 事件。

```js
var result = document.getElementById('result');
var sources = document.querySelectorAll('div');

var source = Rx.Observable.fromEvent(sources, 'click');

var subscription = source.subscribe(e => result.innerHTML = e.clientX + ', ' + e.clientY);
```

另外，`fromEvent` 也支持类库，像 [jQuery](http://jquery.com/), [Zepto.js](http://zeptojs.com/), [AngularJS](https://angularjs.org/), [Ember.js](http://emberjs.com/) and [Backbone.js](http://backbonejs.org)：

```js
var $result = $('#result');
var $sources = $('div');

var source = Rx.Observable.fromEvent($sources, 'click');

var subscription = source.subscribe(e => $result.html(e.clientX + ', ' + e.clientY));
```

如果表现不如预期，你可以通过设置 `Rx.config.useNativeEvents` 为 `true` 去重写它，这会无视任何类库。

```js
// 只使用原生事件，尽管引用了 jQuery
Rx.config.useNativeEvents = true;

// 只使用原生事件
var result = document.getElementById('result');

var source = Rx.Observable.fromEvent(document, 'mousemove');

var subscription = source.subscribe(e => result.innerHTML = e.clientX + ', ' + e.clientY);
```

另外，您可以轻松地给事件系统的事件添加许多快捷方式，比如 `mousemove`， 甚至是 [Pointer](http://www.w3.org/TR/pointerevents/) and [Touch](http://www.w3.org/TR/touch-events/) 事件。

```js
Rx.dom = {};

var events = "blur focus focusin focusout load resize scroll unload click dblclick " +
  "mousedown mouseup mousemove mouseover mouseout mouseenter mouseleave " +
  "change select submit keydown keypress keyup error contextmenu";

if (root.PointerEvent) {
  events += " pointerdown pointerup pointermove pointerover pointerout pointerenter pointerleave";
}

if (root.TouchEvent) {
  events += " touchstart touchend touchmove touchcancel";
}

events.split(' ').forEach(e => {
  Rx.dom[e] = (element, selector) => Rx.Observable.fromEvent(element, e, selector)
});
```

现在我们可以重写单个鼠标拖拽事件：

```js
var draggable = document.getElementById('draggable');

var mousedrag = Rx.dom.mousedown(draggable).flatMap(md => {
  md.preventDefault();

  var start = getLocation(md);

  return Rx.dom.mousemove(document)
    .map(mm => getDelta(start, mm))
    .takeUntil(Rx.dom.mouseup(draggable));
});
```

注意这在 [RxJS-DOM](https://github.com/Reactive-Extensions/RxJS-DOM) 项目中已经可用，但你自己实现也只需要很少量的代码。

## 将 Node.js 事件转换成 RxJS 数据流 ##

Node.js 也支持类似 [`EventEmitter`](http://nodejs.org/api/events.html#events_class_events_eventemitter):

```js
var Rx = require('rx'),
  EventEmitter = require('events').EventEmitter;

var eventEmitter = new EventEmitter();

var source = Rx.Observable.fromEvent(eventEmitter, 'data')

var subscription = source.subscribe(data => console.log('data: ' + data));

eventEmitter.emit('data', 'foo');
// => data: foo
```

## 使用 FromEventPattern 桥接自定义事件 ##

下面有一个使用类库实现事件订阅和退订的实例。[`fromEventPattern`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/fromeventpattern.md) 方法就是为了这个目的而创建的，用来桥接这些自定义事件。

举个例子，你可以想使用 jQuery [`on`](http://api.jquery.com/on/) 方法去桥接。我们可以将下列代码转换为基于表格行单击的 alert。


```js
$( "#dataTable tbody" ).on('click', 'tr', e => alert($( e.target ).text()));
```

使用 `fromEventPattern` 方法转换后的代码看起来像下面这样。每个函数在处理函数中传递，允许您调用 `on` 和 `off` 方法来正确处理事件的处理。

```js
var $tbody = $('#dataTable tbody');

var source = Rx.Observable.fromEventPattern(
  function addHandler (h) { $tbody.on('click', 'tr', h); },
  function delHandler (h) { $tbody.off('click', 'tr', h); });

var subscription = source.subscribe(e => alert($( e.target ).text()));
```

除了这种常用的支持外，我们也支持 `addHandler` 返回一个对象，它可以通过 `removeHandler` 去完全退订。在这个例子中，我们将使用 [Dojo Toolkit](http://dojotoolkit.org) 和 [`on`](http://dojotoolkit.org/api/1.9/dojo/on.html) 模块。

```js
require(['dojo/on', 'dojo/dom', 'rx', 'rx.async', 'rx.binding'], (on, dom, rx) => {

    var input = dom.byId('input');

    var source = Rx.Observable.fromEventPattern(
        function addHandler (h) {
            return on(input, 'click', h);
        },
        function delHandler (_, signal) {
            signal.remove();
        }
    );

    var subscription = source.subscribe(
        x => console.log('Next: Clicked!'),
        err => console.log('Error: ' + err),
        () => console.log('Completed'));

    on.emit(input, 'click');
    // => Next: Clicked!
});
```

## 相关内容

概念
- [Querying Observable Sequences](querying_observable_sequences.md)
