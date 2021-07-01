### Events的作用

**Events是一个可以融合到任何对象的模块**, 给予对象绑定和触发自定义事件的能力. 

Events在**绑定之前不需要声明, 并且还可以传递参数**

```
var object = {};

// 绑定前没有声明
_.extend(object, Backbone.Events);

object.on("alert", function(msg) {
  alert("Triggered " + msg);
});

object.trigger("alert", "an event");
```

---

#### on
```
object.on(event, callback, [context])    别名: bind
```

在`object`上绑定一个**callback回调函数**。只要**event触发**，该回调函数就会调用。

如果你的**一个页面含有大量的不同事件**，我们约定**使用冒号**来为事件添加`命名空间`使用冒号来命名：`"poll:start"`, 或 `"change:selection"`。

**事件字符串**也可能是用**空格分隔**的多个事件列表（注：即可以同时绑定多个事件，事件用空格分隔）

```
book.on("change:title change:author", ...);
```

当回调函数被调用时，**通过可选的第三个参数**可以为this提供一个context（上下文）值：`model.on('change', this.render, this)`（注：即回调函数中的This，指向传递的第三个参数）。

当回调函数被绑定到**特殊"all"事件**时，任何事件的发生都会触发该回调函数，回调函数的第一个参数会传递该事件的名称。举个例子，将一个对象的所有事件代理到另一对象：

```
proxy.on("all", function(eventName) {
  object.trigger(eventName);
});
```

所有Backbone事件方法**还支持事件映射**的语法， 作为可惜的位置参数：

```
book.on({
  "change:title": titleView.update,
  "change:author": authorPane.update,
  "destroy": bookView.remove
});

```

---

#### off
```
object.off([event], [callback], [context])    别名: unbind
```


从object对象**移除先前绑定的callback函数**。如果**没有指定context**，所有上下文下的这个回调函数将被移除。如果**没有指定callback**，所有绑定这个事件回调函数将被移除；如果**没有指定event**，所有事件的回调函数会被移除。

```
// Removes just the `onChange` callback.
object.off("change", onChange);

// Removes all "change" callbacks.
object.off("change");

// Removes the `onChange` callback for all events.
object.off(null, onChange);

// Removes all callbacks for `context` for all events.
object.off(null, null, context);

// Removes all callbacks on `object`.
object.off();

```
需要注意的是，调用`model.off()`确实会删除model上所有的事件**包括Backbone内部用来统计的事件**。

#### trigger
```
object.trigger(event, [*args])
```
触发给定event或用**空格隔开的事件**的回调函数。后续传入trigger的参数会传递到触发事件的回调函数里。

---

#### once
```
object.once(event, callback, [context])
```

用法跟on很像，区别在于绑定的回调函数**触发一次后就会被移除**（注：只执行一次）。简单的说就是“下次不在触发了，用这个方法”。

---
#### listenTo
```
object.listenTo(other, event, callback)
```

让object监听另一个`（other）`对象上的一个**特定事件**。不使用`other.on(event, callback, object)`，而使用这种形式的优点是：**listenTo允许object来跟踪这个特定事件，并且以后可以一次性全部移除它们**。

callback总是在object上下文环境中被调用。

```
view.listenTo(model, 'change', view.render);
```

---

#### stopListening
```
object.stopListening([other], [event], [callback])
```

让object**停止监听事件**。如果**调用不带参数的stopListening**，可以移除object下所有已经**registered(注册)的callback函数**，或者**只删除指定对象上明确告知的监听事件，或者一个删除指定事件，或者只删除指定的回调函数**。

```
view.stopListening();

view.stopListening(model);

```

#### listenToOnce
```
object.listenToOnce(other, event, callback)
```

用法跟 listenTo 很像，但是**事件触发一次后callback将被移除**。

