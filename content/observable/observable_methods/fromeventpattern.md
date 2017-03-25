## [`Rx.Observable.fromEventPattern(addHandler, [removeHandler], [selector])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/fromeventpattern.js)

Creates an observable sequence by using the addHandler and removeHandler functions to add and remove the handlers, with an optional selector function to project the event arguments.

#### Arguments
1. `addHandler` *(`Function`)*: The DOMElement, NodeList or EventEmitter to attach a listener.
2. `removeHandler` *(`Function`)*: The optional function to remove a handler from an emitter.
3. `[selector]` *(`Function`)*: A selector which takes the arguments from the event handler to produce a single item to yield on next.

#### Returns
*(`Observable`)*: An observable sequence of events from the specified element and the specified event.

#### Example

Wrapping an event from [jQuery](http://jquery.com)

[](http://jsbin.com/wihiw/1/embed?js,console)

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
