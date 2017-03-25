## [`Rx.Observable.defer(observableFactory)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/defer.js)

{% if book.isPdf %}

![defer](http://reactivex.io/documentation/operators/images/defer.png)

{% else %}



{% endif %}

Returns an observable sequence that invokes the specified factory function whenever a new observer subscribes.

#### 参数
1. `observableFactory` *(`Function`)*: Observable factory function to invoke for each observer that subscribes to the resulting sequence.

#### 返回值
*(`Observable`)*: An observable sequence whose observers trigger an invocation of the given observable factory function.

#### 例

{% if book.isPdf %}

##### [Using an observable sequence](http://jsbin.com/vigitu/2/edit?js,console)

```js
/* Using an observable sequence */
var source = Rx.Observable.defer(() => Rx.Observable.return(42));

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: 42
// => onCompleted
```

##### [Using a promise](http://jsbin.com/memuf/2/edit?js,console)

```js
/* Using a promise */
var source = Rx.Observable.defer(() => RSVP.Promise.resolve(42));

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: 42
// => onCompleted
```

{% else %}

##### Using an observable sequence

[](http://jsbin.com/vigitu/2/embed?js,console)

##### Using a promise

[](http://jsbin.com/memuf/2/embed?js,console)

{% endif %}


