## RxJS 设计指南

<img style="display: block; margin: 0 auto; clear: right;"
  src="https://raw.githubusercontent.com/Reactive-Extensions/RxJS/master/doc/designguidelines/images/984368.png"
  alt="RxJS Logo">

* [简介](introduction.md)
* [什么时候该用RxJS](when.md)
  * [使用RxJS来编排异步和基于事件的计算](when.md#use-rxjs-for-orchestrating-asynchronous-and-event-based-computations)
  * [使用RxJS处理异步数据队列](when.md#use-rxjs-to-deal-with-asynchronous-sequences-of-data)
* [RxJS规范](contract.md)
  * [RxJS语法](contract.md#assume-the-rxjs-grammar)
  * [假定在一个`onError`或`onCompleted`消息之后资源将被回收](contract.md#assume-resources-are-cleaned-up-after-an-onerror-or-oncompleted-message)
  * [当取消订阅时将会智能地消毁所有未执行的程序](contract.md#assume-a-best-effort-to-stop-all-outstanding-work-on-unsubscribe)
* [使用 Rx 的技巧](using.md)
* [实践操作](implementations.md)

<!-- 1. Introduction
2. When to use RxJS
  1. Use RxJS for orchestrating asynchronous and event-based computations
  2. Use RxJS to deal with asynchronous sequences of data
3. The RxJS contract
  1. Assume the RxJS Grammar
  2. Assume resources are cleaned up after an `onError` or `onCompleted` messages
  3. Assume a best effort to stop all outstanding work on Unsubscribe
4. Using RxJS
  1. Consider drawing a Marble-diagram
  2. Consider passing multiple arguments to `subscribe`
  3. Consider passing a specific scheduler to concurrency introducing operators
  4. Call the `observeOn` operator as late and in as few
  5. Consider limiting buffers
  6. Make side-effects explicit using the `do`/`tap` operator
  7. Assume messages can come through until unsubscribe has completed
  8. Use the Publish operator to share side-effects
5. Operator implementations
  1. Implement new operators by composing existing operators
  2. Implement custom operators using `Observable.create`
  3. Protect calls to user code from within an operator
  4. `subscribe` implementations should not throw
  5. `onError` messages should have abort semantics
  6. Parameterize concurrency by providing a scheduler argument
  7. Provide a default scheduler
  8. The scheduler should be the last argument to the operator
  9. Avoid introducing concurrency
  10. Hand out all disposables instances created inside the operator to consumers
  11. Operators should not block
  12. Avoid deep stacks caused by recursion in operators
  13. Argument validation should occur outside `Observable.create`
  14. Unsubscription should be idempotent
  15. Unsubscription should not throw
  16. Custom Observable implementations should follow the Rx contract
  17. Operator implementations should follow guidelines for Rx usage -->
