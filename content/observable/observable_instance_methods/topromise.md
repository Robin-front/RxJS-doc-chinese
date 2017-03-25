## [`Rx.Observable.prototype.toPromise(promiseCtor)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/topromise.js)

Converts an existing observable sequence to an ES6 Compatible Promise.

#### 参数
1. `promiseCtor` *(`Function`)*: The constructor of the promise. If not provided, it looks for it in Rx.config.Promise.

#### 返回值
*(`Promise`)*: An ES6 compatible promise with the last value from the observable sequence.

#### 例

```js
var promise = Rx.Observable.return(42).toPromise(RSVP.Promise);

promise.then(console.log.bind(console));

// => 42
```

#### 例 with config

```js
Rx.config.Promise = RSVP.Promise;
var promise = Rx.Observable.return(42).toPromise();

promise.then(console.log.bind(console));

// => 42
```