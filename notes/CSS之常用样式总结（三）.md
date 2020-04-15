#  CSS选择器、伪类、伪元素

## 一.CSS选择器

| 选择器          | 例子       | 例子描述                         |
| --------------- | ---------- | -------------------------------- |
| .class          | .intro     | 选择 class=”intro” 的所有元素。  |
| #id             | #firstname | 选择 id=”firstname” 的所有元素。 |
| *（通配符）     | `*`        | 选择所有元素                     |
| element(标签)   | p          | 选择所有 `` 标签元素。           |
| attribute(属性) | [target]   | 选择带有 target 属性所有元素。   |

选择器的优先级：

Id>class=属性>标签>通配符   

（就近原则）

## **二.CSS 伪类**

CSS 伪类用于向某些选择器添加特殊的效果。

| 属性                                                         | 描述                                     |
| ------------------------------------------------------------ | ---------------------------------------- |
| [:active](http://www.w3school.com.cn/cssref/pr_pseudo_active.asp) | 向被激活的元素添加样式。                 |
| [:focus](http://www.w3school.com.cn/cssref/pr_pseudo_focus.asp) | 向拥有键盘输入焦点的元素添加样式。       |
| [:hover](http://www.w3school.com.cn/cssref/pr_pseudo_hover.asp) | 当鼠标悬浮在元素上方时，向元素添加样式。 |
| [:link](http://www.w3school.com.cn/cssref/pr_pseudo_link.asp) | 向未被访问的链接添加样式。               |
| [:visited](http://www.w3school.com.cn/cssref/pr_pseudo_visited.asp) | 向已被访问的链接添加样式。               |
| [:first-child](http://www.w3school.com.cn/cssref/pr_pseudo_first-child.asp) | 向元素的第一个子元素添加样式。           |
| [:lang](http://www.w3school.com.cn/cssref/pr_pseudo_lang.asp) | 向带有指定 lang 属性的元素添加样式。     |

锚伪类

在支持 CSS 的浏览器中，链接的不同状态都可以不同的方式显示，这些状态包括：活动状态，已被访问状态，未被访问状态，和鼠标悬停状态。

```
a:link {color: #FF0000}		/* 未访问的链接 */
a:visited {color: #00FF00}	/* 已访问的链接 */
a:hover {color: #FF00FF}	/* 鼠标移动到链接上 */
a:active {color: #0000FF}	/* 选定的链接 */
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

[亲自试一试](http://www.w3school.com.cn/tiy/t.asp?f=csse_link)

提示：在 CSS 定义中，a:hover 必须被置于 a:link 和 a:visited 之后，才是有效的。

提示：在 CSS 定义中，a:active 必须被置于 a:hover 之后，才是有效的。

提示：伪类名称对大小写不敏感。

CSS 伪元素 (Pseudo-elements)

CSS 伪元素用于向某些选择器设置特殊效果。

## 语法

伪元素的语法：

| `1 `![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) | `selector:pseudo-element {property:value;} `![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                              |                                                              |

CSS 类也可以与伪元素配合使用：

| `1 `![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) | `selector.class:pseudo-element {property:value;} `![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                              |                                                              |



## 三.伪元素

| 属性                                                         | 描述                             |
| ------------------------------------------------------------ | -------------------------------- |
| [:first-letter](http://www.w3school.com.cn/cssref/pr_pseudo_first-letter.asp) | 向文本的第一个字母添加特殊样式。 |
| [:first-line](http://www.w3school.com.cn/cssref/pr_pseudo_first-line.asp) | 向文本的首行添加特殊样式。       |
| [:before](http://www.w3school.com.cn/cssref/pr_pseudo_before.asp) | 在元素之前添加内容。             |
| [:after](http://www.w3school.com.cn/cssref/pr_pseudo_after.asp) | 在元素之后添加内容。             |



```
注释："first-line" 伪元素只能用于块级元素。
注释：下面的属性可应用于 "first-line" 伪元素：
font   background   word-spacing   letter-spacing
text-decoration   vertical-align   text-transform
line-height   clear    color 


注释："first-letter" 伪元素只能用于块级元素。
注释：下面的属性可应用于 "first-letter" 伪元素：
 font  color  background  margin  padding
 border  text-decoration   vertical-align (仅当 float 为 none 时)
 text-transform  line-height   float     clear
```

