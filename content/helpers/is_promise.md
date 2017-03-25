## [`Rx.helpers.isPromise(p)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/headers/basicheader.js#L12)

确定对象是否为`Promise`对象的函数。

#### 参数
1. `p` *(Any)*: 需要判定是否为 `Promise`对象的对象.

#### 返回值
*(Boolean)*: 如果传入对象是 `Promise` 对象，则返回`true` ，否则 `false`

#### 例

```js
var isPromise = Rx.helpers.isPromise;

var p = RSVP.Promise(res => res(42));

console.log(isPromise(p));
// => true
```
