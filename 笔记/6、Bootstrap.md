## introduction

在通过CDN引入bootstrap时，通过以下方式进行引入

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>简介</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl" crossorigin="anonymous">
</head>
<body>

<script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.6.0/dist/umd/popper.min.js" integrity="sha384-KsvD1yqQ1/1+IA7gi3P0tyJcT3vR+NdBTt13hSJ2lnve8agRGXTTyNaBYmCR/Nwi" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/js/bootstrap.bundle.min.js" integrity="sha384-b5kHyXgcpbZJO/tY9Ul7kGkf1S0CWuKcCD38l8YkeH8z8QjE0GmW1gYU5S9FOnJ0" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/js/bootstrap.min.js" integrity="sha384-nsg8ua9HAw1y0W1btsyWgBklPnCUAFLuTMS2G72MMONqmOymq585AcH49TLBQObG" crossorigin="anonymous"></script>
</body>
</html>
```

其中link标签和script标签中的integrity和crossorigin标签分别代表哈希值和跨域请求，在发送请求时会带上所设置的哈希值

meta标签中的设置即css3响应式布局中所提到的

device-width：解决iphone或者ipad上横竖屏的宽度 = 竖屏时候的宽度不能自适应的问题

initial-scale=1.0：解决windows phone ie浏览器上横竖屏的宽度 = 竖屏时候的宽度自适应的问题

该标签还可设置user-scalable属性：是否允许用户缩放页面

## 响应式原理

响应式所具有的特点

1、网页宽度自动调整

2、尽量少用绝对宽度

3、字体要使用rem或em作为单位

4、布局要使用浮动或者弹性布局

### 媒体查询

**媒体查询**：根据一个或多个基于设备类型、具体特点和环境来应用样式

1、媒体类型

2、媒体特性

3、逻辑运算符

参考：https://drafts.csswg.org/mediaqueries/

@规则

@chartset            定义编码

@import              引入css文件（css模块化）

@font-face          自定义字体

@keyframes       animation里的关键帧

@media              媒体查询

#### 媒体类型

all                                     所有设备

print                                 打印机设备

screen                             彩色的电脑屏幕

speech                            听觉设备（针对有视力障碍的人士，可以把页面的内容以语音的方式呈现的设备）

写法：@media screen and(条件)

#### 媒体特性

width   宽度

​    min-width                  最小宽度，宽度只能比这个大

​    max-width                 最大宽度，宽度只能比这个小

height   高度

​    min-height                  最小高度，高度只能比这个大

​    max-height                 最大高度，高度只能比这个小

orientation   方向

​    landscape                横屏（宽度大于高度）

​    portrait                     竖屏（高度大于宽度）

aspect-ratio   宽度比

-webkit-device-pixel-ratio    像素比（wenkit内核的私有属性）

#### 逻辑运算符

and                        合并多个媒体查询（并且的意思）

，                          匹配某个媒体查询（或者的意思）

not                         对媒体查询结果取反

only                      仅在媒体查询匹配成功后应用样式（防范老旧浏览器）

### 栅格系统

默认将整个页面分为12列

#### 前缀分类

超大屏（屏幕宽度≥1200px，.container最大宽度1140px）：**.col-xl-数字**

大屏幕（屏幕宽度≥992px，.container最大宽度960px）：**.col-lg-数字**

中等屏幕（屏幕宽度≥768px，.container最大宽度720px）：**.col-md-数字**

小屏幕（屏幕宽度≥576px，.container最大宽度540px）：**.col-sm-数字**

超小屏幕（屏幕宽度<576px，.container无要求）：**.col-数字**

当屏幕宽度小于额定宽度时，一行只能显示一列

标准写法

```html
<div class="container-fluid">
    <div class="row">
        <div class="col-xl-1">第1列</div>
        <div class="col-xl-1">第2列</div>
        <div class="col-xl-1">第3列</div>
        <div class="col-xl-1">第4列</div>
        <div class="col-xl-1">第5列</div>
        <div class="col-xl-1">第6列</div>
        <div class="col-xl-1">第7列</div>
        <div class="col-xl-1">第8列</div>
        <div class="col-xl-1">第9列</div>
        <div class="col-xl-1">第10列</div>
        <div class="col-xl-1">第11列</div>
        <div class="col-xl-1">第12列</div>
    </div>
