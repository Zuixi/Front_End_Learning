### collection
collection是model的有序组合，我们可以在`collection`上绑定`"change"`事件，从而当collection中的model发生变化时`fetch`（获得）通知，集合也可以监听`add`和 `remove`事件，从服务器更新，并能使用`Underscore.js`提供的方法。

collection中的model**触发的任何事件**都可以在集合身上直接触发，所以我们可以监听collection中model的变化： 

```
documents.on("change:selected", ...)
```
---
#### extend
```
Backbone.Collection.extend(properties, [classProperties])
```

通过扩展 Backbone.Collection创建一个Collection类。实例属性参数properties以及类属性参数classProperties会被直接注册到集合的构造函数。

**model**
```                   
collection.model
```

覆盖此属性来指定集合中包含的模型类。可以传入原始属性对象（和数组）来`add`, `create`和`reset`，传入的属性会被自动转换为适合的模型类型。

```
var Library = Backbone.Collection.extend({
  model: Book
});
```

**collection也可以包含多态模型**，通过用**构造函数重写**这个属性，返回一个模型。

```
var Library = Backbone.Collection.extend({

  model: function(attrs, options) {
    if (condition) {
      return new PublicDocument(attrs, options);
    } else {
      return new PrivateDocument(attrs, options);
    }
  }

});
```
---

#### constructor / initializenew

```
Backbone.Collection([models], [options])
```

当创建collection时，你可以选择传入初始的models数组。集合的comparator函数也可以作为选项传入。传递false作为comparator选项将阻止排序。

如果定义了`initialize`函数，会在collection创建时被调用。

有几个选项，如果提供的话，将直接附加到集合上：`model`和`comparator`。
通过传递null给models选项来创建一个空的集合。

```
var tabs = new TabSet([tab1, tab2, tab3]);
var spaces = new Backbone.Collection([], {
  model: Space
});

```