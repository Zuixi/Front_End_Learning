### Model

Models（模型）是任何Javascript应用的核心，**包括数据交互及与其相关的大量逻辑**： 转换、验证、计算属性和访问控制。

也可以用特定的方法扩展 Backbone.Model，Model 也提供了一组基本的管理变化的功能。

---
**extend**

```
Backbone.Model.extend(properties, [classProperties])
```

要**创建自己的Model**类，你可以**扩展Backbone.Model**并提供实例properties(属性) ， 以及可选的可以**直接注册到构造函数的classProperties**(类属性)。

extend可以**正确的设置原型链**，因此通过extend创建的子类 (subclasses) 也可以被深度扩展。


---
**constructor / initialize**
```
new Model([attributes], [options])
```

当创建model实例时，可以传入属性 (attributes)初始值，这些值会被set（设置）到 model。如果定义了**initialize**函数，该函数会在**model创建后执行**。

```
new Book({
  title: "One Thousand and One Nights",
  author: "Scheherazade"
});
```

在极少数的情况下，你可能需要去**重写constructor** ，它可以让你替换你的model的实际构造函数。

```
var Library = Backbone.Model.extend({
  constructor: function() {
    this.books = new Books();
    Backbone.Model.apply(this, arguments);
  },
  parse: function(data, options) {
    this.books.reset(data.books);
    return data.library;
  }
});
```


如果你传入`{collection: ...}` ，这个options表示这个**model属于哪个collection**，且用于计算这个model的url。否则`model.collection`这个属性会在你第一次添加model到一个collection的时候被自动添加。需要注意的是相反的是不正确的，因为传递这个选项给构造函数将不会自动添加model到集合。有时这个是很有用的。

如果`{parse: true}`被作为一个option选项传递，`attributes`将在set到model之前首先通过parse被转换。

---

**get**

```
model.get(attribute)
```
从当前model中获取当前属性(attributes)值，比如：`note.get("title")`

---
**set**

```
model.set(attributes, [options])
```

向model设置一个或多个hash属性(attributes)。如果任何一个属性**改变了model的状态**，在**不传入 {silent: true} 选项参数的情况下，会触发 "change"事件**，更改特定属性的事件也会触发。 可以绑定事件到某个属性;

**例如：**
```
change:title，及 change:content。

note.set({title: "March 20", content: "In his eyes she eclipses..."});

book.set("title", "A Scandal in Bohemia");
```
---

**fetch**

```
model.fetch([options])
```

通过**委托**给`Backbone.sync`从服务器**重置模型的状态**，**返回jqXHR**。

如果模型**从未填充数据时非常有用**，或者如果你**想确保你有最新的服务器状态**。

如果**服务器的状态不同于当前属性**的"change"事件将被触发。接受`success`和`error`回调的选项散列，这两个回调都可以传递(model, response, options)作为参数。

```
// 每隔 10 秒从服务器拉取数据以保持频道模型是最新的
setInterval(function() {
  channel.fetch();
}, 10000);
```
---
**save**

```
model.save([attributes], [options])
```

通过委托给Backbone.sync，**保存模型到数据库（或替代持久化层）**。如果验证成功，返回`jqXHR`，否则为`false`。

`attributes`散列（如set）应包含你想改变的属性 - 不涉及的键不会被修改 - 但是，该资源的一个完整表示将被发送到服务器。至于`set`，你可能会传递单独的键和值，而不是一个哈希值。

如果模型有一个`validate`方法，并且验证失败，该模型将不会被保存。

如果模型`isNew`，保存将采用"create"（HTTP POST），如果模型在服务器上**已经存在**，保存将采用"update"（HTTP PUT）。

相反，如果你**只想将改变属性发送到服务器**，调用`model.save(attrs, {patch: true})`。 你会得到一个**HTTP PATCH**请求将刚刚传入的属性发送到服务器。

通过**新的属性**调用save将**立即触发一个"change"事件**，一个"request"事件作为Ajax请求开始到服务器， 并且当**服务器确认成功修改后立即触发一个"sync"事件**。如果你想在模型上等待服务器设置新的属性，请传递{wait: true}。

在下面的例子中， 注意我们如何**覆盖Backbone.sync的版本**，在模型初次保存时接收到 "create"请求，第二次接收到 "update" 请求的。

```
Backbone.sync = function(method, model) {
  alert(method + ": " + JSON.stringify(model));
  model.set('id', 1);
};

var book = new Backbone.Model({
  title: "The Rough Riders",
  author: "Theodore Roosevelt"
});

book.save();

book.save({author: "Teddy"});
```

save支持在**选项散列表**中传入`success`和`error`回调函数，回调函数支持 **传入 (model, response, options) 作为参数**。 

如果服务端验证失败，返回非200的HTTP响应码，将产生文本或JSON的错误内容。

```
book.save("author", "F.D.R.", {error: function(){ ... }});
```