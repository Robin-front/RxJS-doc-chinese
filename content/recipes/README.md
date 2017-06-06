# RxJS Recipes(秘方？)

## 错误处理

在RxJS中有许多处理错误的方法。这将涵盖处理错误时的许多方法：

* 增量重试 ```retryWhen```

### 增量重试 ```retryWhen```

此示例将显示如何在几秒钟之后尝试重试，而不是只重试一次。

{% if book.isPdf %}

#### [Example](http://jsbin.com/pizomo/3/edit?js,console)

```js
function identity(x) { return x; }

Rx.Observable.create(o => {
      console.log('subscribing');
      o.onError(new Error('always fails'));
  }).retryWhen(attempts => {
      return Rx.Observable.range(1, 3)
        .zip(attempts, identity)
        .flatMap(i => {
          console.log('delay retry by ' + i + ' second(s)');
          return Rx.Observable.timer(i * 1000);
      });
  }).subscribe();

// => subscribing
// => delay retry by 1 second(s)
// => subscribing
// => delay retry by 2 second(s)
// => subscribing
// => delay retry by 3 second(s)
// => subscribing

```

{% else %}

#### 例
[](http://jsbin.com/pizomo/3/embed?js,console)

{% endif %}
