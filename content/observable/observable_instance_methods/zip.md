## [`Rx.Observable.prototype.zip(...args, [resultSelector])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/zipproto.js)

{% if book.isPdf %}

![zip](http://reactivex.io/documentation/operators/images/zip.png)

{% else %}

<rx-marbles key="zip"></rx-marbles>

{% endif %}

Merges the specified observable sequences or Promises into one observable sequence by using the selector function whenever all of the observable sequences or an array have produced an element at a corresponding index.

The last element in the arguments must be a function to invoke for each series of elements at corresponding indexes in the sources.

#### Arguments
1. `args` *(`Arguments` | `Array`)*: Arguments or an array of observable sequences.
2. `[resultSelector]` *(`Any`)*: Function to invoke for each series of elements at corresponding indexes in the sources, used only if the first parameter is not an array.

#### Returns
*(`Observable`)*: An observable sequence containing the result of combining elements of the sources using the specified result selector function. 

#### Example

##### Using arguments

[](http://jsbin.com/pijaho/1/embed?js,console)

##### Using an array

[](http://jsbin.com/wazuha/1/embed?js,console)