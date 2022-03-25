# HTML+CSS

## 主流浏览器及其内核

**IE               ------>     trident**

**firefox        ------>      Gecko**

**chrome      ------>       Webkit/blink**

**Safari        ------>        Webkit**

**Opera        ------>        presto**

## HTML(Hypertext Markup Language超文本标记语言)

### 简单标签

\<p>，段落标签，独成一段

\<h>，标题标签，h1到h6，独成一段

\<strong>，加粗标签

\<em>，斜体标签

\<del>，删除线标签

\<address>，地址标签，斜体并成段展示

\<div>，\<span>，充当容器，使网页结构化

在html中，回车和空格均为分隔符

\&nbsp;空格符

\&lt;     小于号，均为&开头，分号结尾（little than）

\&gt;   大于号（great than）

\<br>，回车

### 高级标签

**\<ol>**，有序列表，没太大用。。。。。。

属性：

type类型：1，a，A，i，I分别为数字，小写字母，大写字母，罗马数字和大写罗马数字，其中大小写字母为27进制

reversed:"reversed"，可对有序列表进行逆序，

start="num"，从几号开始排

**\<ul>，无序列表，**

属性：type，默认值为disc（实心圆），square（实心方块），circle（空心圆）

**\<img>**，src（source）属性为图片地址，alt属性为img标签的图片占位符，当图片出错时可展示文字信息，title为图片提示符，鼠标停留时会展示图片信息

引入图片方式：1、网上url

​						2、本地的绝对路径，如D:/a/b/test.jpg，若位于同一文件夹下，可以不加路径直接用文件名称

​						3、本地的相对路径，如../testworkspace/logo.png

**\<a>**，用来存放超链接，内部可存放任何标签

作用：1、超链接

​			2、锚点：可用href配合id标签来在当前页面进行跳转，如href="#xxx"，会跳转到id为xxx的标签，应用：如某些网页教程的目录跳转功能

​			3、打电话：href="tel:18306890406"

​			4、发邮件：href="mailto:760371482@qq.com"

​			5、协议限定符，在href中可写入javascript:xxxxx来执行js代码

href属性为超链接地址，target属性为跳转方式（_blank为跳转到新页面，\_self为跳转到自身，还有\_top和\_parent）

**\<form>**，用于为用户创建HTML表单，form表单中常用的标签有\<input>、\<select>，表单用于向服务器传输数据

form标签若要提交，其中的标签下必须有name属性

```html
<form method="get" action="">
    <p>
        username:<input type="text" name="username">
    </p>
    <p>
        username:<input type="password" name="password">
    </p>
    <input type="submit">
</form>
//当提交成功时url会变为如下
http://localhost:63342/Webstromfiles/%20HTML+CSS/%E5%9F%BA%E7%A1%80%E6%A0%87%E7%AD%BE.html?username=111&password=123
```

**\<input>**，可作为输入框，也可用作选择器（当name相同时，相同name的标签只能选一个）如：

```html
111<input type="radio" name="star" value="111">
222<input type="radio" name="star" value="222">
333<input type="radio" name="star" value="333">
//value值可用来在form表单中传值
```

type=text/password(密码框)/submit(提交按钮)/radio(单选框)/checkbox(复选框)

在单选框中，**可用checked="checked"属性来进行默认选中**，如：

```html
<input type="radio" name="star" value="333" checked="checked">
```

账号密码框，如：

```html
username<input type="text" value="请输入账号" onfocus="if(this.value=='请输入账号'){this.value=''}">
password<input type="text" value="请输入密码" onfocus="if(this.value=='请输入账号'){this.value=''}">
```

**\<select>**，下拉菜单，写法如下

```html
<select name="province">
    <option>beijing</option>
    <option>shanghai</option>
    <option>wuhan</option>
</select>
```

## CSS(cascading style sheet层叠样式表)

### 引入CSS

1、行间样式，即直接写入到标签中

2、页面级css，在\<head>标签中写入\<style>标签，然后在这个标签中写入css样式

3、外部css文件，在\<head>标签中写入\<link rel="stylesheet" type="text/css" href="\<css所在目录>"

