### 1.边框属性

```
border-radius 属性用于创建圆角
box-shadow 用于向方框添加阴影
border-image 属性允许您规定用于边框的图片
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 2.背景属性

background-size 属性规定背景图片的尺寸。
background-origin 属性规定背景图片的定位区域。

背景图片可以放置于 content-box、padding-box 或 border-box 区域
CSS3 允许您为元素使用多个背景图像。

实例
为 body 元素设置两幅背景图片：

```
body
{ 
background-image:url(bg_flower.gif),url(bg_flower_2.gif);
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

background-clip 规定背景的绘制区域。
background-origin 规定背景图片的定位区域。
background-size 规定背景图片的尺寸。

### 3.text-shadow 可向文本应用阴影

向标题添加阴影：

```
h1
{
text-shadow: 5px 5px 5px #FF0000;
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

`p {  word-wrap:break-word;  }     word-wrap 属性允许您允许文本强制文本进行换行`


实例
允许对长单词进行拆分，并换行到下一行：

### 4.font-family 属性

引用字体的名称 (myFirstFont)

实例

```
<style> 
@font-face
{
font-family: myFirstFont;
src: url('Sansation_Light.ttf'),
     url('Sansation_Light.eot'); /* IE9+ */
}
div
{
font-family:myFirstFont;
}
</style>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 5.2D 转换

```
translate() 方法，元素从其当前位置移动，根据给定的 left（x 坐标） 和 top（y 坐标） 位置参数

      translate(50px,100px) 把元素从左侧移动 50 像素，从顶端移动 100 像素。




rotate() 方法，元素顺时针旋转给定的角度。允许负值，元素将逆时针旋转。
          值 rotate(30deg) 把元素顺时针旋转 30 度。



scale() 方法，元素的尺寸会增加或减少，根据给定的宽度（X 轴）和高度（Y 轴）参数
      值 scale(2,4) 把宽度转换为原始尺寸的 2 倍，把高度转换为原始高度的 4 倍。



skew() 方法，元素翻转给定的角度，根据给定的水平线（X 轴）和垂直线（Y 轴）参数：
      值 skew(30deg,20deg) 围绕 X 轴把元素翻转 30 度，围绕 Y 轴翻转 20 度



matrix() 方法把所有 2D 转换方法组合在一起。
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```
div
{
transform: translate(50px,100px);
-ms-transform: translate(50px,100px);		/* IE 9 */
-webkit-transform: translate(50px,100px);	/* Safari and Chrome */
-o-transform: translate(50px,100px);		/* Opera */
-moz-transform: translate(50px,100px);		/* Firefox */
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 6.3D 转换

```
rotateX() 方法，元素围绕其 X 轴以给定的度数进行旋转。
  rotateY() 方法，元素围绕其 Y 轴以给定的度数进行旋转
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)