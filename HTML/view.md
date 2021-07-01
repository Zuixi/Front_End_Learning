### View
Backbone视图几乎约定比他们的代码多：他们并**不限定你的HTML或CSS**，并可以配合使用任何JavaScript模板库。 

一般是组织您的**接口转换成逻辑视图**，通过模型的支持，模型变化时，每一个都可以独立地进行更新，而不必重新绘制该页面。我们再也不必钻进JSON对象中，查找DOM元素，手动更新 HTML了，通过绑定视图的`render`函数到模型的`change`事件：**模型数据会即时的显示在 UI中**。

---
#### extend
```
Backbone.View.extend(properties, [classProperties])
```

开始**创建自定义的视图类**。通常我们需要重载`render`函数，声明`events，以及通过 `tagName`, `className`, 或`id`为视图指定根元素。

```
var DocumentRow = Backbone.View.extend({

  tagName: "li",

  className: "document-row",

  events: {
    "click .icon":          "open",
    "click .button.edit":   "openEditDialog",
    "click .button.delete": "destroy"
  },

  initialize: function() {
    this.listenTo(this.model, "change", this.render);
  },

  render: function() {
    ...
  }

});
```
直到**运行时**，像tagName, id, className, el, events这样的属性也可以**被定义为一个函数**

---

#### constructor / initialize
```
new View([options])
```

有几个特殊的选项，如果传入，则直接注册到视图中去：**model, collection, el, id, className, tagName, attributes和events**。

 如果视图**定义了一个initialize初始化**函数，首先创建视图时，它会**立刻被调用**。 如果希望创建一个指向**DOM中已存在的元素**的视图，传入该元素作为选项：`new View({el: existingElement})`。

```
var doc = documents.first();

new DocumentRow({
  model: doc,
  id: "document-row-" + doc.id
});
```

---

#### el

**所有的视图都拥有一个DOM元素**（el属性），即使该元素仍未插入页面中去。视图可以在任何时候渲染，然后一次性插入DOM中去，这样能**尽量减少reflows和repaints**从而获得高性能的UI渲染。**this.el**可以从视图的`tagName, className, id和attributes`创建，如果都未指定，el会是一个`空div`

```
var ItemView = Backbone.View.extend({
  tagName: 'li'
});

var BodyView = Backbone.View.extend({
  el: 'body'
});

var item = new ItemView();
var body = new BodyView();

alert(item.el + ' ' + body.el);

```

---

### $ (jQuery)

```
view.$(selector)
```

如果页面中**引入了jQuery**，**每个视图都将拥有$函数**，可以在视图元素查询作用域内运行。

如果使用该作用域内的jQuery函数，就不需要从列表中指定的元素获取模型的ids这种查询了，我们可以更多的**依赖HTML class**属性。它等价于运行：view.$el.find(selector)。


```
ui.Chapter = Backbone.View.extend({
  serialize : function() {
    return {
      title: this.$(".title").text(),
      start: this.$(".start-page").text(),
      end:   this.$(".end-page").text()
    };
  }
});
```
---

#### template
```
view.template([data])
```

虽然**模板化的视图**不是Backbone直接提供的一个功能，它往往是一个在你视图定义template函数很好的约定。如此渲染你的视图时，可以方便地访问实例数据。例如使用Underscore的模板：

```
var LibraryView = Backbone.View.extend({
  template: _.template(...)
});
```
---

#### render
```
view.render()
```

render默认实现是没有操作的。 

**重载本函数**可以实现从**模型数据渲染视图模板**，并可用新的HTML更新this.el。 推荐的做法是在render函数的**末尾return this以开启链式调用**。

```
var Bookmark = Backbone.View.extend({
  template: _.template(...),
  render: function() {
    this.$el.html(this.template(this.model.attributes));

    // return this 链式调用
    return this;
  }
});
```

Backbone并不知道您首选HTML模板的方法。`render`函数中可以采用拼接HTML字符串，或者使用`document.createElement`生成DOM树。

但还是建议选择一个好的Javascript模板引擎。`Mustache.js`, `Haml-js`和`Eco`都是很好的选择。因为`Underscore.js`已经引入页面了，如果你喜欢简单的插入JavaScript的样式模板。` _.template`可以使用并是一个很好的选择。

无论基于什么考虑，都**永远不要在Javascript中拼接HTML字符串**。在DocumentCloud中，我们使用`Jammit`来打包JavaScript模板，并存储在`/app/views`中，作为我们主要的`core.js`包的一部分。