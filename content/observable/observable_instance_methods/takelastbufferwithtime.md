## [`Rx.Observable.prototype.takeLastBufferWithTime(duration, [scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/takelastbufferwithtime.js)

{% if book.isPdf %}

![takeLastBufferWithTime](http://reactivex.io/documentation/operators/images/takeLastBufferWithTime.png)

{% else %}



{% endif %}

Returns an array with the elements within the specified duration from the end of the observable source sequence, using the specified scheduler to run timers.

This operator accumulates a queue with a length enough to store elements received during the initial duration window. As more elements are received, elements older than the specified duration are taken from the queue and produced on the result sequence. This causes elements to be delayed with duration.  
 
#### Arguments
1. `duration` *(`Number`)*: Duration for taking elements from the end of the sequence.
2. `[scheduler=Rx.Scheduler.timeout]` *(`Scheduler`)*: Scheduler to run the timer on. If not specified, defaults to timeout scheduler.

#### Returns
*(`Observable`)*: An observable sequence containing a single array with the elements taken during the specified duration from the end of the source sequence.
 
#### Example

[](http://jsbin.com/komepa/1/embed?js,console)
