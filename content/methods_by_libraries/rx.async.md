# RxJS Async Module #

The Reactive Extensions for JavaScript provides support for bridging to events, promises, callbacks, Node.js-style callbacks and more.  This module includes all of that functionality.  In addition, this also supports taking ordinary functions and turning them into asynchronous functions via Observable sequences.

This comes with both `rx.async.js` which is for use in modern development environments such as > IE9 and server-side environments such as Node.js.  We also have `rx.async.compat.js` which has backwards compatibility to browsers which do not support all required ES5 features.

## Details ##

Files:
- [`rx.async.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.async.js) | [`rx.lite.compat.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.async.compat.js)
- [`rx.lite.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.lite.js) | [`rx.lite.compat.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.lite.compat.js)

NPM Packages:
- [`rx`](https://www.npmjs.org/package/rx)

NuGet Packages:
- [`RxJS-Async`](http://www.nuget.org/packages/RxJS-Async/)
- [`RxJS-Lite`](http://www.nuget.org/packages/RxJS-Lite/)

File Dependencies:
- [`rx.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.js) | [`rx.compat.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.compat.js)
- [`rx.binding.js`](https://github.com/Reactive-Extensions/RxJS/blob/master/dist/rx.binding.js)

NuGet Dependencies:
- [`RxJS-Main`](http://www.nuget.org/packages/RxJS-Main/)
- [`RxJS-Binding`](http://www.nuget.org/packages/RxJS-Binding/)

## Included Observable Operators ##

### `Observable Methods`
- [`fromCallback`](../observable/observable_methods/fromcallback.html)
- [`fromEvent`](../observable/observable_methods/fromevent.html)
- [`fromEventPattern`](../observable/observable_methods/fromeventpattern.html)
- [`fromNodeCallback`](../observable/observable_methods/fromnodecallback.html)
- [`fromPromise`](../observable/observable_methods/frompromise.html)
- [`start`](../observable/observable_methods/start.html)
- [`startAsync`](../observable/observable_methods/startasync.html)
- [`toAsync`](../observable/observable_methods/toasync.html)
