# 操作流的技巧

### 通过组合现有的流产生新的流 ###

许多操作可以从已存在的操作符进行组合。这会让我们的代码更少更简单。 Rx 团队在所有基本的操作符上下了很多工夫。通过复用这些操作符，你可以自由地组合你自己个性化的操作符。

#### Sample ####

```js
Rx.Observable.prototype.flatMap = function(selector) { return this.map(selector).mergeAll(); }
```

在这个例子中， `flatMap`操作符使用了两个已存在的操作符： [`map`](../../observable/observable_instance_methods/map.html) and [`mergeAll`](../../observable/observable_instance_methods/mergeall.html)。  [`map`](../../observable/observable_instance_methods/map.html) 操作符已经处理围绕选择器函数抛出异常的任何问题。 [`mergeAll`](../../observable/observable_instance_methods/mergeall.html) 操作符已经处理多个可观察序列的并发问题。

#### 何时忽略这条指南 ####

- 没有适当的基础运算符集可用于实现此运算符.
- 性能分析证明，使用现有操作符的实现具有性能问题。比如[`materialize`](../../observable/observable_instance_methods/materialize.html).

### 使用 [`Observable.create`](../../observable/observable_methods/create.html)创建新的流 ###

当无法遵循准则5.1时， 使用 `Observable.Create(WithDisposable)` 方法创建一个新的可观察对象，因为它提供了几个保护，使可观察的序列遵循RxJS约定。

- 当可观察序列已经完成时（不管是触发了[`onError`](../..observer/observer_instance_methods/onerror.html) 还是 [`onCompleted`](../../observer/observer_instance_methods/oncompleted.html))）， 任何订阅者都会自动取消订阅。
- 任何订阅者的实例都只能接收到一条错误（OnError）或完成（OnCompleted）消息。之后可观察序列不会再发送消息。 No more messages are sent through. This ensures the Rx grammar of onNext* (onError|onCompleted)?

#### Sample ####

```js
Rx.Observable.prototype.map = function(selector, thisArg) {
  var source = this;
  return Rx.Observable.create(observer => {
    var idx = 0;
    return source.subscribe(
      x => {
        var result;
        try {
          result = selector.call(thisArg, x, idx++, source);
        } catch (e) {
          observer.onError(e);
          return;
        }

        observer.onNext(result);
      },
      observer.onError.bind(observer),
      observer.onCompleted.bind(observer)
    );
  })
};
```

In this sample, `map` uses the `Observable.create` operator to return a new instance of the Observable class. This ensures that no matter the implementation of the source observable sequence, the output observable sequence follows the Rx contract . It also ensures that the lifetime of subscriptions is a short as possible.

#### 何时忽略这条指南 ####

- The operator needs to return an observable sequence that doesn’t follow the Rx contract. This should usually be avoided (except when writing tests to see how code behaves when the contract is broken)
- The object returned needs to implement more than the Observable class (e.g. Subject, or a custom class).

### Protect calls to user code from within an operator ###

When user code is called from within an operator, this is potentially happening outside of the execution context of the call to the operator (asynchronously). Any exception that happens here will cause the program to terminate unexpectedly. Instead it should be fed through to the subscribed observer instance so that the exception can be dealt with by the subscribers.

Common kinds of user code that should be protected:
- Selector functions passed in to the operator.
- Comparer functions passed into the operator.

**Note:** calls to `Scheduler` implementations are not considered for this guideline. The reason for this is that only a small set of issues would be caught as most schedulers deal with asynchronous calls. Instead, protect the arguments passed to schedulers inside each scheduler implementation.

#### Sample ####

```js
Rx.Observable.prototype.map = function(selector, thisArg) {
  var source = this;
  return Rx.Observable.create(observer => {
    var idx = 0;
    return source.subscribe(
      x => {
        var result;
        try {
          result = selector.call(thisArg, x, idx++, source);
        } catch (e) {
          observer.onError(e);
          return;
        }

        observer.onNext(result);
      },
      observer.onError.bind(observer),
      observer.onCompleted.bind(observer)
    );
  })
};
```

这个例子调用一个用户传进来的 selector 函数。它在调用的时候捕获了所有异常结果，并通过调用 `onError` 传给了观察者实例。

#### 何时忽略这条指南 ####

调用创建可观察序列之前的用户代码（`Observable.create`之外调用）时忽略这条指南。这些调用位于当前执行上下文中，并遵循正常控制流.

**注意:** 谨慎调用 `subscribe`, `dispose`, `onNext`, `onError` 和 `onCompleted` 这些方法。These calls are on the edge of the monad. 在这些地方调用 `onError` 方法会导致异常发生。

### `subscribe` 的实现不应该抛出异常 ###

