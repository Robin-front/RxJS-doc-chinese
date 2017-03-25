## [`Rx.config.Promise`]()

设置当[`toPromise`](../observable/observable_instance_methods/topromise.html)方法被调用时要使用的默认的Promise类型。请注意，Promise实施必须符合ES6规范。其中一些支持的库是[Q](https://github.com/kriskowal/q), [RSVP](https://github.com/tildeio/rsvp.js), [when.js](https://github.com/cujojs/when)等。如果未指定，则默认为原生ES6 Promise（如果可用），否则将抛出错误。

#### 例

```js
Rx.config.Promise = RSVP.Promise;

var p = Rx.Observable.just(1).toPromise()
  .then(value => console.log('Value: %s', s));
// => Value: 1
```
