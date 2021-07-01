### 一些问题
1. **View和Model**如何引起关联呢？
    > 在View的对象中需要**传入特定的Model**，Model和View是一一对应的

    ```
    var myView = new formView({ model: myForm });
    ```
---
2. **在View中如何更新Model的数据呢**

    ```
    // traverse data array to set data of model
    if (this.array.length > 0) {

    for (var val of this.array) {
        //console.log(val);
        // it cannot do something correctly
        this.model.set(Object.keys(val)[0], Object.values(val)[0]);
    }
    this.model.save();

    // continue log data of model after set
    console.log('\n-------after save model----------');
    console.log(this.model.toJSON());

    // clear array for next save
    this.array = [];
    ```

    在上面的code中，如果我去`set`数据，那么将出现如下的错误：
    ```
    Uncaught TypeError: (intermediate value)(intermediate value).callback.call is not a function
    at triggerEvents (backbone.js:258)
    at child.trigger (backbone.js:165)
    at child.set (backbone.js:407)
    at child.OnSubmit (formView.js:59)
    at executeBound (underscore.js:762)
    at HTMLButtonElement.<anonymous> (underscore.js:775)
    at HTMLButtonElement.<anonymous> (underscore.js:122)
    at HTMLDivElement.dispatch (jquery.min.js:3)
    at HTMLDivElement.q.handle (jquery.min.js:3)
    ```

    对于上面的问题，我自己觉得可能是View的`this`在函数调用的时候出现了混乱，导致在执行事件的绑定函数的时候出现了混乱，所以不妨**将model需要处理的事件函数放在View中，而不是写在Model中**；

    **修改后的代码如下所示**
    ```
    initialize: function() {
        _.bindAll(this, 'render', 'OnChange');
        this.model.on('change', this.render, this);

        //this.listenTo(this.model, 'change', this.render);
    },

    render: function() {
        var html = this.template(this.model.toJSON());
        // console.log('\n---form view log message------');
        // console.log(html);
        // $el.html()是什么
        // console.log(this.$el.html());
        this.$el.html(html);

        return this;
    },

    Show: function() {
        alert('model event change');
    },
    ```

3. 如何**重写Model类的save函数**呢，并且需要返回一定的状态？
    我自己的重写方法如下所示，直接在Model里重写这个function，并且调用了父类的save function

    ```
    // overite save function
    var Base = Backbone.Model.extend({
        defaults: function() {
            return {
                id: null
            };
        },

        // overrite save
        save: function() {
            // use super class save function
            Backbone.Model.prototype.set.apply(this, arguments);
            
            console.log(this.toJSON());
        }
    });
    ```

