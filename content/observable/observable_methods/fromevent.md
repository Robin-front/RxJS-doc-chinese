## [`Rx.Observable.fromEvent(element, eventName, [selector])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/fromevent.js)

{% if book.isPdf %}

![fromEvent](http://reactivex.io/documentation/operators/images/fromEvent.png)

{% else %}



{% endif %}

Creates an observable sequence by adding an event listener to the matching DOMElement, jQuery element, Zepto Element, Angular element, Ember.js element or EventEmitter.

Note that this uses the library approaches for jQuery, Zepto, Backbone.Marionette, AngularJS and Ember.js and falls back to native binding if not present. If you are using AMD you may need to include these libraries as dependencies of RxJs in your requirejs configuration file. RxJs will attempt to detect their presence when deciding which library to use.

#### 参数
1. `element` *(`Any`)*: The DOMElement, NodeList, jQuery element, Zepto Element, Angular element, Ember.js element or EventEmitter to attach a listener. For Backbone.Marionette this would be the application or an EventAggregator object.
2. `eventName` *(`String`)*: The event name to attach the observable sequence.
3. `[selector]` *(`Function`)*: A selector which takes the arguments from the event emitter so that you can return a single object.

#### 返回值
*(`Observable`)*: An observable sequence of events from the specified element and the specified event.

#### 例

Wrapping an event from [jQuery](http://jquery.com)

[](http://jsbin.com/kemudu/1/embed?js,console)

Using in Node.js with using an `EventEmitter` with a selector function (which is not required).

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