当多个可观察序列合并时， 订阅一个特殊的可观察序列可能不会发生在用户调用 `subscribe`的时候（比如：在`concat` 操作符内，第一个可观察序列完成后，第二个可观察序列参数只能被订阅一次）。抛出异常会终止程序。相反，订阅发生的异常应该被传到`onError`方法。

#### Sample ####

```js
var CLOSED = 3;

function readWebSocket(socket) {
  return Rx.Observable.create(observer => {
    if (socket.readyState === CLOSED) {
      observer.onError(new Error('The websocket is no longer open.'));
      return;
    }
    // 这里放具体实现代码
  });
}
```

In this sample, an error condition is detected in the subscribe method implementation. An error is raised by calling the `onError` method instead of throwing the exception. This allows for proper handling of the exception if `subscribe` is called outside of the execution context of the original call to Subscribe by the user.

#### 何时忽略这条指南 ####

When a catastrophic error occurs that should bring down the whole program anyway.

### `onError` messages should have abort semantics

As normal control flow in JavaScript uses abort semantics for exceptions (the stack is unwound, current code path is interrupted), RxJS mimics this behavior. To ensure this behavior, no messages should be sent out by an operator once one of it sources has an error message or an exception is thrown within the operator.

#### Sample ####

```js
Rx.Observable.prototype.minimumBuffer = function(bufferSize) {
  var source = this;
  return Rx.Observable.create(observer => {
    var data = [];

    return source.subscribe(
      value => {
        data = data.concat(value);
        if (data.length > bufferSize) {
          observer.onNext(data.slice(0));
          data = [];
        }
      },
      observer.onError.bind(observer),
      () => {
        if (data.length > 0) {
          observer.onNext(data.slice(0));
        }
        observer.onCompleted();
      }
    );
  });
};
```

In this sample, a buffering operator will abandon the observable sequence as soon as the subscription to source encounters an error. The current buffer is not sent to any subscribers, maintain abort semantics.

#### 何时忽略这条指南 ####

There are currently no known cases where to ignore this guideline.

### Parameterize concurrency by providing a scheduler argument ###

As there are many different notions of concurrency, and no scenario fits all, it is best to parameterize the concurrency an operator introduces. The notion of parameterizing concurrency in RxJS is abstracted through the `Scheduler` class.

#### Sample ####

```js
Rx.Observable.just = (value, scheduler) => {
  return Rx.Observable.create(observer => {
    return scheduler.schedule(() => {
      observer.onNext(value);
      observer.onCompleted();
    });
  });
};
```

In this sample, the `just` operator parameterizes the level of concurrency the operator has by providing a scheduler argument. It then uses this scheduler to schedule the firing of the `onNext` and `onCompleted` messages.

#### 何时忽略这条指南 ####

- The operator is not in control of creating the concurrency (e.g. in an operator that converts an event into an observable sequence, the source event is in control of firing the messages, not the operator).
- The operator is in control, but needs to use a specific scheduler for introducing concurrency.

### Provide a default scheduler ###

In most cases there is a good default that can be chosen for an operator that has parameterized concurrency through guideline 5.6. This will make the code that uses this operator more succinct.

**Note:** Follow guideline 5.9 when choosing the default scheduler, using the immediate scheduler where possible, only choosing a scheduler with more concurrency when needed.

#### Sample ####

```js
Rx.Observable.just = (value, scheduler) => {
  // Pick a default scheduler, in this case, immediately
  Rx.helpers.isScheduler(scheduler) || (scheduler = Rx.Scheduler.immediate);

  return Rx.Observable.create(observer => {
    return scheduler.schedule(() => {
      observer.onNext(value);
      observer.onCompleted();
    });
  });
};
```

In this sample, we provided a default scheduler if not provided by the caller.

#### 何时忽略这条指南 ####

Ignore this guideline when no good default can be chosen.

### The scheduler should be the last argument to the operator ###

Adding the scheduler as the last argument is a must for all operators introducing concurrency.  This is to ensure that the schedulers are optional, and a default one can be chosen.  This also makes the programming experience much more predictable.

#### Sample ####

```js
Rx.Observable.just = (value, scheduler) => {
  // Pick a default scheduler, in this case, immediately
  Rx.helpers.isScheduler(scheduler) || (scheduler = Rx.Scheduler.immediate);

  return Rx.Observable.create(observer => {
    return scheduler.schedule(() => {
      observer.onNext(value);
      observer.onCompleted();
    });
  });
};
```

In this sample the `return` operator has two parameters, and the scheduler parameter defaults to the immediate scheduler if not provided. As the scheduler argument is the last argument, adding or omitting the argument is clearly visible.

#### 何时忽略这条指南 ####

JavaScript supports rest arguments syntax. With this syntax, the rest arguments has to be the last argument. Make the scheduler the final to last argument in this case.

### Avoid introducing concurrency ###

