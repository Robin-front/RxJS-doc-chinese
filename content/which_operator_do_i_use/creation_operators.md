# 我该选用哪个操作符? - 创建操作符 #

使用此页面查找[`Observable`](../observable/observable_methods/index.html)符合您需求的类型创建操作：

<table style="display: table">
<thead>静态方法</thead>
<tbody>
    <!-- Observable factories -->
    <tr>
        <td rowspan="26">我想创建一个新的序列</td>
        <td rowspan="4">使用自定义逻辑</td>
        <td colspan="2"></td>
        <td>
            <a href="../observable/observable_methods/create.html">Observable.create</a>
        </td>
    </tr>
    <tr>
        <td rowspan="3">像一个for循环</td>
        <td></td>
        <td><a href="../observable/observable_methods/generate.html">Observable.generate</a></td>
    </tr>
    <tr>
        <td rowspan="2">并随时间发射值</td>
        <td><a href="../observable/observable_methods/generatewithrelativetime.html">Observable.generateWithRelativeTime</a></td>
    </tr>
    <tr>
        <td><a href="../observable/observable_methods/generatewithabsolutetime.html">Observable.generateWithAbsoluteTime</a></td>
    </tr>
    <tr>
        <td rowspan="2">它返回一个值</td>
        <td colspan="2"></td>
        <td>
            <a href="../observable/observable_methods/return.html">Observable.return/just</a>
        </td>
    </tr>
    <tr>
        <td colspan="2">多次</td>
        <td><a href="../observable/observable_methods/repeat.html">Observable.repeat</a></td>
    </tr>
    <tr>
        <td colspan="3">这会抛出错误</td>
        <td><a href="../observable/observable_methods/throw.html">Observable.throw</a></td>
    </tr>
    <tr>
        <td colspan="3">完成了</td>
        <td><a href="../observable/observable_methods/empty.html">Observable.empty</a></td>
    </tr>
    <tr>
        <td colspan="3">从来没有做任何事情</td>
        <td><a href="../observable/observable_methods/never.html">Observable.never</a></td>
    </tr>
    <tr>
        <td rowspan="2">从事件</td>
        <td colspan="2"></td>
        <td><a href="../observable/observable_methods/fromevent.html">Observable.fromEvent</a></td>
    </tr>
    <tr>
        <td colspan="2">它使用自定义函数来添加和删除事件处理程序</td>
        <td><a href="../observable/observable_methods/fromeventpattern.html">Observable.fromEventPattern</a></td>
    </tr>
    <tr>
        <td colspan="3">来自一个<a title="ES6 Promise" href="https://www.promisejs.org">ES6 Promise</a></td>
        <td><a href="../observable/observable_methods/frompromise.html">Observable.fromPromise</a></td>
    </tr>
    <tr>
        <td rowspan="6">它可迭代</td>
        <td rowspan="2">覆盖到数组中的值</td>
        <td></td>
        <td>
            <a href="../observable/observable_methods/fromarray.html">Observable.fromArray</a><br>
        </td>
    </tr>
    <tr>
      <td>对象键/值对</td>
      <td><a href="../observable/observable_methods/pairs.html">Observable.pairs</a></td>
    </tr>
    <tr>
        <td colspan="2">异步元素</td>
        <td><a href="../observable/observable_methods/for.html">Observable.for</a></td>
    </tr>
    <tr>
        <td colspan="2">数值范围内的值</td>
        <td><a href="../observable/observable_methods/range.html">Observable.range</a></td>
    </tr>
    <tr>
        <td colspan="2">来自一个可迭代的数组或类似数组的对象的值</a></td>
        <td><a href="../observable/observable_methods/from.html">Observable.from</a></td>
    </tr>
    <tr>
        <td colspan="2">来自参数</a></td>
        <td><a href="../observable/observable_methods/of.html">Observable.of</a></td>
    </tr>
    <tr>
        <td rowspan="2">根据定时器发出值</td>
        <td colspan="2"></td>
        <td><a href="../observable/observable_methods/interval.html">Observable.interval</a></td>
    </tr>
    <tr>
        <td colspan="2">具有可选的初始延迟</td>
        <td><a href="../observable/observable_methods/timer.html">Observable.timer</a></td>
    </tr>
    <tr>
        <td rowspan="2" colspan="2">不传参调用函数</td>
        <td>在特定的调度程序</td>
        <td>
            <a href="../observable/observable_methods/start.html">Observable.start</a>
        </td>
    </tr>
    <tr>
        <td>异步</td>
        <td>
            <a href="../observable/observable_methods/startasync.html">Observable.startAsync</a>
        </td>
    </tr>
    <tr>
        <td rowspan="4">取决于订阅时</td>
        <td colspan="2">基于布尔条件</td>
        <td><a href="../observable/observable_methods/if.html">Observable.if</a></td>
    </tr>
    <tr>
        <td colspan="2">f从一组预先设定的序列</td>
        <td><a href="../observable/observable_methods/case.html">Observable.case</a></td>
    </tr>
    <tr>
        <td colspan="1" rowspan="2">使用自定义逻辑</td>
        <td></td>
        <td><a href="../observable/observable_methods/defer.html">Observable.defer</a></td>
    </tr>
    <tr>
        <td>它取决于资源</td>
        <td><a href="../observable/observable_methods/using.html">Observable.using</a></td>
    </tr>
    <!-- Function factories -->
    <tr>
        <td rowspan="3">我想包装一个函数</td>
        <td colspan="2"></td>
        <td rowspan="3">并产生一个序列的结果</td>
        <td><a href="../observable/observable_methods/toasync.html">Observable.toAsync</a></td>
    </tr>
        <td colspan="2">它接受回调</td>
        <td><a href="../observable/observable_methods/fromcallback.html">Observable.fromCallback</a></td>
    </tr>
    <tr>
        <td colspan="2">它接受Node.js回调</td>
        <td><a href="../observable/observable_methods/fromnodecallback.html">Observable.fromNodeCallback</a></td>
    </tr>
    <!-- Flatteners -->
    <tr>
        <td rowspan="30">我想结合多个序列</td>
        <td colspan="3">并且仅从产生值的序列中接收值</td>
        <td><a href="../observable/observable_methods/amb.html">Observable.amb</a></td>
    </tr>
    <tr>
        <td colspan="3">所有人都已经完成通知</td>
        <td><a href="../observable/observable_methods/forkjoin.html">Observable.forkJoin</a></td>
    </tr>
    <tr>
        <td colspan="3">并输出所有这些值</td>
        <td><a href="../observable/observable_methods/merge.html">Observable.merge</a></td>
    </tr>
    <tr>
        <td rowspan="2">为了</td>
        <td colspan="2">不改变时重复使用最新值</td>
        <td><a href="../observable/observable_methods/operators/combinelatest.html">Observable.combineLatest</a></td>
    </tr>
    <tr>
        <td colspan="2">每个值只使用一次</td>
        <td><a href="../observable/observable_methods/zip.html">Observable.zip</a></td>
    </tr>
    <tr>
        <td rowspan="3">通过订阅每个序列为了</td>
        <td colspan="2">当前一个序列完成时</td>
        <td><a href="../observable/observable_methods/concat.html">Observable.concat</a></td>
    </tr>
    <tr>
        <td colspan="2">当另一个序列抛出错误时</td>
        <td><a href="../observable/observable_methods/catch.html">Observable.catch</a></td>
    </tr>
    <tr>
        <td colspan="2">不管先前的序列是完成还是抛出错误</td>
        <td><a href="../observable/observable_methods/onerrorresumenext.html">Observable.onErrorResumeNext</a></td>
    </tr>
    <tr>
        <td colspan="3">通过响应不同的值组合<a href="http://en.wikipedia.org/wiki/Join-calculus">（连接微积分）</a></td>
        <td><a href="../observable/observable_methods/when.html">Observable.when</a></td>
    </tr>
</tbody></table>

## 相关阅读 ##

*参考*
 - [`Observable`](../observable/observable_methods/index.html)

*概念*
- [查询可观察对象](../getting_started_with_rxjs/creating_and_querying_observable_sequences/querying_observable_sequences.html)
- [操作符分类](../getting_started_with_rxjs/creating_and_querying_observable_sequences/operators_by_category.html)