</div>
```

#### 设置等宽列

通过.col来实现

如果想在某个元素后进行换行，可通过一个class为w-100(可通过样式设置使其宽高均为0)，能够让后面的列换行

.col还可以实现平分当前行剩余宽度的效果，其会自动计算已设置元素的宽度总和，将剩余宽度进行平分

#### 根据内容调整元素的宽度

使用.col-{breakpoint}-auto，如.col-md-auto，当页面宽度大于额定宽度时，元素的宽度由内容决定

#### 所有尺寸下都占同样列数

.col-数字即可实现，于小屏幕下的设置一样

```html
<div class="row mt-5">
    <div class="col-8"></div>
    <div class="col-4"></div>
</div>
```

混合排列或组合模式

1、超大屏幕下一行显示6个div，一个div占2列

2、大屏幕下一行显示4个div，一个div占3列

3、中等屏幕下一行显示3个div，一个div占4列

4、小屏幕下一行显示2个div，一个div占6列

5、超小屏幕下一行显示1个div，一个div占1列

实现方法：即将所有情况下的class属性均设置到div中

```html
<div class="row mt-5">
    <div class="col-xl-2 col-lg-3 col-md-4 col-sm-6 col-12">超大屏6个，大屏4个，中等屏3个，小屏2个，超小屏1个</div>
    <div class="col-xl-2 col-lg-3 col-md-4 col-sm-6 col-12">超大屏6个，大屏4个，中等屏3个，小屏2个，超小屏1个</div>
    <div class="col-xl-2 col-lg-3 col-md-4 col-sm-6 col-12">超大屏6个，大屏4个，中等屏3个，小屏2个，超小屏1个</div>
    <div class="col-xl-2 col-lg-3 col-md-4 col-sm-6 col-12">超大屏6个，大屏4个，中等屏3个，小屏2个，超小屏1个</div>
    <div class="col-xl-2 col-lg-3 col-md-4 col-sm-6 col-12">超大屏6个，大屏4个，中等屏3个，小屏2个，超小屏1个</div>
    <div class="col-xl-2 col-lg-3 col-md-4 col-sm-6 col-12">超大屏6个，大屏4个，中等屏3个，小屏2个，超小屏1个</div>
</div>
```

#### 对齐

##### 垂直对齐

需配合v-align进行使用

（即flex中的align-items和align-self属性）

1、行的对齐方式（用来设置整行的对齐方式，设置在父元素中）

align-items-start                                  顶对齐

align-items-center                               中间对齐

align-items-end                                   底对齐

2、列的单独对齐方式（用来单独设置元素的对齐方式，设置在子元素中）

align-self-start                                    顶对齐

align-self-center                               中间对齐

align-self-end                                   底对齐

##### 水平对齐

需配合v-align进行使用，均设置在父级元素

（即flex中的justify-content属性）

1、justify-content-start                      左对齐

2、justify-content-center                  中间对齐

3、justify-content-end                      右对齐

4、justify-content-round                   分数居中对齐（两个元素两侧的间距时相等的）

5、justify-content-between              左右两端对齐（元素之间的间距时自动平分的，左右两边的元素仅靠元素壁）

#### 行排序-order

order-{breakpoint}-数字

根据其后边跟的数字大小，对元素的排列顺序进行重排

也可利用order-first和order-last进行排序，分别为排在第一位和排在最后一位

#### 列偏移

offset-{breakpoint}-数字

表示向右偏移几列，该偏移为当前行中所有元素偏移

#### 间距

使用margin工具可以让列之间产生间距

mr-{breakpoint}-auto        使右侧的列远离到最右边

ml-{breakpoint}-auto        使左侧的列远离到最左边

#### 嵌套

每一个列中可以再继续放行，那嵌套里面的元素就会以父级的宽度为基准，再份12个列

## 重置

bootstrap会对页面中的元素样式进行重置，即改变原先的默认样式

如：bootstrap去掉了所有元素的margin-top

## 排版

### 标题

可利用.h1/.h2/.h3/.h4/.h5/.h6属性将文本标签变为标题

.display-1 / .display-2 / .display-3 / .display-4 可将标题变为更大的标题

.text-mutes可设置副标题

### 引言

.lead属性可将文本设置为引言形式

### 内联文本

mark标签可设置标记文本

del标签可设置删除形式的文本

ins标签可设置下划线文本，即插入进去的文本

small标签可设置小号字体的文本

strong标签可设置加粗形式的文本

em标签可设置斜体的文本

.mark 和 .small也可将文本设置为标记的和小号字体的文本

### 缩写

abbr标签主要用作缩写，其有个title属性设置为其全称

```html
<p><abbr title="attribute">attr</abbr></p>
<p><abbr title="HypperText Markup Langauge" class="initialism">HTML</abbr></p> <!-- .initialism可以让字体变的小一点 -->
```

### 引用与署名

blockquote可用作引用，署名可用footer标签来实现，其属性值如下设置

```html
<blockquote class="blockquote">
   <p>时间就像海绵里的水，只要愿挤，总还是有的</p>
   <footer class="blockquote-footer">来自<cite>鲁迅</cite></footer>