By adding concurrency, we change the timeliness of an observable sequence. Messages will be scheduled to arrive later. The time it takes to deliver a message is data itself, by adding concurrency we skew that data.  This includes not using such mechanisms as `setTimeout`, `setImmediate`, `requestAnimationFrame`, `process.nextTick`, etc which should be avoided directly in your code, and instead be wrapped by a `Scheduler` class.

#### Sample 1 ####

```js
Rx.Observable.prototype.map = function(selector, thisArg) {
  var source = this;
  return Rx.Observable.create(observer => {
    var idx = 0;
    return source.subscribe(
      x => {
        var result;
        try {
          result = selector.call(thisArg, x, idx++, source);
        } catch (e) {
          observer.onError(e);
          return;
        }

        observer.onNext(result);
      },
      observer.onError.bind(observer),
      observer.onCompleted.bind(observer)
    );
  })
};
```

In this sample, the select operator does not use a scheduler to send out the `onNext` message. Instead it uses the source observable sequence call to `onNext` to process the message, hence staying in the same time-window.

#### Sample 2 ####

```js
Rx.Observable.just = (value, scheduler) => {
  // Pick a default scheduler, in this case, immediately
  Rx.helpers.isScheduler(scheduler) || (scheduler = Rx.Scheduler.immediate);

  return Rx.Observable.create(observer => {
    return scheduler.schedule(() => {
      observer.onNext(value);
      observer.onCompleted();
    });
  });
};
```

In this case, the default scheduler for the `just` operator is the immediate scheduler. This scheduler does not introduce concurrency.

#### 何时忽略这条指南 ####

Ignore this guideline in situations where introduction of concurrency is an essential part of what the operator does.

**NOTE:** When we use the Immediate scheduler or call the observer directly from within the call to `subscribe`, we make the `subscribe` call blocking. Any expensive computation in this situation would indicate a candidate for introducing concurrency.

### Hand out all disposables instances created inside the operator to consumers ###

`Disposable` instances control lifetime of subscriptions as well as cancelation of scheduled actions. RxJS gives users an opportunity to unsubscribe from a subscription to the observable sequence using disposable instances.

After a subscription has ended, no more messages are allowed through. At this point, leaving any state alive inside the observable sequence is inefficient and can lead to unexpected semantics.

To aid composition of multiple disposable instances, RxJS provides a set of classes implementing `Disposable` such as:

Name                       | Description
-------------------------- | ---------------------------------------------------------------
CompositeDisposable        | Composes and disposes a group of disposable instances together.
SerialDisposable           | A place holder for changing instances of disposable instances. Once new disposable instance is placed, the old disposable instance is disposed.
SingleAssignmentDisposable | A place holder for a single instance of a disposable.
ScheduledDisposable        | Uses a scheduler to dispose an underlying disposable instance.

#### Sample ####

```js
Observable.prototype.zip = function() {
  var parent = this,
      sources = slice.call(arguments),
      resultSelector = sources.pop();

  sources.unshift(parent);
  return new AnonymousObservable(observer => {
    var n = sources.length,
      queues = arrayInitialize(n, () => []),
      isDone = arrayInitialize(n, () => false);

    function next(i) {
      var res, queuedValues;
      if (queues.every(x => x.length > 0)) {
        try {
          queuedValues = queues.map(x => x.shift());
          res = resultSelector.apply(parent, queuedValues);
        } catch (ex) {
          observer.onError(ex);
          return;
        }
        observer.onNext(res);
      } else if (isDone.filter((x, j) => j !== i).every(identity)) {
        observer.onCompleted();
      }
    };

    function done(i) {
      isDone[i] = true;
      if (isDone.every(x => x)) {
        observer.onCompleted();
      }
    }

    var subscriptions = new Array(n);
    for (var idx = 0; idx < n; idx++) {
      (i => {
        var source = sources[i], sad = new SingleAssignmentDisposable();
        Rx.helpers.isPromise(source) && (source = Rx.Observable.fromPromise(source));
        sad.setDisposable(source.subscribe(x => {
          queues[i].push(x);
          next(i);
        }, observer.onError.bind(observer), () => {
          done(i);
        }));
        subscriptions[i] = sad;
      })(idx);
    }

    return new CompositeDisposable(subscriptions);
  });
};
```

In this sample, the operator groups all disposable instances controlling the various subscriptions together and returns the group as the result of subscribing to the outer observable sequence. When a user of this operator subscribes to the resulting observable sequence, he/she will get back a disposable instance that controls subscription to all underlying observable sequences.

#### 何时忽略这条指南 ####

There are currently no known instances where this guideline should be ignored.

### Operators should not block ###

RxJS is a library for composing asynchronous and event-based programs using observable collections.

