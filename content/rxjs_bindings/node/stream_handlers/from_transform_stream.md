# fromTransformStream

### <a id="rxnodefromtransformstreamstream"></a>`Rx.Node.fromTransformStream(stream)`
<a href="#rxnodefromtransformstreamstream">#</a> [&#x24C8;](https://github.com/Reactive-Extensions/RxJS/blob/master/rx.node.js#L141-L143 "View in source")

Converts a flowing transform stream to an Observable sequence.

#### 参数
1. `stream` *(Stream)*: A stream to convert to a observable sequence.

#### 返回值
*(Observable)*: An observable sequence which fires on each 'data' event as well as handling 'error' and 'finish' events.

#### 例
```js
var Rx = require('rx');

var subscription = Rx.Node.fromTransformStream(getTransformStreamSomehow());
```

{% if book.isPdf %}



{% else %}

### Location

- rx.node.js

{% endif %}