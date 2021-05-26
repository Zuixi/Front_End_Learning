### border
>`border`属性允许您指定**元素边框的样式、宽度和颜色**

#### CSS 边框样式
> border-style 属性指定要**显示的边框类型**

允许以下值：

* **dotted** - 定义点线边框
* **dashed** - 定义虚线边框
* **solid** - 定义实线边框
* **double** - 定义双边框
* **groove** - 定义3D坡口边框。效果取决于border-color值
* **ridge** - 定义3D脊线边框。效果取决于border-color值
* **inset** - 定义3D inset边框。效果取决于border-color值
* **outset** - 定义3D outset边框。效果取决于border-color值
* **none** - 定义无边框
* **hidden** - 定义隐藏边框

**border-style 属性可以设置一到四个值（用于上边框、右边框、下边框和左边框**

**除非设置了 border-style 属性，否则其他 CSS 边框属性（将在下一章中详细讲解）都不会有任何作用！**

---

#### CSS 边框宽度
**border-width**属性指定四个边框的宽度。

可以将宽度设置为特定大小（以px、pt、cm、em计），也可以使用以下三个预定义值之一：**thin、medium 或 thick**


---
#### CSS 边框颜色
`border-color`属性用于设置四个边框的颜色。

**可以通过以下方式设置颜色**

* **name** - 指定颜色名，比如 "red"
* **HEX** - 指定十六进制值，比如 "#ff0000"
* **RGB** - 指定 RGB 值，比如 "rgb(255,0,0)"
* **HSL** - 指定 HSL 值，比如 "hsl(0, 100%, 50%)"
* **transparent**

**注释：如果未设置 border-color，则它将继承元素的颜色**

----
#### CSS 边框各边
在 CSS 中，还有一些属性**可用于指定每个边框**（顶部、右侧、底部和左侧）：

```
p {
  border-top-style: dotted;
  border-right-style: solid;
  border-bottom-style: dotted;
  border-left-style: solid;
}
```

**工作原理**
1. 如果 border-style 属性设置四个值：
    > border-style: dotted solid double dashed;
    上边框是虚线
    右边框是实线
    下边框是双线
    左边框是虚线
2. 如果 border-style 属性设置三个值：
    >border-style: dotted solid double;
    上边框是虚线
    右和左边框是实线
    下边框是双线

3. 如果 border-style 属性设置两个值：  
    >border-style: dotted solid;
    上和下边框是虚线
    右和左边框是实线

4. 如果 border-style 属性设置一个值：
    >border-style: dotted;
    四条边均为虚线