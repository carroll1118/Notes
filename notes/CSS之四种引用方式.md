# css的四种引入方式

## 1.行内样式

最简单的一种，直接对HTML标签使用 style=" "

```
例如：
<p style="color:#F00; "></p>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

缺点：HTML页面不纯净，文件体积大，不利于浏览器编译，后期维护不方便。

## 2.内嵌样式

内嵌样式就是将CSS代码写在<head></head>之间，并且用<style type="text/css"> </style>进行声明

```
例如：

	 <style type="text/css">
            p{
                color: blue;
            }
        </style>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

优缺点：页面使用公共CSS代码，也是每个页面都要定义的，如果一个网站有很多页面，每个文件都会变大，后期维护难度也大，如果文件很少，CSS代码也不多，这种样式还是很不错的。

## 3.外部样式

###   3.1.链接样式（推荐）


  链接样式是使用频率最高，最实用的样式，只需要在<head></head>之间加上<link…/>就可以了，如下：

```
  <link type="text/css" rel="stylesheet" href="style.css" />
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

   优缺点：实现了页面框架代码与表现CSS代码的完全分离，使得前期编写代码和后期代码维护都十分方便

###   3.2.导入样式（不建议使用）

  导入样式和链接样式比较相似，采用@import样式导入CSS样式表，在HTML初始化时，会被导入到HTML或者CSS文件中，  成为文件的一部分，类似第二种内嵌样式。

  @import在html中使用，如下：

    <style type="text/css">    @import url(style.css);    </style>

  @import在CSS中使用，如下：

  @import url(style.css);

## 4. 链接式和导入式的区别

   <link>
    1、属于XHTML
    2、优先加载CSS文件到页面
  @import
    1、属于CSS2.1
    2、先加载HTML结构在加载CSS文件。

## 5.四种CSS引入方式的优先级

1.就近原则

2.理论上：行内>内嵌>链接>导入

3.实际上：内嵌、链接、导入在同一个文件头部，谁离相应的代码近，谁的优先级高（页面多种方式使用css样式引入）

```
综合代码如下：

<!DOCTYPE>
<html>
    <head>
        <meta charset="utf-8" />
        <title>行内样式/内嵌样式和外部样式的优先级</title>
 
        <!--外部样式表-->
        <link rel="stylesheet" type="text/css" href="css/style.css" />
 
        <!--内嵌样式表-->
        <style type="text/css">
            p{
                color: blue;
            }
        </style>
 
        <!--外部样式表-->
        <!--<link rel="stylesheet" type="text/css" href="css/style.css" />-->
    </head>
    <body>
        <p>我是p段落（注意：内嵌样式和外部引入样式 离我最近的那种样式方式给我带来的影响）</p>
        <div >我是div，我什么样式都没有</div>
 
        <!--行内样式-->
        <div style="color: hotpink;">我是行内样式</div>
 
        <ol>
            <li>欢迎进入博客一起学习</li>
            <li>https://blog.csdn.net/qq_40722827</li>
        </ol>
    </body>
</html>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)