### 选择器

1、id选择器，写法#\<id>{··· ···}，id与元素为一对一的关系

2、类选择器，写法.\<class>{··· ···}，class与元素为多对多的关系，即元素的class可以为多个值，写法为class="value1 value2"

3、标签选择器，写法：标签名{··· ···}

4、通配符选择器，*{··· ···}，对html文件中的所有标签起作用，包括\<html>,\<body>等

5、属性选择器，如：[id]、[class]、[id="xxx"]、[class="xxx"]

#### 选择器优先级

id选择器优先级高于class选择器，class选择器优先级又高于标签选择器，标签选择器优先级高于通配符选择器，

行间样式 > 页面级css = 外部css文件，可用!important来提升优先级

**!important >**  **id > class = 属性 > 标签 > ***（越具体优先级越高）

#### 选择器补充

1、父子选择器\<父标签> \<子标签>，即通过父标签，空格，子标签的方式实现，如：

\<div> \<em>{}选中的为div下的所有em标签

2、直接子元素选择器\<父标签> > <子标签>，即通过>来实现

3、并列选择器，标签名.class值或标签名#id值

4、分组选择器，如：em, span, strong{··· ···}，可同时对多个兄弟标签进行属性设置，该选择器对类和id选择器等同样适用

**注：父子选择器的优先级通过css权重来进行计算，当权重值一样时，后边的选择器会覆盖前边的**

**浏览器遍历父子元素的顺序为自右向左**

### CSS权重

!important               infinity

行间样式                 1000

id                              100

class|属性|伪类          10

标签|伪元素                 1

通配符                         0

**css权重数字为256进制**

### CSS代码

**注释方式：/\*xxxxxxxxx\*/，css注释为块注释方式**

#### 修饰文本的css属性

**font-size**，设置字体大小，默认为16px，常用为12/14px

**font-weight**，设置字体的形态，lighter、normal、bold、bolder

**font-style**，设置字体加粗、斜体等

**font-family**，设置字体类型，如arial等

**text-align**，文本对齐方式，center、right、left

**line-height**，设置文本的行高，**当单行文本的行高等于容器高度时，文本即为居中显示**

**text-indent**，文本缩进，如首行缩进两个字符即为**text-indent: 2em;**这里的em为相对单位，**1em=1font-size**

**text-decoration**，文本修饰，如del标签可用line-through（中划线）实现，同时也可以利用text-indent: none来去掉del标签中的中划线、

**cursor**，设置鼠标悬停时的鼠标样式，属性值：pointer、help、resize

**color**，设置字体颜色,设置颜色又三种方法：

1、土鳖式（纯英文单词，如red、green）

2、颜色代码（以#开头，如#ff4400，组成为rgb三原色，各占两位，以16进制的方式表示00-ff，即#ff0000为红色，#00ff00为绿色，#0000ff为蓝色，若三原色代码中两两一样，可简写为三位，如#ff4400淘宝红可写为#f40）

3、颜色函数,rgb(0~255, 0~255, 0~255)

**border**，盒子边框，写法：border: xxx xxx xxx，border三个属性分别为border-width、border-style、border-color，也可对每个边进行单独设置，例：

```css
div{
	width: 0;
	height: 0;
	border: 100px solid black;
	border-left-color: #ff4400;
	border-top-color: #00ff00;
	border-bottom-color: #0000ff;
}
```

### 伪类选择器

1、hover，

```css
a{
	background: orange;
}
```

### 小结

html标签分类：

1、**行级/内联元素**，span、strong、a、del、em，默认display: inline

​	feature：内容决定元素所占位置，不可通过css改变宽高，可设置左右margin

2、**块级元素**，div、p、ul、ol、li、form、address，默认display: block

​	feature：独占一行，可通过css改变宽高

3、**行级块元素**，img，默认display: inline-block

​	feature：内容决定大小，可改变宽高

凡是带有inline的元素，都有文字特性（文字间距）

**小技巧：在进行html+css代码编写时，可先在css中定义功能（利用class选择器），然后再去html中对标签选配功能，可大大提升开发效率**

