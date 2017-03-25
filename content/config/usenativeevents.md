## [`Rx.config.useNativeEvents`]()

确定该[`fromEvent`](../observable/observable_methods/fromevent.html)方法是否仅使用原生的DOM事件，并忽略引用的支持的库，例如[jQuery](http://jquery.com/), [Zepto.js](http://zeptojs.com/), [AngularJS](https://angularjs.org/), [Ember.js](http://emberjs.com/)和[Backbone.js](http://backbonejs.org)。

#### 例

例如，我们项目中引入了jQuery，但是，我们只需要使用原生DOM事件。

```html
<script src="jquery.js"></script>
<script src="rx.lite.js"></script>
```

我们可以通过设置`Rx.config.useNativeEvents`的值为 `true`来做到这一点。

```js
Rx.config.useNativeEvents = true;

Rx.Observable.fromEvent(document, 'mousemove')
  .subscribe(e => console.log('ClientX: %d, ClientY: %d', e.clientX, e.clientY));
```
