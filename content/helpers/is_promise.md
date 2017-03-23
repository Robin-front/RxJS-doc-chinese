## [`Rx.helpers.isPromise(p)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/headers/basicheader.js#L12)

A function which determines whether the object is a `Promise`.

#### Arguments
1. `p` *(Any)*: The object to determine whether it is a promise.

#### Returns
*(Boolean)*: `true` if the object is a `Promise` else `false`

#### Example 

```js
var isPromise = Rx.helpers.isPromise;

var p = RSVP.Promise(res => res(42));

console.log(isPromise(p));
// => true
```