## [`Rx.helpers.defaultError(err)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/headers/basicheader.js#L11)

抛出指定的错误

#### 参数
1. `err` *(Any)*: 抛出的错误

#### 例

```js
var defaultError = Rx.helpers.defaultError;

// Returns its value
defaultError(new Error('woops'))
// => Error: woops
```
