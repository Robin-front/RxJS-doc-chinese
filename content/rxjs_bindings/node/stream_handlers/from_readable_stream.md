#fromReadableStream

### <a id="rxnodefromreadablestreamstream"></a>`Rx.Node.fromReadableStream(stream)`
<a href="#rxnodefromreadablestreamstream">#</a> [&#x24C8;](https://github.com/Reactive-Extensions/RxJS/blob/master/rx.node.js#L123-L125 "View in source")

Converts a flowing readable stream to an Observable sequence.

#### 参数
1. `stream` *(Stream)*: A stream to convert to a observable sequence.

#### 返回值
*(Observable)*: An observable sequence which fires on each 'data' event as well as handling 'error' and 'end' events.

#### 例
```js
var Rx = require('rx');

var subscription = Rx.Node.fromReadableStream(process.stdin)
    .subscribe(function (x) { console.log(x); });

// => r<Buffer 72>
// => x<Buffer 78>
```

{% if book.isPdf %}



{% else %}

### Location

- rx.node.js

{% endif %}