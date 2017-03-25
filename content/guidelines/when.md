# 何时该使用RxJS

### 使用RxJS来编排异步和基于事件的计算
处理多个事件或异步计算的代码很快就会变得复杂，因为它需要建立一个状态机来处理排序问题。接下来，代码需要处理每个事件的成功和失败状态。这导致代码不遵循正常的控制流程，很难理解并难以维护。

RxJS使这些计算成为一等公民。 这提供了可读和可组合的API处理这些异步计算的模型。

#### 栗子 ####
```js
var input = document.getElementById('input');
var dictionarySuggest = Rx.Observable.fromEvent(input, 'keyup')
  .map(() => input.value)
  .filter(text => !!text)
  .distinctUntilChanged()
  .throttle(250)
  .flatMapLatest(searchWikipedia)
  .subscribe(
    results => {
      list = [];
      list.concat(results.map(createItem));
    },
    err => logError(err)
  );
```

此示例模拟在用户键入时，接收输入建议数据(autocomplete)的常见UI范例。

RxJS创建一个可观察序列，对输入的现有keyup事件建模。

然后它在事件后放置了几个过滤器和映射，让事件只有在发生了值的变化时才触发。 （每次按键都会触发 keyup 事件，包括用户按左右箭头移动光标，但这时输入文本并不会变化）。

接下来通过使用 [`throttle`](../../observable/observable_instance_methods/throttle.md) 操作符确保事件只在250毫秒的没有输入时才被触发。 （如果用户仍在键入字符，会被立即延迟请求，从而减少一次潜在的昂贵的请求）。

在以前编写的程序中，这种节流将通过定时器引入单独的回调。 这个计时器可能会抛出异常（某些定时器在运行中可能有大量的操作）。

一旦用户输入被过滤掉，就是执行查找的时间。 由于这通常是耗时的操作（例如对在世界的另一侧的服务器的请求），所以该操作本身也是异步的。

[`flatMap`/`selectMany`](../../observable/observable_instance_methods/flatmap.md) 操作符可以方便地组合多个异步操作。它不仅可以合并请求成功后返回的值；它还可以跟踪在每个单独操作中发生的任何异常。

在传统的编程中，这将引入单独的回调和异常捕获。

如果用户在查询操作仍在进行时键入了新字符，我们不再需要看到该查询的结果。 因为用户已经键入更具体的词，看到旧的结果将会非常困惑。

`flatMapLatest`操作确保一旦检测到新的`keyup`，就忽略查询操作。

最后，我们订阅所得的可观察序列。 只有在这个时候我们的回调函数将被调用。 我们传递两个函数到`subscribe` 调用：
1. 从我们的计算接收结果。
2. 在执行的任何地方发生故障的情况下捕获异常。

#### 何时忽略此指南 ####

如果所涉及的应用程序/类库具有非常少的基于异步/基于事件的操作，或者具有很少的需要组合这些操作的地方，使用RxJS（重新分配库以及学习曲线）的成本可能超过手动编码的成本 。

### 使用RxJS来处理异步数据序列

还有一些其他库用于简化JavaScript生态系统中的异步操作。 即使这些库是强大的，它们通常在返回单个结果的操作上工作得非常好。 但它们通常不支持在运行的生命周期中产生多个结果的操作。

RxJS遵循以下语法：`onNext`* (`onCompleted`|`onError`)?。 这允许随着时间推移返回多个值。 这使得RxJS适合产生单个或多个结果的操作。

```js
var fs = require('fs');
var Rx = require('rx');

// Read/write from stream implementation
function readAsync(fd, chunkSize) { /* impl */ }
function appendAsync(fd, buffer) { /* impl */ }
function encrypt(buffer) { /* impl */}

//open a 4GB file for asynchronous reading in blocks of 64K
var inFile = fs.openSync('4GBfile.txt', 'r+');
var outFile = fs.openSync('Encrypted.txt', 'w+');

readAsync(inFile, 2 << 15)
  .map(encrypt)
  .flatMap(data => appendAsync(outFile, data))
  .subscribe(
    () => {},
    err => {
      console.log('An error occurred while encrypting the file: %s', err.message);
      fs.closeSync(inFile);
      fs.closeSync(outFile);
    },
    () => {
      console.log('Successfully encrypted the file.');
      fs.closeSync(inFile);
      fs.closeSync(outFile);
    }
  );
```

在此示例中，一个4 GB的文件被完整读取，加密并保存到另一个文件。

将整个文件读入内存，加密它并写出整个文件将是一个耗时的操作。

相反，事实上，我们依赖于RxJS可以分段返回部分结果。

我们以64K的块异步读取文件。这产生可订阅的字节数组队列。然后我们分别加密每个块（对于这个示例，我们假设加密操作可以在文件的单独部分上操作）。 一旦块被加密，它立即被沿着管道进一步向下发送以保存到另一个文件。 `writeAsync`操作是一个可以处理多个消息的异步操作。

#### 何时忽略此指南 ####

如果所涉及的应用程序/库对于多个消息具有非常少的操作，则使用RxJS（重新分配库以及学习曲线）的成本可能超过手动编码这些操作的成本。