By making an operator blocking we lose these asynchronous characteristics. We also potentially lose composability (e.g. by returning a value typed as `T` instead of `Observable<T>`).  This is in contrast to the Array#extras such as `sum`, `reduce`, `some` and `every` which return a single value.

#### Sample ####

```js
Rx.Observable.prototype.sum = function() { return this.reduce((acc, x) => acc + x, 0); }
```

In this sample, the `sum` operator has a return type of `Observable<Number>` instead of `Number`. By doing this, the operator does not block. It also allows the result value to be used in further composition.

#### 何时忽略这条指南 ####

There are currently no known instances where this guideline should be ignored.

### Avoid deep stacks caused by recursion in operators ###

As code inside Rx operators can be called from different execution context in many different scenarios, it is nearly impossible to establish how deep the stack is before the code is called. If the operator itself has a deep stack (e.g. because of recursion), the operator could trigger a stack overflow quicker than one might expect.

There are two recommended ways to avoid this issue:

- Use the recursive `scheduleRecursive` methods on the `Scheduler`
- Implement an infinite looping generator using the yield iterator pattern, convert it to an observable sequence using the `from` operator.

#### Sample 1 ####

```js
Rx.Observable.repeat = (value, scheduler) => {
  return Rx.Observable.create(observer => {
    return scheduler.scheduleRecursive(self => {
      observer.onNext(value);
      self();
    });
  });
};
```

In this sample, the recursive `scheduleRecursive` method is used to allow the scheduler to schedule the next iteration of the recursive function. Schedulers such as the current thread scheduler do not rely on stack semantics. Using such a scheduler with this pattern will avoid stack overflow issues.

#### Sample 2 ####

```js
Rx.Observable.repeat = value => {
  return Rx.Observable.from(
    function* () {
      while(true) { yield value; }
    }());
};
```

The yield iterator pattern ensures that the stack depth does not increase drastically. By returning an infinite generator with the `from` operator can build an infinite observable sequence.

#### 何时忽略这条指南 ####

There are currently no known instances where this guideline should be ignored.

### Argument validation should occur outside `Observable.create`

As guideline 5.3 specifies that the `Observable.create` operator should not throw, any argument validation that potentially throws exceptions should be done outside the `Observable.create` operator.

#### Sample ####

```js
Rx.Observable.prototype.map = function(selector, thisArg) {
  if (this == null) {
    throw new TypeError('Must be an instance of an Observable');
  }
  if (selector == null) {
    throw new TypeError('selector cannot be null/undefined');
  }
  var selectorFn = typeof selector !== 'function' ? () => selector : selector;
  var source = this;
  return Rx.Observable.create(observer => {
    var idx = 0;
    return source.subscribe(
      x => {
        var result;
        try {
          result = selectorFn.call(thisArg, x, idx++, source);
        } catch (e) {
          observer.onError(e);
          return;
        }

        observer.onNext(result);
      },
      observer.onError.bind(observer),
      observer.onCompleted.bind(observer)
    );
  };
};
```

In this sample, the arguments are checked for null values before the `Observable.create` operator is called.

#### 何时忽略这条指南 ####

Ignore this guideline if some aspect of the argument cannot be checked until the subscription is active.

### Unsubscription should be idempotent ###

The observable `subscribe` method returns a `Disposable` instance that can be used to clean up the subscription. The `Disposable` instance doesn’t give any information about what the state of the subscription is. As consumers do not know the state of the subscription, calling the `dispose` method multiple times should be allowed. Only the first call the side-effect of cleaning up the subscription should occur.

#### Sample ####

```js
var subscription = xs.subscribe(console.log.bind(console));
subscription.dispose();
subscription.dispose();
```

In this sample, the subscription is disposed twice, the first time the subscription will be cleaned up and the second call will be a no-op.

### Unsubscription should not throw ###

As the RxJS’s composition makes that subscriptions are chained, so are unsubscriptions. Because of this, any operator can call an unsubscription at any time. Because of this, just throwing an exception will lead to the application crashing unexpectedly. As the observer instance is already unsubscribed, it cannot be used for receiving the exception either. Because of this, exceptions in unsubscriptions should be avoided.

#### 何时忽略这条指南 ####

There are currently no known cases where to ignore this guideline.

### Custom `Observable` implementations should follow the RxJS contract ###

When it is not possible to follow guideline 5.1, the custom implementation of the `Observable` class should still follow the RxJS contract in order to get the right behavior from the RxJS operators.

#### 何时忽略这条指南 ####

Only ignore this guideline when writing observable sequences that need to break the contract on purpose (e.g. for testing).

### Operator implementations should follow guidelines for RxJS usage ###

As Rx is a composable API, operator implementations often use other operators for their implementation (see paragraph 5.1). RxJS usage guidelines should be strongly considered when implementing these operators.

#### 何时忽略这条指南 ####

As described in the introduction, only follow a guideline if it makes sense in that specific situation.