</blockquote>
```

### 对齐

可用.text-left / .text-center / .text-right属性来设置文本的对齐方式

### 无特效列表

.list-unstyled可去掉列表的默认样式 ，.list-inline可将列表中的内容设置成一行中显示

```html
<ul class="list-unstyled">
   <li>red</li>
   <li>
      <ul class="list-unstyled">
         <li>blue</li>
      </ul>
   </li>
</ul>
<ul class="list-inline">
   <li class="list-inline-item">red</li>  <!-- 3.x的版本是不需要在li身上添加class -->
   <li class="list-inline-item">blue</li>
   <li class="list-inline-item">green</li>
</ul>
```

### 超出内容省略

通过.text-truncate属性可设置超出内容省略

```html
	<!-- text-truncate可以让超出的内容省略，3.x的版本里使用的是text-overflow -->
<div class="container-fluid">
   <dl class="row">
      <dt class="col-sm-3">矮</dt>
      <dd class="col-sm-9">矮，汉语常用字，读作ǎi，最早见于秦代小篆，其本义是身材短，即《说文解字》：“矮，短人也。”后引申为低、不高的、等级地位低、卑下的等义。（基本信息栏参考资料：）</dd>
      <dt class="col-sm-3">矬</dt>
      <dd class="col-sm-9">说一个人挫，可以是说一个人长的丑，有辱市容市貌，行为猥琐，挑战大众的审美观；也可能是熟人之间开玩笑，说一个人倒霉，或者做事不成样子，具体要看说的人的语气（一般这个意思的“挫”，会是熟人间笑着问“你挫不挫啊？”）。而挫，挫人也有轻蔑谩骂的意思。</dd>
      <dt class="col-sm-3 text-truncate">穷，穷的不要不要的了。穷，穷的不要不要的了</dt>
      <dd class="col-sm-9">穷，汉语常用字，读作qióng或者gōng，最早见于甲骨文，造字本义为身居洞穴，身体被迫弯屈、不自由，后引申为物质上困顿的、不得志的、贫困的，又引申为追究、终结、尽、完等。（基本信息栏参考资料：）</dd>
   </dl>
</div>
```

## 代码

利用code可在页面显示代码

```html
<p>我要在这段文本里写一个<code>&lt;section&gt;</code>的html标签</p>
```

```html
<code>
    <h1>;这是一个标题</h1>
    <p>这是一段文字</p>
    <p>这些代码放在code里，再放到pre标签里，给pre标签来上一个pre-scrollable的class，就会显示成一个340px高的框，超过后就会出现滚动条</p>
</code>
```

### 变量

可用var标签进行实现

### 按钮

可通过kbd标签来实现

```html
<!-- 变量 -->
<var>y</var> = <var>m</var><var>x</var> + <var>b</var>

<!-- 按钮 -->
<p>想要保存的话，请按下<kbd>ctrl</kbd>+<kbd>s</kbd></p>
```

## 图片

### 响应式

.img-fluid属性可以将图片变为响应式图片，会随页面的大小改变尺寸

### 缩略图

通过.img-thumbnail属性可将图片变成缩略图

### 圆角图

通过.rounded属性可将图片变成圆角图片

### 图片对齐方式

通过.float-left和.float-right可实现图片的左右对齐

### 通过父级调整对齐

通过在父级中设置text-letf、text-center、text-right可以设置其内部图片的对齐方式

### 自己居中

首先要将图片变成block，然后使用margin: 0 auto即可设置图片的自己居中

实现方法：通过添加d-block和mx-auto属性即可实现上述效果

### Picture元素

HTML5标准提供了一个全新的`<picture>` 元素，它可以为 `<img>`指定多个`<source>` 定义，可配合媒体查询来完成不同尺寸加载不同的图片

```html
<picture>
   <source media="(min-width:1200px)" srcset="images/1140.jpg">
   <source media="(min-width:992px)" srcset="images/960.jpg">
   <source media="(min-width:768px)" srcset="images/720.jpg">
   <source media="(min-width:576px)" srcset="images/540.jpg">
   <img src="images/img_01.jpg" alt="">   <!-- 当尺寸小于576的时候会显示这个图片 -->
