# 我该选用哪个操作符? - 实例操作符 #

使用此页面通过类型查找[`Observable`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md) 适合您需要的实例运算符：

<table style="display: table">
<thead>实例操作符</thead>
<tbody>
    <!-- Observable operators -->
    <tr>
        <td rowspan="71">使用现有的序列</td>
        <td colspan="3">我想改变每个值</td>
        <td><a href="../observable/observable_instance_methods/select.html">map/select</a></td>
    </tr>
    <tr>
        <td colspan="3">我想从每个值拉一个属性</td>
        <td><a href="../observable/observable_instance_methods/pluck.html">pluck</a></td>
    </tr>
    <tr>
        <td colspan="3">我想在不影响值的情况下被通知值</td>
        <td><a href="../observable/observable_instance_methods/do.html">do/tap</a><br>
            <a href="../observable/observable_instance_methods/doonnext.html">doOnNext/tapOnNext</a><br>
            <a href="../observable/observable_instance_methods/doonerror.html">doOnError/tapOnError</a><br>
            <a href="../observable/observable_instance_methods/dooncompleted.html">doOnCompleted/tapOnCompleted</a></td>
    </tr>
    <tr>
        <td rowspan="6">我想包含值</td>
        <td colspan="2">基于自定义逻辑</td>
        <td><a href="../observable/observable_instance_methods/where.html">filter/where</a></td>
    </tr>
    <tr>
        <td rowspan="2">从序列开头</td>
        <td></td>
        <td><a href="../observable/observable_instance_methods/take.html">take</a></td>
    </tr>
    <tr>
        <td>基于自定义逻辑</td>
        <td><a href="../observable/observable_instance_methods/takewhile.html">takeWhile</a></td>
    </tr>
    <tr>
    </tr>
    <tr>
        <td colspan="2">从序列的末尾</td>
        <td><a href="../observable/observable_instance_methods/takelast.html">takeLast</a></td>
    </tr>
    <tr>
        <td colspan="2">直到另一个序列发射一个值或完成</td>
        <td><a href="../observable/observable_instance_methods/takeuntil.html">takeUntil</a></td>
    </tr>
    <tr>
        <td rowspan="7">我想忽略值</td>
        <td colspan="2">全部</td>
        <td><a href="../observable/observable_instance_methods/ignoreelements.html">ignoreElements</a></td>
    </tr>
    <tr>
        <td rowspan="2">从序列的开头</td>
        <td></td>
        <td><a href="../observable/observable_instance_methods/skip.html">skip</a></td>
    </tr>
    <tr>
        <td>基于自定义逻辑</td>
        <td><a href="../observable/observable_instance_methods/skipwhile.html">skipWhile</a></td>
    </tr>
    <tr>
        <td colspan="2">从序列的末尾</td>
        <td><a href="../observable/observable_instance_methods/skiplast.html">skipLast</a></td>
    </tr>
    <tr>
        <td colspan="2">直到另一个序列发出一个值</td>
        <td><a href="../observable/observable_instance_methods/skipuntil.html">skipUntil</a></td>
    </tr>
    <tr>
        <td colspan="2">与以前的值相同</td>
        <td><a href="../observable/observable_instance_methods/distinctuntilchanged.html">distinctUntilChanged</a></td>
    </tr>
    <tr>
        <td colspan="2">这（触发）太频繁</td>
        <td><a href="../observable/observable_instance_methods/throttle.html">throttle</a></td>
    </tr>
    <tr>
        <td rowspan="4">我想计算</td>
        <td>总和</td>
        <td rowspan="2">这些值的</td>
        <td><a href="../observable/observable_instance_methods/sum.html">sum</a></td>
    </tr>
    <tr>
        <td>平均值</td>
        <td><a href="../observable/observable_instance_methods/average.html">average</a></td>
    </tr>
    <tr>
        <td rowspan="2">使用自定义逻辑</td>
        <td>并且只输出最终值</td>
        <td><a href="../observable/observable_instance_methods/aggregate.html">aggregate</a><br>
            <a href="../observable/observable_instance_methods/reduce.html">reduce</a>
        </td>
    </tr>
    <tr>
        <td>并在计算出值时输出（每一步的）值</td>
        <td><a href="../observable/observable_instance_methods/scan.html">scan</a></td>
    </tr>
    <tr>
        <td rowspan="3">我想用元数据包装它的消息</td>
        <td colspan="2">描述每个消息</td>
        <td><a href="../observable/observable_instance_methods/materialize.html">materialize</a></td>
    </tr>
    <tr>
        <td colspan="2">包括从最后一个价值以来的时间</td>
        <td><a href="../observable/observable_instance_methods/timeinterval.html">timeInterval</a></td>
    </tr>
    <tr>
        <td colspan="2">包括时间戳</td>
        <td><a href="../observable/observable_instance_methods/timestamp.html">timestamp</a></td>
    </tr>
    <tr>
        <td rowspan="2">经过一段时间的不活动</td>
        <td colspan="2">我想抛出一个错误</td>
        <td><a href="../observable/observable_instance_methods/timeout.html">timeout</a></td>
    </tr>
    <tr>
        <td colspan="2">我想切换到另一个序列</td>
        <td><a href="../observable/observable_instance_methods/timeout.html">timeout</a></td>
    </tr>
    <tr>
        <td rowspan="2">我想确保只有一个值</td>
        <td colspan="2">并且如果存在多于或少于一个值则抛出错误</td>
        <td><a href="../observable/observable_instance_methods/single.html">single</a></td>
    </tr>
    <tr>
        <td colspan="2">并且如果没有值，则使用默认值</td>
        <td><a href="../observable/observable_instance_methods/singleordefault.html">singleOrDefault</a></td>
    </tr>
    <tr>
        <td rowspan="3">我只想取第一个值</td>
        <td colspan="2">并且如果没有值，则抛出错误</td>
        <td><a href="../observable/observable_instance_methods/first.html">first</a></td>
    </tr>
    <tr>
        <td colspan="2">并且如果没有值，则使用默认值</td>
        <td><a href="../observable/observable_instance_methods/firstordefault.html">firstOrDefault</a></td>
    </tr>
    <tr>
        <td colspan="2">在一段时间内</td>
        <td><a href="../observable/observable_instance_methods/sample.html">sample</a></td>
    </tr>
    <tr>
        <td rowspan="2">我只想取最后的值</td>
        <td colspan="2">如果没有值，则报错</td>
        <td><a href="../observable/observable_instance_methods/last.html">last</a></td>
    </tr>
    <tr>
        <td colspan="2">并且如果没有值，则使用默认值</td>
        <td><a href="../observable/observable_instance_methods/lastordefault.html">lastOrDefault</a></td>
    </tr>
    <tr>
        <td colspan="3">我想知道它包含多少值</td>
        <td><a href="../observable/observable_instance_methods/count.html">count</a></td>
    </tr>
    <tr>
        <td colspan="3">我想知道它是否包含一个指定的值</td>
        <td><a href="../observable/observable_instance_methods/includes.html">contains</a></td>
    </tr>
    <tr>
        <td rowspan="2">我想知道条件是否满足</td>
        <td colspan="2">只需要任一值满足</td>
        <td><a href="../observable/observable_instance_methods/any.html">any/some</a></td>
    </tr>
    <tr>
        <td colspan="2">需要所有值都满足</td>
        <td><a href="../observable/observable_instance_methods/every.html">all/every</a></td>
    </tr>
    <tr>
        <td rowspan="2" colspan="2">我想把消息延迟一段特定的时间</td>
        <td></td>
        <td><a href="../observable/observable_instance_methods/delay.html">delay</a></td>
    </tr>
    <tr>
        <td>基于自定义逻辑</td>
        <td><a href="../observable/observable_instance_methods/delaywithselector.html">delayWithSelector</a></td>
    </tr>
    <tr>
        <td rowspan="11">我想给值分组</td>
        <td colspan="2">直到序列完成</td>
        <td>
          <a href="../observable/observable_instance_methods/toarray.html">toArray</a><br>
          <a href="../observable/observable_instance_methods/tomap.html">toMap</a><br>
          <a href="../observable/observable_instance_methods/toset.html">toSet</a>
        </td>
    </tr>
    <tr>
        <td rowspan="2">使用自定义逻辑</td>
        <td>作为数组</td>
        <td><a href="../observable/observable_instance_methods/buffer.html">buffer</a></td>
    </tr>
    <tr>
        <td>作为序列</td>
        <td><a href="../observable/observable_instance_methods/window.html">window</a></td>
    </tr>
    <tr>
        <td rowspan="2">根据特定大小分批</td>
        <td>作为数组</td>
        <td><a href="../observable/observable_instance_methods/bufferwithcount.html">bufferWithCount</a></td>
    </tr>
    <tr>
        <td>作为序列</td>
        <td><a href="../observable/observable_instance_methods/windowwithcount.html">windowWithCount</a></td>
    </tr>
    <tr>
        <td rowspan="2">基于时间</td>
        <td>作为数组</td>
        <td><a href="../observable/observable_instance_methods/bufferwithtime.html">bufferWithTime</a></td>
    </tr>
    <tr>
        <td>作为序列</td>
        <td><a href="../observable/observable_instance_methods/windowwithtime.html">windowWithTime</a></td>
    </tr>
    <tr>
        <td rowspan="2">基于时间或计数，以先发生者为准</td>
        <td>作为数组</td>
        <td><a href="../observable/observable_instance_methods/bufferwithtimeorcount.html">bufferWithTimeOrCount</a></td>
    </tr>
    <tr>
        <td>作为序列</td>
        <td><a href="../observable/observable_instance_methods/windowwithtimeorcount.html">windowWithTimeOrCount</a></td>
    </tr>
    <tr>
        <td rowspan="2">基于一个指定的key</td>
        <td>直到序列完成</td>
        <td><a href="../observable/observable_instance_methods/groupby.html">groupBy</a></td>
    </tr>
    <tr>
        <td>并控制每组的生命周期</td>
        <td><a href="../observable/observable_instance_methods/groupbyuntil.html">groupByUntil</a></td>
    </tr>
    <tr>
        <td rowspan="6">我想为每个值开始一个新的序列</td>
        <td colspan="2">并且并行地从所有序列中发出值</td>
        <td><a href="../observable/observable_instance_methods/selectmany.html">flatMap/selectMany</a></td>
    </tr>
    <tr>
        <td colspan="2">并按顺序从每个序列中输出值</td>
        <td><a href="../observable/observable_instance_methods/concatmap.html">concatMap/selectConcat</a></td>
    </tr>
    <tr>
        <td colspan="2">并在新值到达时取消先前的序列</td>
        <td><a href="../observable/observable_instance_methods/flatmaplatest.html">flatMapLatest/selectSwitch</a></td>
    </tr>
    <tr>
        <td colspan="2">并递归地为每个新值启动一个新的序列</td>
        <td><a href="../observable/observable_instance_methods/expand.html">expand</a></td>
    </tr>
    <tr>
        <td colspan="2">并根据onNext，onError和onCompleted并行地从所有序列发出值</td>
        <td><a href="../observable/observable_instance_methods/flatmapobserver.html">flatMapObserver/selectManyObserver</a></td>
    </tr>
    <tr>
        <td colspan="2">并根据onNext，onError和onCompleted顺序地从所有序列发出值</td>
        <td><a href="../observable/observable_instance_methods/flatmapobserver.html">concatMapObserver/selectConcatObserver</a></td>
    </tr>
    <tr>
        <td>我想把它与另一个结合起来</td>
        <td colspan="2">两者都完成时发出通知</td>
        <td><a href="../observable/observable_instance_methods/forkjoin.html">forkJoin</a></td>
    </tr>
    <tr>
        <td colspan="3">我想执行复杂的操作，而不会打破流畅的调用</td>
        <td><a href="../observable/observable_instance_methods/let.html">let</a></td>
    </tr>
    <tr>
        <td rowspan="5">我想在多个订阅者之间共享订阅</td>
        <td colspan="2">使用特定的`subject`实现</td>
        <td><a href="../observable/observable_instance_methods/multicast.html">multicast</a></td>
    </tr>
    <tr>
        <td colspan="2"></td>
        <td>
          <a href="../observable/observable_instance_methods/publish.html">publish</a><br>
          <a href="../observable/observable_instance_methods/share.html">share</a>
        </td>
    </tr>
    <tr>
        <td colspan="2">并向未来订阅者提供最后的值</td>
        <td>
          <a href="../observable/observable_instance_methods/publishlast.html">publishLast</a><br>
          <a href="../observable/observable_instance_methods/sharelast.html">shareLast</a>
        </td>
    </tr>
    <tr>
        <td colspan="2">并向未来订阅者重播默认值或最新值</td>
        <td>
          <a href="../observable/observable_instance_methods/publishvalue.html">publishValue</a><br>
          <a href="../observable/observable_instance_methods/sharevalue.html">shareValue</a>
        </td>
    </tr>
    <tr>
        <td colspan="2">并向未来的订阅者重播n个值</td>
        <td>
          <a href="../observable/observable_instance_methods/publish.html">replay</a><br>
          <a href="../observable/observable_instance_methods/share.html">shareReplay</a>
        </td>
    </tr>
    <tr>
        <td rowspan="3">发生错误时</td>
        <td colspan="2">我想重新订阅</td>
        <td><a href="../observable/observable_instance_methods/retry.html">retry</a></td>
    </tr>
    <tr>
        <td rowspan="2">我想开始一个新序列</td>
        <td></td>
        <td><a href="../observable/observable_instance_methods/catch.html">catch</a></td>
    </tr>
    <tr>
        <td>取决于错误</td>
        <td><a href="../observable/observable_instance_methods/catch.html">catch</a></td>
    </tr>
    <tr>
        <td rowspan="2">当完成时</td>
        <td colspan="2">我想重新订阅</td>
        <td><a href="../observable/observable_instance_methods/repeat.html">repeat</a></td>
    </tr>
    <tr>
        <td colspan="2">我想开始一个新序列</td>
        <td><a href="../observable/observable_instance_methods/concat.html">concat</a></td>
    </tr>
    <tr>
        <td>当完成或抛出错误时</td>
        <td colspan="2">我想开始一个新序列</td>
        <td><a href="../observable/observable_instance_methods/onerrorresumenext.html">onErrorResumeNext</a></td>
    </tr>
    <tr>
        <td>当完成，抛出错误或退订时</td>
        <td colspan="2">我想执行一个函数</td>
        <td><a href="../observable/observable_instance_methods/finally.html">finally</a></td>
    </tr>
    <tr>
        <td rowspan="2">我想改变路由的调度程序</td>
        <td colspan="2">调用`subscribe`（订阅）</td>
        <td><a href="../observable/observable_instance_methods/subscribeon.html">subscribeOn</a></td>
    </tr>
    <tr>
        <td colspan="2">消息</td>
        <td><a href="../observable/observable_instance_methods/observeon.html">observeOn</a></td>
    </tr>
    <tr>
        <td rowspan="9">使用两个序列</td>
        <td>我想决定从哪个接收值</td>
        <td colspan="2">取决于哪个序列先发出值</td>
        <td><a href="../observable/observable_instance_methods/amb.html">amb</a></td>
    </tr>
    <tr>
        <td colspan="3">我想确定它们的值是否相等</td>
        <td><a href="../observable/observable_instance_methods/sequenceequal.html">sequenceEqual</a></td>
    </tr>
    <tr>
        <td rowspan="5">我想合并它们的值</td>
        <td colspan="2">只有当第一个序列发射时，使用每个序列的最新值</td>
        <td><a href="../observable/observable_instance_methods/withlatestfrom.html">withLatestFrom</a></td>
    </tr>
    <tr>
        <td rowspan="2">为了</td>
        <td>不改变时重复使用最新值</td>
        <td><a href="../observable/observable_instance_methods/combinelatest.html">combineLatest</a></td>
    </tr>
    <tr>
        <td>每个值只使用一次</td>
        <td><a href="../observable/observable_instance_methods/zip.html">zip</a></td>
    </tr>
    <tr>
        <td rowspan="2">重复分享我选择的“生命周期”</td>
        <td>并通知每个组合</td>
        <td><a href="../observable/observable_instance_methods/join.html">join</a></td>
    </tr>
    <tr>
        <td>并给每个“左”的序列的值给“右”的序列</td>
        <td><a href="../observable/observable_instance_methods/groupjoin.html">groupJoin</a></td>
    </tr>
    <tr>
        <td colspan="3">我想包含两者的值</td>
        <td><a href="../observable/observable_instance_methods/merge.html">merge</a></td>
    </tr>
</tbody></table>

## 相关阅读 ##

*参考*
 - [`Observable`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md)

*概念*
- [查询可观察序列](querying.md)
- [操作符分类](categories.md)