通配符选择器多用来初始化所有标签，如设置默认margin和padding值为0

### 盒子模型

盒子三大部分：盒子壁（border）、内边距（padding）、盒子内容（width×height）

盒子模型的四部分：margin + border + padding + content（width × height）

padding/border/margin后为四个值时，分别为上、右、下、左

padding/border/margin后为三个值时，分别为上、左右、下

padding/border/margin后为两个值时，分别为上下、左右

除此之外还可用padding/border/margin-left/right/top/bottom等进行单独设置

**border-radius可用来设置圆角边框，值为百分比形式，同时也可以为像素，当值为50%时，边框为圆形**

### position

1、**absolute**，绝对定位，将元素定义为可定位的元素，配合**left/right**和**top/bottom**使用，分别为距离左边框的边距和距离上边框的距离，一般情况下不采用bottom进行定位

**注：absolute，相对于最近的有定位的父级进行定位，如果没有，相对于文档进行定位**

2、**relative**，相对定位，保留原来位置进行定位，即原位置保留，并不会被其他元素占用，相对于本该处于的位置进行定位

**注：relative，相对于元素出生位置进行定位**

3、**fixed**，固定定位，即将元素定位到固定位置，不随滚动条的滚动而移位

### 两个经典bug

1、**margin塌陷**：父子嵌套的元素，垂直方向上的margin取两个元素的最大值（即此时子元素在垂直方向上的margin是相对于html页面的，且会带动父元素的位置）

解决方案：bfc（block format context），如何触发bfc（1、position: absolute; 2、display: inline-block; 3、float=left/right; 4、overflow: hidden）使用上述方法均会产生新的问题，因此具体用哪个视情况而定

2、span标签margin不可共用：如两个span排列在一行，左边的标签margin-right: 100px，右边的标签margin-left: 100px，此时两个标签之间的空隙为200px

div标签margin重叠：如两个div标签，第一个标签的margin-bottom: 100px，第二个标签的margin-top: 100px，此时两个div之间的空隙为100px

### float

属性值：right/left

1、浮动元素产生了浮动流，块级元素看不到他们，**产生了bfc的元素和文本类属性的元素和文本**都能看到浮动元素，即浮动元素会跟普通块级元素产生重叠

2、浮动的清除，1) 可通过伪元素来清除浮动，如.wrapper::after{content: ""; display: block; clear: both}

​							2) 可对父级元素开启bfc

注：凡是设置了position:absolute和float=right/left的元素，均为inline-block元素

### 伪元素选择器

在元素创建的同时会相伴着产生两个伪元素，分别为before和after

**伪元素天生为行级元素**

标签名 :: before，

标签名 :: after，通过伪元素选择器可修改伪元素的样式如：

```css
span::before{
    display: inline-block;
    content: "";
    width: 100px;
    height: 100px;
}
```

### 文字溢出处理

#### 单行文本溢出处理

三件套处理单行文本溢出，如\<p>标签，

```css
p{
	white-space: nowrap;
	overflow: hidden;
	text-overflow: ellipsis;
}
```

#### 多行文本溢出处理

一般采用多行截断处理，即overflow: hidden

### 背景图片大小设置

如下：

```css
div{
	width: 200px;
	height: 200px;
	border: 1px solid #ffffff;
	background-image: url(../image/xxx.jpg)
	background-size: 100px 100px;
    background-repeat: no-repeat;
    background-position:center center;
    /*background-position的值为上下位置和左右位置*/
}
```

图片和文字同时表示一个网址时，可用如下方法：

方法一：文字设置缩进为容器的宽度，并设置white: nowrap使文字处于一行，并设置溢出隐藏（overflow: hidden）

```css
a{
    display: inline-block;
    text-decoration: none;
    color: #f40;
    width: 100px;
    height:100px;
    border: 1px solid black;
    background-image: url(图片地址);
    background-size: 100px 100px;
    /*a标签利用图片和文字同时代替的情况可设置文字的样式使文字溢出并隐藏*/
    text-indent: 100px;
    white-space: nowrap;
    overflow: hidden;
}
```

