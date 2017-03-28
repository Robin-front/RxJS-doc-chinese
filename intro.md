# 前言

网络上现在翻译资料和专题讲解比较零散，所以打算整理现有翻译资源，并尝试自己翻译，也希望有人参与，水平有限，错误之处请多多指点。
引用翻译都会在里面注明原翻译作者和发布链接。

> **本翻译 _未经授权_ ！随时可能删除，请勿作它用！仅供交流学习。** 翻译自[rc-book](https://github.com/xgrommx/rx-book), 也有人提issue,但作者迟迟没有动作.如有侵权，我会立即删除。


# RxJS <sup>v4.0</sup>

Reactive Extensions (Rx) 是一个库，用于使用可观察序列和语言集成查询来组成的异步和基于事件的程序。

数据序列可以采取多种形式，例如来自文件或Web服务的数据流，Web服务请求，系统通知或用户输入的一系列事件。

Reactive Extensions 称所有这些数据序列为可观察序列。应用程序可以订阅这些可观察的序列，以便在新数据到达时接收异步通知。

RxJS没有依赖性，它与同步数据流（例如JavaScript中的可迭代对象)以及单值异步计算（如Promises）取长补短，如下图所示：

<center>
<table style="display: table">
    <thead>
        <tr>
            <th></th>
            <th>单个返回值</th>
            <th>多个返回值</th>
        </tr>
    </thead>
    <tbody>
        <tr>
          <td>Pull/Synchronous/Interactive</td>
          <td>Object</td>
          <td>Iterables (Array | Set | Map | Object)</td>
        </tr>
        <tr>
          <td>Push/Asynchronous/Reactive</td>
          <td>Promise</td>
          <td>Observable</td>
        </tr>
    </tbody>
</table>
</center>

更具体地说，如果你知道如何使用Array＃extras操作数组，那么你已经知道如何使用RxJS！

<center>
<h3>示例代码展示了如何将类似的高阶函数应用于数组和可观察的值</h3>

<table style="display: table">
    <thead>
        <tr>
            <th style="text-align:center;" colspan="2">可迭代</th>
            <th style="text-align:center;" colspan="2">可观察</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td colspan="2">
                <pre>
<code>
getDataFromLocalMemory()
    .filter (s => s != null)
    .map(s => `${s} transformed`)
    .forEach(s => console.log(`next => ${s}`))
</code>
                </pre>
            </td>
            <td colspan="2">
                <pre>
<code>
getDataFromNetwork()
    .filter (s => s != null)
    .map(s => `${s} transformed`)
    .subscribe(s => console.log(`next => ${s}`))
</code>
                </pre>
            </td>
        </tr>
    </tbody>
</table>
</center>
