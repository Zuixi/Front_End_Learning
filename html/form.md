### HTML form
> 表单用于搜集**不同类型的用户输入**
---
**form element**
```
<form>
form element
</from>
```
---
**the element in the html form**
> input 元素、复选框、单选按钮、提交按钮等

### Element of form 
**input element**
> input 元素是最重要的表单元素。input元素有很多形态，根据不同的**type**属性

for example:
type | description
---|---
**text** | 定义常规文本输入。
**radio** |	定义单选按钮输入（选择多个选择之一）
**submit**|	定义提交按钮（提交表单）

**注释**：表单本身并不可见。还要注意文本字段的默认宽度是 20 个字符

---
**radio element**

```
<input type="radio" name="sex" value="male" checked>Male<br>

<input type="radio" name="sex" value="female">Female<br>
```

---

**submit element**

<input type="submit"> 定义用于**向表单处理程序（form-handler）提交表单的按钮**。

表单处理程序通常是包含用来处理输入数据的脚本的服务器页面。

表单处理程序在**表单的action**属性中指定
---

**Action 属性**
action属性定义在**提交表单时执行的动作**,向服务器提交表单的通常做法是使用提交按钮。

通常，表单会被提交到 web 服务器上的网页; 如果**省略action属性**，则action会被设置为当前页面

