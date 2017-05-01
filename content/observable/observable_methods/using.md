## [`Rx.Observable.using(resourceFactory, observableFactory)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/using.js)

![using](http://reactivex.io/documentation/operators/images/using.png)

构造一个可观察的序列，该序列依赖于一个资源对象，其生命周期与所得到的可观察序列的寿命有关。

#### 参数
1. `resourceFactory` *(`Function`)*: 获取资源对象的工厂函数.
2. `observableFactory` *(`Scheduler`)*: 工厂的功能，以获得一个可观察到的序列，取决于所获得的资源。

#### 返回值
*(`Function`)*: 可观察序列，其生命周期由资源对象的寿命。

#### 例

```js
/* Using an AsyncSubject as a resource which supports the .dispose method */
function DisposableResource(value) {
    this.value = value;
    this.disposed = false;
}

DisposableResource.prototype.getValue = function () {
    if (this.disposed) {
        throw new Error('Object is disposed');
    }
    return this.value;
};

DisposableResource.prototype.dispose = function () {
    if (!this.disposed) {
        this.disposed = true;
        this.value = null;
    }
    console.log('Disposed');
};

var source = Rx.Observable.using(
    function () { return new DisposableResource(42); },
    function (resource) {
        var subject = new Rx.AsyncSubject();
        subject.onNext(resource.getValue());
        subject.onCompleted();
        return subject;
    }
);

var subscription = source.subscribe(
    function (x) {
        console.log('Next: ' + x);
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

// => Next: 42
// => Completed

subscription.dispose();

// => Disposed
```
[](http://jsbin.com/yewaf/1/embed?js,console)