方法二：利用padding-top将文字撑出容器，然后利用overflow: hidden

```css
a{
    display: inline-block;
    text-decoration: none;
    color: #f40;
    width: 100px;
    height:100px;
    border: 1px solid black;
    background-image: url(图片地址);
    background-size: 100px 100px;
    
    padding-top: 100px;
    overflow: hidden;
}
```

### css知识补充

1、嵌套盒子可用margin: 0 auto来实现内部盒子的自适应居中

2、inline和inline-block元素均称为文本类元素，会被文字分割符分割，分割后页面元素跟元素之间会有一定的空隙，该空隙并不需要手动修改，在代码打包压缩过程中，为节省空间，会自动去掉标签之间的分隔符即恢复正常

3、position: absolute和float: left/right这两个属性一旦设置其中一个，元素会在内部被转化为display: inline-block

4、当inline-block元素内部不包含文字时，外部文本元素会跟标签底对齐，当inline-block元素内部包含文字时，外部文本元素会跟标签内部文本底对齐（img标签为inline-block元素，其外部文本与标签底部对齐，当span设置为inline-block时，若其内部存在文字内容，其后紧跟的文本会与span内部的文本底对齐），这个对其可通过vertical-align: ···px来调整文本对齐位置

### 例子

奥运五环

1、利用border-top/bottom/right/left-color=transparent来实现环的一边为透明色

2、利用z-index值控制每个环的上下显示顺序

```html
<div class="wrapper">
    <div class="position">
        <div class="blue color"></div>
        <div class="blue1 color"></div>
        <div class="yellow color"></div>
        <div class="yellow1 color"></div>
        <div class="black color"></div>
        <div class="black1 color"></div>
        <div class="green color"></div>
        <div class="green1 color"></div>
        <div class="red color"></div>
        <div class="red1 color"></div>
    </div>
</div>
```

```css
*{
    margin: 0;
}
.wrapper{
    position: absolute;
    width: 400px;
    height: 250px;
    border: 50px solid;
    border-top-color: #f40;
    border-bottom-color: #0f0;
    border-right-color: yellow;
    border-left-color: #0ff;
    background-color: #cccccc;
    left: 50%;
    top: 50%;
    transform: translateX(-50%) translateY(-50%);
}
.position{
    width: 325px;
    height: 155px;
    position: absolute;
    left: 50%;
    top: 50%;
    /*transform: translateX(-50%) translateY(-50%);*/
    margin-left: -162.5px;
    margin-top: -77.5px;
}
.color{
    position: absolute;
    width: 95px;
    height: 95px;
    border: 5px solid;
    border-radius: 50%;
}
.blue{
    border-color: deepskyblue;
    z-index: 0;
}
.blue1{
    border-color: deepskyblue;
    border-bottom-color: transparent;
    z-index: 10;
}
.yellow{
    left: 55px;
    top: 50px;
    border-color: #ffff00;
    z-index: 1;
}
.yellow1{
    left: 55px;
    top: 50px;
    border-color: #ffff00;
    border-right-color: transparent;
    z-index: 9;
}
.black{
    left: 110px;
    border-color: #000000;
    z-index: 2;
}
.black1{
    left: 110px;
    border-color: #000000;
    border-bottom-color: transparent;
    z-index: 8;
}
.green{
    left: 165px;
    top: 50px;
    border-color: #00ff00;
    z-index: 3;
}
.green1{
    left: 165px;
    top: 50px;
    border-color: #00ff00;
    border-right-color: transparent;
    z-index: 6;
}
.red{
    left: 220px;
    border-color: #ff0000;
    z-index: 4;
}
.red1{
    left: 220px;
    border-color: #ff0000;
    border-right-color: transparent;
    z-index: 5;
}
```

## 冷知识

body默认margin为8px

a标签的使用技巧，在设置a标签的大小前，将其设为display: inline-block，还可用text-decoration来去掉a标签文字的下划线

padding上可以加颜色和背景图片

行级元素只能嵌套行级元素，块级元素可以嵌套任何元素（**特例：p标签中不可嵌套div**）

a标签里不可嵌套a标签