</picture>
```

**注：IE不支持，>=ios9.3 || >android 4.4支持**

## 表格

表格的响应式可通过在其class中添加table来实现，还可通过添加table-.....来实现各种样式

如：table-dark

### 表头

thead-......可用来设置表头的颜色样式

### 隔行换色

通过为tbody设置table-striped属性可实现条纹状表格

### 边框

table-borded可为表格设置边框

### 去除边框

通过table-borderless可以去除表格的边框

### 行悬停效果

通过table-hover可实现行悬停高亮的效果

### 紧缩表格

通过table-sm可实现紧缩表格

### 语义化状态

使用语义状态样式对表格逐行或单个单元格进行着色表达，即设置表格的颜色

如table-active、table-primary等

### 响应式表格

可在具有table属性的标签上添加table-responsive来实现，使其出现滚动条

## 图文框

通过为figure标签添加figure属性，该属性并不会为图片定义明确的大小，因此需要将img-fluid属性添加到img标签中才能实现与响应式的完美配合

在figure中添加figcaption标签即可实现图文框，其属性中需添加figure-caption，其文字的对齐方式通过text-left/center/right来进行设置

## 公共样式

### 边框

border-xxxx详情可查看课件或中文网

可设置边框及颜色等属性

如颜色、圆角（rounded）样式

### 颜色

文本（text-）、边框（border-）、背景（bg-）、背景渐变（bg-gradient-）等

其中背景渐变需下载源文件并修改sass源码

### display属性

d-{breakpoint}-inline/block/inline-block/none/flex等，详情可查看中文官网

常用于在各种尺寸下显示某些元素，不显示某些元素，可通过更改元素的display属性来进行完成，可添加多个属性来完成不同大小屏幕下的隐藏与显示

### 嵌入

将这些规则应用到 `<iframe>`、`<embed>`、 `<video>`、 `<object>` 上，当需要配合其它属性（如响应式）时，也可以加入 `.embed-responsive-item` 定义

你可以将任何代码嵌入到 `<iframe>` 中，并使用 `.embed-responsive` 实现同比例收缩

#### 长宽比

可以通过修改 class样式来实现纵横比定义，如embed-responsive-21by9即长宽比为21：9

### flex

可通过为元素添加d-flex使其变为弹性盒模型，还可通过d-{breakpoint}-flex/inline-flex来实现响应式变化的弹性盒模型

#### 方向

flex-{breakpoint}-row / column / row-reverse / column-reverse来实现弹性盒模型内元素的方向

#### 内容对齐与对准

可通过添加justify-content-{breakpoint}-strat / end / center / round / between属性来实现内容的在垂直方向上的对齐与对准方式

#### 对齐项目

可通过为弹性布局容器添加align-items-{breakpoint}-start / end / center / stretch / baseline属性来实现其在水平方向上的对齐方式以及元素是否拉伸

#### 自对齐

可通过为子元素添加align-self-{breakpoint}-start / end / center / stretch / baseline属性来实现内容在横走方向上的对齐方式

#### 自相等

通过在一系列兄弟元素上使用flex-{breakpoint}-fill来强制他们变成相等的宽度，同时占据所有可用的水平空间

```html
<div class="d-flex bg-dark">
    <div class="p-2 flex-fill bg-primary">Flex item</div>
    <div class="p-2 bg-dark">Flex item</div>
    <div class="p-2 bg-dark">Flex item</div>
</div>//上述代码会将弹性盒子行中多出来的空间分到带有flex-fill属性的标签上
```

#### 等宽变幻

flex-{breakpoint}-grow / shrink-数字，扩展或伸缩，其中数字只能是0或1，数字为0表示不扩展或伸缩，数字为1表示扩展或伸缩

#### 自浮动

mr /  ml-auto将当前元素最右边推到最右边或将元素最左边的元素推到最左边

#### 垂直布局

结合align-items-*、flex-column、mt/mb-auto可实现垂直方向上的布局

mt表示将当前元素顶部的元素推到最顶部，mb表示将当前元素底部的元素推到最底部

#### wrap包裹

通过flex-{breakpoint}-wrap / nowrap / wrap-reverse可实现元素的换行形式，wrap-reverse可对正常换行后的元素进行反转

#### order排序

将元素按照order数字大小进行排序

order-{breakpoint}-*，其中\*表示整数数字

#### 多行对齐

**对单行无效**

通过对弹性盒模型父级元素设置align-content-{breakpoint}-start / end / center / around / stretch属性即可完成多行对齐

### 浮动

#### 设置浮动

可通过为元素添加float-{breakpoint}-left / right / none来实现元素的浮动效果

**注：弹性盒模型中的浮动是无效的，故设置浮动的元素父级不能是弹性盒模型**