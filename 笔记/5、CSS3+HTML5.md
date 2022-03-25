# CSS3

## introduction

预处理器：pre-processor

less/sass  cssNext

后处理器：post-processor

autoprefixer

postCss + 插件（充分体现扩展性，200多个）

用js实现的css的抽象的语法树

AST（Abstract Syntax Tree）

## selector

### 关系型选择器（Relationship Selectors）

E F，父子选择器

E > F，子元素选择器

E + F，下一个满足条件的兄弟元素节点

```html
<div></div>
<p>1</p>
<p>2</p>
<p>3</p>
//div + p即可选择到内容为1的p标签
```

E ~ F，E后面所有满足条件的兄弟元素节点

### 属性选择器（Attribute Selectors）

标签名[属性名~=“xxx”]，属性里包含独立xxx

标签名[属性名|=“xxx”]，选择属性以xxx开头或者以xxx-开头的元素节点

标签名[属性名^=“xxx”]，以xxx开头的元素节点被选中

标签名[属性名$=“xxx”]，以xxx结尾的元素节点被选中

标签名[属性名*=“xxx”]，只要属性中存在xxx即被选中

### 伪元素选择器（Pseudo-Element Selector）

E::placeholder，可以改变input元素节点placeholder字体颜色**（兼容性不太好）**

E::selection，可以改变文字内容选中后的样式，包括color、background-color和text-shadow三个属性

### 伪类选择器（Pseudo-Classes Selector）

E:not(条件)，选择非条件的所有E元素节点

E:root，选择根节点，即html标签，一般情况下直接写:root

E:target，被设为锚点的元素节点在被选中时样式变为选择器中设置好的样式

E:first-child，选择某元素的第一个子元素节点且该节点为E元素节点，其中E为要选择的元素类型

E:last-child，选择某元素的最后一个子元素节点且该节点为E元素节点

E:only-child，选择某元素仅有的一个元素节点且类型为E

E:nth-child(n)，选择某元素下第n个子元素节点，且该节点为E，可用2n或2n+1来代表偶数和奇数或even/odd

==以上几个选择器中的E均指所要选择的目标元素==，如下即为选择div下的奇数span标签

```css
div span:nth-child(2n + 1){
    color: #fd0808;
}
```

***小知识：CSS是从1开始计数***

**E:first-of-type，选择父元素下第一个E元素节点**

**E:last-of-type，选择父元素下最后一个E元素节点**

**E:nth-of-type(n)，选择父元素下第n个E元素节点**

**E:empty，选择内容为空的E元素**

针对input标签

==注：input标签默认`display:inline-block`==

E:checked，选择被选中状态下的类型为checkbox的input标签

E:enable，选择可以操作的input标签

E:disabled，选择不可操作的input标签，如\<input type="text" disabled>

E:read-only，选择只读的input标签，如\<input type="text" readonly value="xxxx">

### 手风琴案例

html部分

```html
<div class="centerBox">
    <span>Accordion List</span>
    <a class="title" href="#text1">content1</a>
    <span class="text" id="text1">text1</span>
    <a class="title" href="#text2">content2</a>
    <span class="text" id="text2">text2</span>
    <a class="title" href="#text3">content3</a>
    <span class="text" id="text3">text3</span>
</div>
```

css部分

```css
.centerBox{
    position: absolute;
    width: 400px;
    height: 800px;
    border: 1px solid #ff0000;
    top: calc(50% - 400px);
    left: calc(50% - 200px);
}
.title{
    color: #fff;
    display: block;
    width: 100%;
    height: 50px;
    line-height: 50px;
    background-color: #6e6e6e;
    text-decoration: none;
}
.text{
    display: none;
}
.text:target{
    line-height: 25px;
    display: block;
}
```

效果如下图

![image-20210430165640036](C:\Users\木豆\AppData\Roaming\Typora\typora-user-images\image-20210430165640036.png)

## border&background

### border-radius

写法：

1、border-radius: xxpx/xx%;

2、border-radius: xxpx xxpx xxpx xxpx;

3、border-top/bottom-left/right-radius: xxpx;

4、border-top/bottom-left/right: xxp  xxpx;

5、border-radius: 10px 20px 30px 40px / 10px 20px 30px 40px;这里的顺序为从左上、右上、右下、左下，按顺时针顺序

### box-shadow

#### 外阴影

box-shadow: npx npx npx npx color

第一个参数为阴影的左右偏移量，值为正时向右侧偏移，值为负时向左侧偏移

第二个参数为阴影的上下偏移量，值为正时向下偏移，值为负时向上偏移

第三个参数为高斯模糊范围大小，模糊为向两侧同时进行模糊

第四个参数为四个边框向外侧的偏移量

最后一个参数为阴影的颜色

#### 内阴影

box-shadow: inset npx npx npx color

第一个参数为左右偏移量，值为正时，阴影从左侧边界向右延伸，值为负时，阴影从右侧边界向左延伸

第二个参数为上下偏移量，值为正时，阴影从上侧边界向下延伸，值为负时，阴影从下侧边界向上延伸

第三个参数为模糊的范围大小

最后一个参数为阴影的颜色

内阴影和外阴影还可以配合使用，用，隔开，还可以为一个元素同时设置多个阴影配合使用

#### 例1

```css
box-shadow:inset 0px 0px 50px #fff,
                inset 10px 0px 80px #f0f,
                inset -10px 0px 80px #0ff,
                inset 10px 0px 130px #f0f,
                inset -10px 0px 130px #0ff,
                0px 0px 50px #fff,
                -10px 0px 80px #f0f,
                10px 0px 80px #0ff;
```

#### 例2

```css
box-shadow: 0px 0px 100px 50px #fff,
			0px 0px 250px 125px #ff0;
```

**注：阴影在背景颜色上层，在内部文字的下层**

#### 例3

```css
body{
    background-color: #000;
}

div{
    position: absolute;
    width: 200px;
    height: 200px;
    left: calc(50% - 100px);
    top: calc(50% - 100px);
    background-color: #00ffff;
    border-radius: 50%;
    transition: all 1s;
    box-shadow: 0px 0px 10px 10px #00ffff;
}

div::after{
    position: absolute;
    left: 0;
    top: 0;
    content: "";
    width: 200px;
    height: 200px;
    border-radius: 50%;
    background-color: transparent;
    transition: all 1s;
}

div:hover::after{
    box-shadow: 0px 0px 50px 30px #0078d7;
}

div:hover{
    background-color: transparent;
    transform: scale(1.5, 1.5);
    box-shadow: inset 10px 0px 50px #3c8dff,
                inset -10px 0px 50px #3c8dff,
                inset 0px 10px 50px #3c8dff,
                inset 0px -10px 50px #3c8dff;
}
```

### border-image

border-image可直接写成border-image: source slice repeat格式

#### border-image-resource

其属性为所选用的边框图片地址

#### border-image-slice

其属性为1-4个值，分别为上、右、下、左四个方向截取线截取的距离，值为纯数字或百分数，不可填px

#### border-image-repeat

用来设置拉伸等效果，属性有stretch、repeat、round和space，可以写入两个参数

stretch（默认值）：直接进行拉伸

repeat：直接进行复制

round：先进行拉伸，拉伸到一定程度（拉伸到元素的一半宽度）会进行复制，其复制同repeat稍有不同

space：同round类似，其拉伸到元素之间距离达到一般宽度时会进行复制，在复制之前使用空白进行填补

#### border-image-outset

用来设置边框图片向外扩张多少像素，值为npx

#### border-image-width

设置border里图片能显示的范围，可以填数字或像素，填数字时，其宽度为数字*边框的宽度

### background-image

属性值可以填写图片的url地址，也可以通过linear-gradient或radial-gradient来生成渐变图片

可配合background-size/background-repeat/background-position进行使用

#### background-origin

属性值有：border-box、padding-box和content-box，默认值为padding-box

其中border-box指背景图片从border开始渲染

padding-box为从padding开始渲染背景图片

content-box为从content部分开始渲染背景图片

设置背景图片从盒子左上角的哪个位置开始，当背景图片设置no-repeat属性后，背景图片覆盖不到的地方就为空

#### background-position

设置图片位置，其位置相对于图片开始位置，即background-origin设置的左上角起始位置

#### background-clip 

属性值有：border-box、padding-box、content-box和text

设置背景图片从哪里开始阶段，如设置padding-box，则padding-box以外不会显示背景图片，一般会配合background-origin进行使用

如果设置属性为text，可以将字体的颜色设置为背景图片，只有内核为webkit的浏览器才可以进行该操作

```css
-webkit-background-clip: text;
background-clip:text;
-webkit-text-fill-color:transparent;
text-fill-color:transparent;
```

#### background-repeat

其属性值有：no-repeat、repeat-x和repeat-y

#### background-attachment

其属性值有：fixed、scroll和local

fixed：背景图像相对于可视区视口固定，当超出容器范围时，超出部分会被隐藏

scroll：背景图像相当于与元素固定，也就是说当元素内容滚动时背景图像不会跟着滚动，因为背景图像总是要跟着元素本身

local：背景图像相对于元素内容固定，也就是说当元素内容滚动时，背景图像也会跟着滚动

#### background-size

有两个属性：cover和contain

cover：在不改变图片比例的情况下用一张图片缩放到完全覆盖容器，背景图像可能超出容器

contain：在不改变图片比例的情况下将图片缩放到图片的宽度或高度与容器的宽度或高度相等，图片处于容器内部

也可直接写入宽高（像素或比例均可）

#### linear-gradient

用于创建一个颜色渐变的图片，默认图片渐变颜色的方向为从上到下，也可以通过角度或者自定义方向来进行改变

写法：

linear-gradient(blue, red);

linear-gradient(45deg, blue, red);

linear-gradient(to left top, blue, red);

linear-gradient(0def, blue, npx/n%, green, npx/n%, red......)

#### radial-gradient

与linear-gradient类似，同样用于创建一个颜色渐变的图片，其结果为放射性的效果

参数有shape（circle/ellipsis）、截止位置（closet-corner/closet-side/farthest-cornor/farthest-side）、圆心位置（at 方向/npx 方向/npx）、颜色

## color

### HSL

H（Hue色调）0（或360）表示红色，120表示绿色，240表示蓝色，也可用其它数值来指定颜色，取值为：0-360

S（Saturation饱和度），取值为：0.0%-100.0%

L（Lightness亮度），取值为0.0%-100.0%

### HSLA

H（Hue色调）0（或360）表示红色，120表示绿色，240表示蓝色，也可用其它数值来指定颜色，取值为：0-360

S（Saturation饱和度），取值为：0.0%-100.0%

L（Lightness亮度），取值为0.0%-100.0%

A（Alpha透明度），取值0-1之间

### transparent

透明色

## text

### text-shadow

属性值：x y blur color，前三个值均以像素进行表示，也可利用多个阴影来加强效果

当采用background-clip: text配合text-fill-color: transparent设置文字背景后，文字也会变成背景的一部分，会造成设置text-shadow后，阴影位于文字表面

### white-space

指定元素是否保留文本间的空格、换行；指定文本超出边界时是否换行

属性值有：normal、pre、nowrap、pre-wrap和pre-line

normal：默认处理方式，会将序列的空格合并为一个，内部是否换行由换行规则决定

pre：原封不动的保留你输入时的状态，空格、换行都会保留，并且当文字超出边界时不换行。等同pre元素效果

nowrap：与normal值一致，不同的是会强制所有文本在同一行内显示

pre-wrap：与pre值一致，不同的是文字超出边界时将自动换行

pre-line：与normal值一致，但是会保留文本输入时的换行

### word-break

定义元素内容文本的字间与字符间的换行行为

其属性有：normal、keep-all、break-all和break-word

break-word：会尽可能保证一个单词的完整性

keep-all：不允许在一个单词间进行换行

break-all：可随意换行

## Multi-column

### columns

属性值为：<列宽> <列数>

### column-width

通过列宽来实现多列

该值不准确，如设置column-width为200px，而当前容器宽度所能存放的列不为整数列，此时列宽就会不准确，即不为所设置的200px

### column-count

直接定义多列

### column-gap

设置列之间的间隙

### column-span

设置或检索对象元素是否横跨所有列

其属性值有：all和none，默认为none

## box

### box-sizing

可实现IE6混杂模式下的盒模型

属性值：content-box（默认值）和border-box

当将其设为border-box时，盒子整体宽度为width所设置的宽度

用处：可用于元素总宽度固定，但border或padding会发生变化的情况，此时可利用box-sizing: border-box来限制盒子的总宽度

### overflow

用来设置盒子内容溢出后的处理方案

其属性值有：visible、hidden、scroll、auto和clip（略）

visible：对溢出内容不做处理，内容可能会超出容器

hidden：隐藏溢出内容且不出现滚动条

scroll：隐藏溢出内容，溢出的内容可以同过滚动呈现

auto：当内容没有溢出容器时不出现滚动条，当内同溢出容器时出现滚动条，按需出现滚动条

**当overflow-x或overflow-y任意设置一个值时，另一个值会从默认值改为auto**

### resize

可以使box变为大小可更改的状态，配合overflow: hidden

其属性值有：none、both、vertical和horizontal

### flex（属性）

弹性盒子

#### flex-direction（方向）

设置主轴方向

其属性值有：row、row-reverse、column和column-reverse

#### flex-wrap（换行）

设置换行

其属性值有：nowrap（不换行）、wrap（换行）和wrap-reverse（反过来换行）

#### justify-content（主轴对齐）

弹性盒子内元素的对齐方式

其属性值有：flex-start（盒子内元素从主轴起始处开始排列）、flex-end（盒子内元素从主轴结束处开始排列）、center（盒子内元素位于主轴中间）、space-between（每个盒子在主轴上的间距相同，两侧元素靠在盒子壁上）和space-around（每个元素在主轴上的margin相同，即两侧元素距离盒子壁的距离为两个盒子之间距离的一半）

#### align-items（侧轴单行对齐）

其属性值有：flex-start（侧轴开始处）、flex-end（侧轴结束处）、center（居中）、baseline（基于子元素基线进行对齐）、stretch（当盒子内元素未设置高度时，会在侧轴上进行拉伸，使其与父元素在侧轴上的宽度或高度一致）

注：可利用justify-content: center和align-items: center配合实现盒子内元素居中

#### align-content（多行对齐）

与align-items和justify-content类似，区别为适用于多行的弹性盒模型容器，即基于行的对齐方式

其属性值有：flex-start、flex-end、center、space-between、space-around和stretch

#### order

用来对弹性盒子内部子元素排列顺序进行定义，若未设置order值，则默认为0

#### align-self

用来设置弹性盒子单个子元素在侧轴（即y轴）上的对齐方式，其设置的优先级要大于父级赋予的align-items，小于父级元素赋予的align-content

其属性值有：auto、flex-start、flex-end、center、baseline和stretch

#### flex-grow

利用数值按照比例将主轴上的剩余空间分配给每个元素，其属性值为数字

#### flex-basis

设置或检索弹性盒子子元素伸缩基准值，在设置flex-basis属性之后，若内部元素所占尺寸大于元素尺寸，元素尺寸会随其内部内容的多少被撑开，前提是内容区不换行

当width和basis同时设置且basis值小于width值时，width作为元素的最大宽度，而basis作为元素的最小宽度

#### flex-shrink

当一行子元素的总尺寸大于盒子尺寸时，按照设置的比例将多出的尺寸分配到每个元素上进行缩减使得元素在盒子内可以放得下，默认值为1

例：一个弹性盒子（宽度为600px）内有三个元素，第一个元素的宽度为200px、第二个元素的宽度也为200px、第三个元素的宽度为400px，且前两个元素的flex-shrink值为1，第三个元素的flex-shrink值为3，其多出宽度的计算方式为200 * 1 + 200 * 1 + 400 * 3 = 1600px，前两个元素的缩减值为((200px * 1) / 1600) * 200px = 25px，第三个元素的缩减值为(400px * 3) / 1600 * 200px = 150px，即自己的宽度（**content区域宽度**）×加权值占加权总宽度的比例×其多出来的宽度即为其应该缩减的值。

#### flex

复合属性，设置或检索弹性盒模型对象的子元素如何分配空间

其属性值为none或flex-grow flex-shrink flex-basis，当设为none时，默认为0 0 auto，flex-grow的默认值为1，flex-shrink的默认值为1，flex-basis的默认值为0%

#### 例1

可动态增加的导航栏

```css
*{
    margin: 0;
    padding: 0;
}

.wrapper{
    width: 600px;
    height: 600px;
    border: 1px solid black;

    display: flex;
}

.content{
    width: 200px;
    height: 30px;
    font-size: 20px;
    line-height: 30px;
    text-align: center;
    color: #f20;
    border-radius: 5px;
    box-sizing: border-box;
}

.content:hover{
    background-color: #f20;
    color: #fff;
}
```

#### 例2

四等分布局

display: flex结合flex: 1 1 auto

#### 例3

flex实现圣杯布局：上中下三栏的布局应为沿侧轴展开（flex-direction: column），上下两栏各占一定比例，中间设为auto，在中间的三栏中，左右两栏各占一样的比例，中间的宽度设置为auto

html部分

```html
<div class="wrapper">
    <div class="header"></div>
    <div class="contain">
        <div class="center"></div>
        <div class="left"></div>
        <div class="right"></div>
    </div>
    <div class="footer"></div>
</div>
```

css部分

```css
*{
    margin: 0;
    padding: 0;
}
.header, .footer{
    width: 100%;
    height: 150px;
    flex: 0 0 auto;
    background: #6b9eee;
}
.wrapper{
    display: flex;
    flex-direction: column;
}
.contain{
    width: 100%;
    height: 100%;
    flex: 1 1 auto;
    display: flex;
}
.left, .right{
    flex: 0 0 auto;
    width: 200px;
    height: 100%;
    background: #0c7b93;
}
.left{
    order: -1;
}
.center{
    flex: 1 1 auto;
    height: 100%;
    width: 100%;
}
```

## 动画

### transition

过渡动画

其参数有：

**transition-property（选择要监听的属性）**

**transition-duration（指定对象过度的时间长度）**

**transition-timing-function（运动类型）**：linear（线性过渡）、ease（平滑过渡）、ease-in（由慢到快）、ease-out（由快到慢）、ease-in-out（由慢到快再到慢）、step-start（等同于steps(1, start)）、step-end（等同于steps(1, end)）、steps（接受两个参数的步进函数，第一个参数必须为正整数，指定函数的部署。第二个参数的取值为start或end，指定每一步发生变化的时间点，第二个参数是可选的，默认值为end）、cubic-bezier（指定的贝塞尔曲线类型，有四个参数，均为数值，可以判断为两个坐标，x坐标的数值须在[0, 1]区间内，y坐标的数值可以出现小于零或者大于1的情况）

**transition-delay（设置延迟过渡的时间，即多少秒之后才开始过渡）**

### animation

配合@keyframes进行使用，keyframes用来定义动画的运动过程，其内部以百分比的形式进行区分不同时刻要变化的状态，也可同时为一个元素添加多个动画来实现不同属性的变化

属性包括：

animation-name：检索或设置对象所应用的动画名称

animation-duration：检索或设置对象动画的持续时间

animation-timing-function：检索或设置对象动画的过渡类型，其属性值同transition-timing-function的属性类型一致

animation-delay：检索或设置对象动画延迟的时间，属性值为秒数

animation-iteration-count：检索或设置对象动画的循环次数，属性值为数字或infinite

animation-direction：检索或设置对象动画在循环中是否反向运动，属性值包括normal、reverse、alternate（动画先正常运行再反方向运行，并持续交替运行）、alternate-reverse（动画先反方向运动再正常运行，并持续交替运行）

animation-fill-mode：检索或设置对象动画时间之外的状态，其属性只有none、forwards（设置对象状态为动画结束后即100%时的状态）、backwords（设置对象状态为动画运动前即0%时的状态）和both（设置对象状态为运动开始前或运动结束后的状态）

animation-play-state：检索或设置对象动画的状态，其属性值有running和paused，可以设置在hover中，该属性的兼容性不是很好

如：这里的0%可以写为from，100%可以写为100%

```css
@keyframes run{
    0%{
        left: 0;
        top: 0;
    }
    100%{
        left: 100px;
        top: 100px;
    }
}
div{
    position: absolute;
    width: 100px;
    height: 100px;
    animation: run 4s;
}
```

### 例题

模拟白天黑夜

html部分

```html
<div class="space"></div>

<div class="sun"></div>

<div class="moon"></div>
```

css部分

```css
*{
    margin: 0;
    padding: 0;
}

body{
    background-color: #000;
}

@keyframes space-change {
    0%{
        opacity: .3;
    }
    25%{
        opacity: 1;
    }
    50%{
        opacity: .3;
    }
    75%{
        opacity: .1;
    }
    100%{
        opacity: .3;
    }
}

@keyframes sunrise {
    0%{
        opacity: 0;
    }
    10%{
        opacity: 1;
        transform: scale(.7, .7) translateX(0) translateY(0);
    }
    30%{
        opacity: 1;
        transform: scale(.5, .5) translateX(0) translateY(-500px);
    }
    50%{
        opacity: 0;
        transform: scale(.7, .7) translateX(800px) translateY(0px);

    }
    100%{
        opacity: 0;
    }
}

@keyframes moonrise {
    0%{
        opacity: 0;
    }
    30%{
        opacity: 0;
        transform: translateY(0);
    }
    50%{
        opacity: 0;
    }
    70%{
        transform: translateY(-300px);
        opacity: 1;
    }
    80%{
        transform: translateY(-300px);
        opacity: 1;
    }
    100%{
        transform: translateY(0);
        opacity: 0;
    }
}

.space{
    height: 500px;
    background-image: linear-gradient(to bottom, rgba(0, 130, 255, 1), rgba(255, 255, 255, 1));
    animation: space-change 20s cubic-bezier(.5, 0, 1, 1) infinite;

}

.sun{
    position: absolute;
    left: calc(50% - 25px);
    top: calc(50% - 25px);
    width: 50px;
    height: 50px;
    background-color: #fff;
    border-radius: 50%;
    transform: scale(.5, .5);
    box-shadow: 0px 0px 100px 50px #fff,
                0px 0px 250px 125px #ff0;
    animation: sunrise 20s infinite;
}

.moon{
    position: absolute;
    left: calc(50% + 400px);
    top: calc(50% - 25px);
    width: 50px;
    height: 50px;
    background-color: #000;
    border-radius: 50%;
    box-shadow: 0px 0px 16px #fff,
                inset 8px 0px 10px #fff,
                inset 8px 0px 10px #fff,
                inset 8px 0px 10px #fff,
                inset 8px 0px 10px #fff;
    animation: moonrise 20s cubic-bezier(0, 0, .5, .5) infinite;
}
```

### step

一种timing-function，即过渡方式

写法：steps(步数, end/start)，第一个参数用来设置过渡过程中两个状态之间要经历的步骤，第二个参数默认为end，start为保留下一帧的状态，end为保留上一帧的状态，end可配合forwards来弥补视觉上缺失运动最后一帧的状态

### 打字效果

html部分

```html
<div>abcdefghijkl</div>
```

css部分

```css
*{
    margin: 0;
    height: 0;
}

@keyframes cursor {
    0%{
        border-left-color: rgba(0, 0, 0, 0);
    }
    50%{
        border-left-color: rgba(0, 0, 0, 1);
    }
    100%{
        border-left-color: rgba(0, 0, 0, 0);
    }
}
@keyframes cover {
    0%{
        left: 0;
    }
    100%{
        left: 100%;
    }
}

div{
    position: relative;
    display: inline-block;
    height: 100px;
    line-height: 100px;
    font-size: 80px;
    font-family: monospace;
}

div::after{
    content: "";
    position: absolute;
    left: 0;
    top: 10px;
    height: 90px;
    width: 100%;
    background-color: #fff;
    box-sizing: border-box;
    border-left: 2px solid black;
    animation: cursor 1s steps(1, end) infinite, cover 12s steps(12, end);
}
```

### 跑马效果

html部分

```html
<div></div>
```

css部分

```css
*{
    margin: 0;
    padding: 0;
}

@keyframes run {
    from{
        background-position: 0;
    }
    100%{
        background-position: -2400px 0;
    }
}

div{
    background-color: #fff;
    top: calc(50% - 50px);
    left: calc(50% - 100px);
    width: 200px;
    height: 100px;
    border: 1px solid #000;
    background-image: url(horse.png);
    background-repeat:no-repeat;
    background-position: 0 0;
    animation: run 1s steps(12, end) forwards infinite;
}
```

### transform

#### rotate

旋转

参数为旋转的度数，如transform: rotate(5deg)

配合transform-origin使用，即旋转中心，

#### rotateX

绕X轴旋转

#### rotateY

绕Y轴旋转

#### rotateZ

绕Z轴旋转，XYZ可配合使用，其为3D效果，要搭配perspective进行使用

#### rotate3d

可以通过比例来自定义旋转轴，其参数包括x方向上的比例，y方向上的比例，y方向上的比例和旋转角度，如rotate3d(1, 2, 0, 0deg)

注：动画均可以设置旋转方向和过渡效果（即animation-direction和animation-timing-function的属性值）

#### scale

作用：伸缩元素变化**坐标轴的刻度**，即伸缩后元素对应的单个刻度大小为原先刻度大小的倍数，如：设置元素scale(2, 2)，在使用translateX(100px)时，其实际移动距离为200px

其参数有两个，分别为x方向上的转换值和y方向上的转换值，值大于1时为放大，值小于1时为缩小

**注意：1、scale伸缩元素变化坐标轴的刻度**

**2、scale会发生叠加操作，即在同一个元素上进行多次scale操作时，后边的操作会依照前边操作之后的比例进行缩放**

**3、雁过留声：scale伸缩过的方向，会一直保留下去，如先进行scale(2, 1)，然后进行rotate(-45deg)，会发现元素在旋转过程中还会在水平方向上发生形变，即旋转过后的方向上收到scale影响，但在旋转前的方向上伸缩后的效果依然存在**

scale也可分为scaleX、scaleY和scaleZ

#### skew

对坐标轴进行倾斜，且倾斜的坐标轴刻度被拉伸，这里的skewX改变垂直方向上的轴，skewY改变水平方向上的坐标轴

写法：skew(旋转度数, 旋转度数)、skewX、skewY

#### translate

translateX、translateY，在X或Y方向上进行平移，translateZ，在Z轴方向上进行平移

当元素尺寸不确定时，可通过left/top配合translateX/translateY进行居中

如：left: 50%; translateX(-50%)即可使元素在X方向上居中

#### perspective

景深：指定观察者与平面的距离，使具有三维位置变换的元素产生透视效果，配合perspective-origin进行使用，当translateZ为负值时，perspective值越小，则观察到元素的大小就越小

**注：景深应设置到父级元素上**

#### perspective-origin

指定透视点的位置，默认值为50% 50%，效果等同于center center

其值可以为百分数或像素值，也可为方向（left等）

#### transform-style

其属性值有：flat和preserve-3d，默认为flat，用于设置元素为平面还是3d

**注：该属性只对自己的子元素负责**

#### transform-origin

设置或检索对象以某个原点进行旋转，默认值为50% 50%，即center center

其值可以为百分数或像素值，也可为方向（left等），也可写入三个值来设定空间运动旋转中心

**注：当元素设置perspective或transform-style: preserve-3d属性后，该元素就变成参照物定位元素，此时需要通过继承来为元素赋予高度**

**注：在设置animation时，如需增添transform效果，要注意元素本身是否包含transform属性，若包含，只能在原transform后增添效果，否则原效果将会被覆盖**

#### backface-visibility

其属性值有：visible和hidden

指定元素背面面向用户时是否可见

#### matrix

元素的transform动作均可以通过matrix来实现

## 性能优化

### 浏览器渲染顺序

download html

download css                                               download js

css rules tree(construct)                              domAPI

​																	domTree

​				cssrulestree                                 cssomAPI

​                                                                    cssomTree

​                   domtree                cssomTree

​                                  renderTree

​                                        |

​                                        |

​                                  layout       --------- >     paint

​	      	（reflow）重构							（repaint）重排

​								  逻辑图（多层矢量化） ----->  

​								  实际绘制（栅格化）

### 触发reflow

改变窗口大小、改变文字大小、激活伪类，如：hover、操作class属性、脚本操作DOM、计算offsetWidth和offsetHeight、设置style属性均会产生页面重构，应少触发这些问题来减少页面性能的消耗

### 触发repaint

如果只是改变某个元素的背景色、文字颜色、边框颜色，则不影响它周围或内部布局的属性

repaint的速度要快于reflow

### 指定触发GPU

1、改变opacity

2、transform: translate3d(0 0 0)  translatez(0)

translatez可以触发GPU层，因此在使用transform时，可在最后加上translatez(0)来主动触发GPU进行渲染

**3、标准方法：will-change: transform**，不要将will-change属性用到太多元素上，该属性的启用最好用在元素要变化的前一刻，不要过早的应用will-change优化

## 成像原理

像素说到底，是一个相对单位

物理像素 === 设备出厂时，像素的大小

dpi表示每英寸所能容纳的像素点数，1英寸 = 2.54cm

即一像素的大小为2.54cm/dpi值cm

dpi = ppi

dpi最早的概念为打印机在一英寸的屏幕里面可以打印多少个墨点

ppi可理解为一英寸所能容纳的像素点数（即点距数）

### 参照像素

96dpi 一臂之遥的视角去看，显示出的具体大小为标杆1/96 * 英寸

即当屏幕为96dpi时，显示过程中的像素大小以1 ：1的比例进行显示

当屏幕为200dpi时，显示过程总的像素大小以大约2 ：1的比例进行显示

也就是按照参考像素的比例进行像素大小的转换，从而达到在每个设备上显示出来的效果尺寸是一致的

css像素 = 逻辑像素

**逻辑像素的概念**：比如在css里设置像素为100px，而使用的设备显示器为200dpi，则在显示过程中，会按照设备像素比，用2设备物理像素去表示1逻辑像素（css像素）

逻辑像素的目的是为了使同样的元素在不同设备上显示的尺寸大小一致

### 设备像素比

dpr全称设备像素比。dpr = 物理像素/css像素，如200dpi / 96dpi，则dpr ≈ 2，即在展示过程中200dpi的设备要使用2个像素去展示css像素里的一个像素

实际展示过程中，元素的尺寸大小按照dpr值来决定使用物理像素的数量

iphone6的dpr为2

我们也管css编程的逻辑像素方式，叫做逻辑屏幕

## 响应式网页开发

概念：响应式网页设计或称自适应网页设计或称回应式网页设计/对应式网页设计，是一种网页设计的技术做法，该设计可使网站在不同的设备（从桌面计算机显示器到移动电话或其他移动产品设备）上浏览时对应不同分辨率皆有适合的呈现，减少用户进行缩放，平移和滚动等操作行为

真正的响应式设计方法不仅仅是根据可视区域大小而改变的网页布局，而是要从整体上颠覆当前网页的设计方法，是针对任意设备的网页内容进行完美布局的一种显示机制

即用一套代码解决几乎所有设备的页面展示问题，设计工作由产品经理或美工来出

实现方法：采用mate标签来进行完成，将页面大小根据分辨率不同进行相应的调节，以展示给用户的大小感觉上差不多

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

device-width：解决iphone或者ipad上横竖屏的宽度 = 竖屏时候的宽度不能自适应的问题

initial-scale=1.0：解决windows phone ie浏览器上横竖屏的宽度 = 竖屏时候的宽度自适应的问题

该标签还可设置user-scalable属性：是否允许用户缩放页面

maximum-scale：最大缩放倍数，其属性值用数值表示

minimum-scale：最小缩放倍数

### 响应式网页开发方法

1、流体网格：可伸缩的网格（大小宽高都是可伸缩（可用flex或者百分比来控制大小）float）   ---->布局上面元素大小不固定可伸缩

2、弹性图片：图片宽高不固定（可设置min-width: 100%）

3、媒体查询：让网页在不同的终端上面展示效果相同（用户体验相同 -> 让用户用着更爽）在不同的设备（大小不同、分辨率不同）上面均展示合适的页面

4、主要断点：设备宽度的临界点

响应式网页开发主要在css上进行操作

#### 媒体查询

媒体查询是向不同设备提供不同样式的一种方式，它为每种类型的用户提供了最佳的体验

css2：media type

media type（媒体类型）是css2中的一个非常有用的属性，通过media type我们可以对不同的设备指定特定的样式，从而实现更丰富的界面

css3：media query

media query是css3对media type的增强，事实上我们可以将media query看成是media type+css属性（媒体特性Media features）判断

**引入方式**

1、外部引入：在link标签中用media属性引入，其属性值为媒体类型和媒体特性，用and连接

```html
<link rel="stylesheet" media="screen and (max-width: 375px)" href="index.css">
```

2、行间样式：在style标签中用@import引入，同样为媒体类型和媒体特性，用and连接

```html
<style>
	@import 'index.css' screen and (max-width: 375px);
</style>
```

3、css样式引入

```html
<style>
    @media(max-width: 375px){
        html, body{
            width: 100%;
            height: 100%;
            background: red;
        }
    }
</style>
```

**注：媒体查询不占用权重**

还可通过逗号来连接多个条件，其产生的效果为“或”，即满足其中一个条件即产生作用

```html
<style>
    @media screen and (max-width: 375px), (min-width: 600px){
        xxxxxxxxxxxxxxxx
    }
</style>
```

否定：用not实现

```html
<style>
    @media not screen and (max-width: 375px), (min-width: 600px){
        xxxxxxxxxxxxxxxx
    }
</style>
```

### 单位值

**rem**：rem是css3新增的一个相对单位（root em，根em）**相对的只是HTML根元素**

例：根标签的font-size值设置为14px，此时1rem的值即为14px

**em**：em是相对长度单位，相对于当前对象内文本的字体尺寸，如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸，相对于本身的font-size大小

**vw / vh**：相对于视口的宽度，视口被均分为100单位的vw / vh

**vmax / vmin**：取视口宽高中最大/最小的一边分为100份

# HTML5

## 属性篇

### input新增属性

#### placeholder

用于输入框中的提示信息

```html
<input type="text" placeholder="请输入密码">
```

### input新增type

#### date

日期类型

```html
<input type="date">
```

兼容性较差

#### time

时间类型

```html
<input type="time">
```

#### week

#### datetime-local

以上时间类型的插件目前均不太常用

#### number

数字输入框，兼容性不好

```html
<input type="number">
```

#### email

邮件输入框，当输入不完整便提交时，会提示输入完整

#### color

颜色选择标签，兼容性不好

#### range

通过滚动条取值

#### search

在输入过程中会将先前提交过的内容当成下拉提示，兼容性不好

#### url

会自动检测输入框中输入内容规则，看是否为标准网址，兼容性不好

### contenteditable

可将容器变成可编辑状态，没有兼容性问题

```html
<div contenteditable="true"></div>
```

### draggable

将元素变为可拖拽的情况，所有的标签元素，当拖拽周期结束后，默认事件时回到原处，兼容性不好，只有chrome和safari可正常使用

```html
<div draggable="true"></div>
```

a标签和img标签默认draggable属性值为true

拖拽的生命周期：拖拽开始、拖拽进行中、拖拽结束

拖拽的组成：被拖拽的物体，目标区域

被拖拽物体的三个事件：ondragStart、ondrag、ondragend，可以用来监听拖拽开始、过程和结束

目标元素的事件：ondragenter（当鼠标拖拽一个元素进入目标区域时，会触发这个事件，这里的进入指的是拖拽物体的鼠标进入目标区域才触发事件）、ondragover（当拖拽物体的鼠标进入目标区域后，便会触发事件，进入后，鼠标移动时该事件便会不停触发）、ondrop，停止拖拽时触发

事件是由行为触发的，但是，一个行为可以不止触发一个事件，在拖拽元素结束后，元素默认事件ondragover回到原处，需先利用e.preverntDefault来阻止默认事件（ondragover）的触发，才可触发ondrop事件

### 拖拽练习 

样式

```html
<style>
    *{
        margin: 0;
        padding: 0;
    }
    .box1{
        position: absolute;
        width: 150px;
        height: auto;
        border: 1px solid;
        padding-bottom: 10px;
    }
    .box2{
        position: absolute;
        left: 300px;
        width: 150px;
        height: auto;
        border: 1px solid;
        padding-bottom: 10px;
    }
    li{
        position: relative;
        width: 100px;
        height: 30px;
        background-color: #abcdef;
        margin: 10px auto 0px auto;
        list-style: none;
    }
</style>
```

```html
<div class="box1">
    <ul>
        <li></li>
        <li></li>
        <li></li>
    </ul>
</div>
<div class="box2"></div>
<script>
    var dragDom;
    var liList = document.getElementsByTagName('li');
    for(var i = 0; i < liList.length; i ++){
        liList[i].setAttribute("draggable", true);
        liList[i].ondragstart = function(e){
            dragDom = e.target;
        }
    }

    var box1 = document.getElementsByClassName('box1')[0];
    var box2 = document.getElementsByClassName('box2')[0];
    box2.ondragover = function(e){
        e.preventDefault();
    }
    box2.ondrop = function(e){
        this.append(dragDom);
        dragDom = null;
    }
    box1.ondragover = function(e){
        e.preventDefault();
    }
    box1.ondrop = function(e){
        this.append(dragDom);
        dragDom = null;
    }
</script>
```

## 标签篇

### 语义化标签

header一般用作页面顶部区域

footer一般用于页面底部区域

nav一般用作导航栏

article、section一般用于分块的页面

aside一般用作侧边栏

以上标签实质上均为div

### canvas

在canvas标签中设置宽高相当于拖动了画布的边框从而使画布变大，而在style中为canvas设置宽高则相当于点击放大镜对图像进行放大，使得里面的内容也会发生变化，因此一般设置canvas标签的宽高通过行间样式来进行设置

#### 画线

canvas的使用通过js完成，先获取到canvas标签，然后才可进行绘制

```js
var ctx = canvas.getContext('2d');//获取画笔

ctx.beginPath();
ctx.moveTo(100, 100);//将画笔移到指定位置
ctx.lineTo(200, 100);//从起始点到指定点进行画线
ctx.lineTo(200, 200);
ctx.closePath();//使所画图形闭合(闭合针对某一笔进行闭合)
ctx.stroke();//根据前面设置的路径在画布上绘制

ctx.beginPath();//停止前面的绘制，重新进行绘制第二笔
ctx.lineWidth = 5;
ctx.moveTo(200, 200);
ctx.lineTo(300, 100);
ctx.stroke();
ctx.fill();//对闭合部分进行填充
```

#### beginPath

ctx.beginPath()，结束当前笔画，开始下一此绘图

#### lineWidth

可设置绘制时的线宽，其属性值为数字

#### 画矩形

1、可以通过画线来进行绘制

2、ctx.rect(100, 100, 200, 100) / ctx.strokeRect(100, 100, 200, 100)其参数为起点x坐标、起点y坐标、长和宽

3、ctx.fillRect(100, 100, 200, 100)，可直接绘制填充后的矩形

#### 清除矩形

ctx.clearRect(0, 0, 500, 500)，括号中参数为设置要清除的范围坐标

#### 画圈

ctx.arc(x, y, 半径, 弧度(起始弧度，结束弧度), 方向(顺时针，逆时针))

上述参数中，以Math.PI × 数字来表示弧度，以0 / 1来表示方向，0为顺时针，1为逆时针

#### 画圆角矩形

arcTo()，其参数为圆角所在点坐标、第二条直线方向坐标、圆角的大小

```javascript
var canvas = document.getElementsByTagName('canvas')[0];
var ctx = canvas.getContext('2d');

ctx.beginPath();
ctx.moveTo(100, 100);
ctx.arcTo(110, 200, 200, 200, 10);
ctx.arcTo(200, 200, 200, 100, 10);
ctx.arcTo(200, 100, 100, 100, 10);
ctx.arcTo(100, 100, 100, 200, 10);
ctx.stroke();
```

#### 画贝塞尔曲线

其原理为通过n条直线（n+1个点）来进行

二次：quadraticCurveTo(中间点坐标，结束点坐标)

三次：bezierCurveTo(第二点坐标、第三点坐标、最后一点坐标)

#### 坐标平移旋转与缩放

ctx.rotate(旋转的度数)，旋转方向为顺时针旋转，度数用Math.PI * 数字来表示，其旋转的目标为坐标系

ctx.translate(坐标)，可改变坐标的位置，其参数为改变后的坐标原点的坐标

ctx.scale(2， 2)，对坐标系进行缩放，参数分别为x轴和y轴方向上缩放的倍数

#### save和restore

rotate和scale的作用范围均为全局的

因此需要通过save来保存当前使用的坐标系，并结合restore在某处恢复保存过的坐标系，这里的操作针对坐标系的平移、旋转和缩放

#### 背景填充

ctx.fillStyle可以改变填充的颜色，这里的填充可改为图片

```javascript
var canvas = document.getElementsByTagName('canvas')[0];
var ctx = canvas.getContext('2d');

var img = new Image();
img.src = "./xxx.png"
img.onload = function(){
    ctx.beginPath();
    ctx.translate(100, 100);
    var bg = ctx.createPattern(img, "no-repeat");
    ctx.fillStyle = bg;
    ctx.fillRect(0, 0, 200, 100);
}
```

**注：填充图案是根据坐标系原点进行填充的，因此需要配合translate进行使用**

#### 线性渐变

ctx.createLinearGradient(过渡的起始方向坐标、过渡的终点方向坐标)，配合bg.addColorStop进行使用

```javascript
ctx.createLinearGradient(0, 0, 200, 0)；
bg.addColorStop(0, "white");
bg.addColorStop(1, "black");
ctx.fillStyle = bg;
ctx.fillRect(0, 0, 200, 100)
```

**注：线性渐变的起点依然是坐标系原点**

#### 辐射渐变

ctx.createRadialGradient(x1, y1, r1, x2, y2, r2)

#### 阴影

ctx.shadowColor = "颜色"

ctx.shadowBlur = 数值

ctx.shadowoffsetX = 数值

ctx.shadowoffsetY = 数值

分别为阴影的颜色、模糊的宽度、阴影在X方向上的偏移和Y方向上的偏移

#### 渲染文字

ctx.strokeText(”xxxxxxx“, x, y)，ctx.fillText("xxxxxx", x, y)，参数分别为内容、坐标，fillText渲染的文字可通过fillStyle来设置颜色，stroke绘制的文字为空心的字体

#### 线端样式

ctx.lineCap = "xxx"，设置线的两端样式，butt、square和round

ctx.lineJone = "xxx"，设置线与线连接处的样式，样式有round、bevel和miter（配合miterLimit进行使用）

ctx.miterLimit = 数值，设置线与线交汇处的长度限制，该属性的使用需使用lineJoin属性设置样式为miter

### svg

SVG：矢量图，放大不会失真，适合大面积的贴图，通常动画较少或者较简单，使用标签和css去画

Canvas：适合用于小面积的绘图，适合动画

#### 线和矩形

line标签和rect标签

```html
<style>
    .line1{
        stroke: black;
        stroke-width: 3px;
    }
    .line2{
        stroke: red;
        stroke-width: 5px;
    }
</style>
```

```html
<svg width="500px" height="300px" style="border: 1px solid">
    <line x1="100" y1="100" x2="200" y2="100" class="line1"></line>
    <line x1="200" y1="100" x2="200" y2="200" class="line2"></line>
    <rect height="50" width="100" x="0" y="0" rx="10" ry="10"></rect>
</svg>
```

在svg中，所有闭合图形默认都是天生填充并直接画出来的，不需要在css中设置stroke进行绘制，rect标签的rx属性和ry属性分别为x方向上的圆角大小和y方向上的圆角大小

#### 画圆

circle标签和ellipse标签

```html
<circle r="50" cs="50" cy="200"></circle>
<ellipse rx="100" ry="30" cx="400" cy="200"></ellipse>//rx和ry分别为x和y方向上的半径大小
<polyline points="0 0 , 50 50, 50 100, 100 100, 100 50"></polyline>
```

当polyline未在css样式中设置stroke时，会自动填充，可利用stroke: 颜色、fill: transparent和stroke-width: 宽度来填充

#### 画多边形

polygon标签，其属性值与polyline的设置一致，也可通过css属性来设置样式

#### 文本

text标签，其属性值有x和y，分别代表起始x和y坐标

#### 透明度与线条样式

stroke-opacity和fill-opacity可设置线条透明度和填充透明度

线条在加粗时，宽度向两侧延伸

stroke-linecap与canvas中的lineCap属性值基本一致

stroke-linejoin与canvas中的linejoin属性值基本一致

#### Path标签

通过路径进行绘制，同样会闭合填充

```html
<svg width="500px" height="300px" style="border: 1px solid">
    <path d="M 100 100 L 200 100 L 200 200 l 100 100 H 200 V 200"></path>//M即moveTo，L即lineTo
</svg>
```

L表示移到相应坐标点，l表示向两个方向各移动相应距离

H表示horizontal，即水平方向上移动到的位置

V表示vertical，即垂直方向上移动到的位置，同理h和v为相对位置

#### Path画弧

```html
<svg width="500px" height="300px" style="border: 1px solid">
    <path d="M 100 100 A 100 50 0 1 1 150 200"></path>//M即moveTo，L即lineTo
</svg>
```

A即arc，其后边的属性分别为x轴半径、y轴半径、旋转角度、大弧（1）/小弧（0）、顺时针（0）/逆时针（1）、重点坐标

#### 线性渐变

linearGradient标签，写在defs标签内部，defs全称defines，所有的定义均放在defs标签中，在使用时通过标识符进行调用

使用方法

```html
<svg width="500px" height="300px" style="border: 1px solid">
    <defs>
        <linearGradient id="bg1" x1="0" y1="0" x2="100%" y2="100%">//这里的x1/y1/x2/y2用来指定渐变的方向，id为设置的渐变色的标识，用url进行引用
            <stop offst="0%" style="stop-color: rgb(255, 255, 0)"></stop>
            <stop offst="100%" style="stop-color: rgb(255, 0, 0)"></stop>
        </linearGradient>
    </defs>
    <rect x="100" y="100" height="100" width="200" style="fill: url(#bg1)"></rect>
</svg>
```

#### 高斯模糊

filter标签搭配feGaussianBlur进行使用

filter标签表示滤镜，其使用方法同线性渐变一样

使用方法

```html
<svg width="500px" height="300px" style="border: 1px solid">
    <defs>
        <linearGradient id="bg1" x1="0" y1="0" x2="100%" y2="100%">//这里的x1/y1/x2/y2用来指定渐变的方向，id为设置的渐变色的标识，用url进行引用
            <stop offst="0%" style="stop-color: rgb(255, 255, 0)"></stop>
            <stop offst="100%" style="stop-color: rgb(255, 0, 0)"></stop>
        </linearGradient>
        <filter id="Gaussian">
            <feGaussianBlur in="SourceGraphic" stdDeviation="20"></feGaussianBlur>
        </filter>
    </defs>
    <rect x="100" y="100" height="100" width="200" style="fill: url(#bg1); filter: url(#Gaussian)"></rect>
</svg>
```

#### 虚线

在css样式中设置stroke-dasharray，其属性值可以添加多个参数，也可填入一个值表示等间距，其值用像素来表示

```css
stroke-dasharray: 10px 20px 30px;
stroke-dashoffset: 10px;//stroke-offset用来设置偏移
```

#### viewBox

视窗

如下案例，当前视窗为坐标系中0，0点到250，150的部分

```html
<svg width="500px" height="300px" viewBox="0, 0, 250, 150" style="border: 1px solid"></svg>
```

### audio

用来在网页上插入音频的标签，其目标源用src属性进行存储，需要用controls来使其显示控制

```html
<audio src="HwVideoEditor_2020_11_14_231050.mp4" controls></audio>
```

### video

用来在网页上插入视频的标签，其目标源用src属性进行存储，同样默认控制播放等东西用controls属性打开，可通过css样式来设置其宽度、高度，鉴于该按钮不太美观，一般重新使用一个div来自己设置其播放控制按钮

#### 播放器的开始与暂停

通过video标签的==play()==和==pause()==方法可实现视频的播放和暂停，通过其==paused==属性可查看当前视频是暂停还是播放状态

```javascript
var play = document.getElementsByClassName('play')[0];
var video = document.getElementsByTagName('video')[0];
play.onclick = function(){
    if (video.paused){//通过video.paused可以查看当前视频是否在播放
        video.play();//播放
        this.innerHTML = '暂停';
    }else{
        video.pause();//暂停
        this.innerHTML = '播放';
    }
}
```

#### 播放器的时间进度

播放的总时间：==video.duration==

播放的当前时间：==video.currentTime==

以上两个值均以秒计数，实际运用过程中需使用parseInt配合除法和取余将其转换成标准的时间格式

#### 播放器的进度条

通过在播放器自定义控制栏里添加一个div当作进度条，添加一个i标签当作进度点

用一个定时器来实时监测video的duration和currentTime两个属性，并计算当前播放进度所占总进度的比例，从而与进度条总长相乘得出当前播放进度条应有的长度

为总进度条添加事件，利用e.layerX和clientWidth来计算点击的位置然后控制播放进度条的长度和进度点的位置

#### 播放器调节倍速

通过设置video.playbackRate的值即可实现视频的倍速播放

#### 音量调节

通过video.volume的值来控制声音的大小，其值最大为1，最小为0

#### 页面全屏

通过以下两行代码即可实现浏览器的全屏，除此之外还需要设置播放器元素的大小，以及进度条的显示与隐藏等属性

```javascript
var dom = document.documentElement;
dom.requestFullscreen();
```

## 进阶篇

### geoLocation

获取地理信息
一些系统不支持这个功能
定位（GPS），台式机几乎都没有GPS，笔记本绝大多数都没有GPS，智能手机几乎都有GPS
网络，来粗略的估计地理位置

使用方法：```window.navigator.geolocation.getCurrentPosition(func1, func2)```，其两个参数均为回调函数，第一个参数为获取成功后的回调函数，第二个参数为获取失败后的回调函数

```js
window.navigator.geolocation.getCurrentPosition(function (position){
    console.log(position)
}, function (){
    console.log("=======================")
});
//返回结果为一个GeolocationPosition对象，其中包含用户的地理信息
//GeolocationPosition
//coords: GeolocationCoordinates
//accuracy: 265
//altitude: null
//altitudeAccuracy: null
//heading: null
//latitude: 30.457806745872308
//longitude: 114.40100695180502
//speed: null
//__proto__: GeolocationCoordinates
//timestamp: 1617178864145
//__proto__: GeolocationPosition
```

使用限制：https协议、file协议、http协议下是不可以获取的

### 四行写个服务器

创建文件夹，并利用```npm init```创建工程，```npm install express```安装模块，创建server.js文件用来写服务器，其中代码为

```js
const express = require("express");

let app = new express();

app.use(express.static("./page"));

app.listen(12306);//端口号尽量大于8000，或等于80
//浏览器访问服务器默认为80端口
//服务器默认访问index.html
//可通过http://127.0.0.1:12306/xxx.html来访问指定的html
```

在创建完后，直接运行当前文件，即可启动创建好的服务器，在当前工程中建立page目录，并在创建好的目录下创建index.html文件，在浏览器中输入```127.0.0.1:12306```即可访问到我们创建的html文件，其中12306为先前创建好的服务器的端口号

### deviceorientation

体感事件，一般绑定到window上，只有带有陀螺仪的设备才支持体感

```js
window.addEventListener("deviceorientation", function(event){
    document.getElementById("main").innerHTML = `alpha: ${event.alpha}<br/>beta: ${event.beta}<br/>gamma: ${event.gamma};`
});
//陀螺仪，只有带有陀螺仪的设备才支持体感
//苹果设备的页面只有在https协议的情况下才能使用这些接口
//11.1.X以及之前，是可以使用的。微信的浏览器

```

参数：**alpha**: 指北（指南针）[0, 360) 当为0的时候指北，180指南
           **beta**: 平放的时候beta值为0，如果将手机立起来（短边接触桌面），直立的时候beta为90（屏幕向人）
           **gamma**：平方的时候gamma值为0.如果手机直立起来（长边接触桌面），直立的时候gamma为90（右长边着地）

手机访问电脑的条件：

1、手机要和电脑在统一局域网下
2、获取电脑的ip地址
3、在手机上输入相应的ip和端口进行访问

### devicemotion

当给window绑定devicemotion事件后，通过`event.acceleration`即可查看窗口移动的加速度，可利用`event.acceleration.x | event.acceleration.y | event.acceleration.z`来判断窗口在x、y和z方向上的加速度

### requestAnimationFrame

渲染帧

requestAnimationFrame为每秒60帧

```js
function (){
    let square = document.getElementById("main");
    if(square.offsetLeft > 700){
        return;
    }
    square.style.left = square.offsetLeft + 20 + "px";
    requestAnimationFrame(move);
}
```

`setInterval`为60帧时，当业务逻辑较为复杂时，上一帧执行未完成时，下一帧不会进行执行，因此有时候在视觉上会产生误差

而`requestAnimationFrame`会按时执行每一帧

`cancelAnimationFrame`相当于`clearTimeout`

requestAnimationFrame兼容性极差

```js
window.requestAnimationFrame = (function(){
    return window.requestAnimationFrame || window.webkitRequestAnimationFrame || window.mozRequestAnimationFrame || function(id){
        window.setTimeout(id);
    }
})()
```

### localStorage

localStorage只能存储字符串，若要存储数组或对象，可利用`JSON.stringfy`和`JSON.parse`来完成

长期放在浏览器的，写入localStorage（无论窗口是否关闭都需要存储）

本次会话临时存储在页面上的可采用sessionStorage，每次窗口关闭的时候，sessionStorage都会自动清空

localStorage和cookie的区别：
1、localStorage在发送请求的时候不会把数据发出去，cookie会把所有的数据带出去
2、cookie只能存储少量数据（4kb左右），localStorage存储的数据量要大得多（5MB）

不同的域下的localStorage存储在不容的位置

域：相同协议、相同域名、相同端口称为一个域如：（https://www.baidu.com）

### history

`history.pushState()`，有三个参数，第一个参数一般设置为对象，第二个参数一般设为null，第三个参数为当前要记录的锚点，与第一个参数对照设置

一般配合`popstate`事件进行使用，为`window`绑定popstate事件，

```js
const arr = [
    {name: "JS"},
    {name: "HTML"},
    {name: "CSS"},
    {name: "NODE"},
    {name: "TypeScript"}
];
function search(){
    let value = document.getElementById("search").value;
    let result = arr.filter(obj => obj.name.indexOf(value) > -1);
    render(result);
    history.pushState({"where": value}, null, "#" + value);
}
function render(results){
    let content = "";
    for (const result of results) {
        content += `<div>${result.name}</div>`;
    }
    document.getElementsByClassName("main")[0].innerHTML = content;
}
window.addEventListener("popstate", function(e){
    document.getElementById("search").value = e.state.where ? e.state.where : "";
    let value = document.getElementById("search").value;
    let result = arr.filter(obj => obj.name.indexOf(value) > -1);
    render(result);
})
```

`hashchange`事件也为window对象身上的一个事件，在hash值发生变化时，会触发该事件，锚点变了会触发，而`popstate`为只要url发生变化就会触发

### worker

js都是单线程的

worker是多线程的，是真的多线程，不是伪多线程

worker不能操作dom，没有window对象，不能读取本地文件，可以发送ajax请求，可以计算

使用方法：`var worker = new Worker("路径")`，用来开启新的线程

其使用过程中，onmessage为接收数据，而postmessage为发送数据到新建的线程，其使用方式同dom元素事件类似，传送的数据存放在`event.data`里

`worker.terminate()`可以结束线程

理论上在worker中还可以创建worker，但实际上没有任何一款浏览器支持

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script>
    console.log("11111111111111111111");
    console.log("22222222222222222222");
    let a = 100000;
    let worker = new Worker("./worker.js");
    worker.postMessage({
        num: a
    });
    worker.onmessage = function(e){
        console.log(e.data);
    }
    console.log("33333333333333333333");
    console.log("44444444444444444444");
</script>
</body>
</html>
```

```js
this.onmessage = function(e){
    let result = 0;
    for(let i = 0; i < e.data.num; i++){
        result += i;
    }
    this.postMessage(result);
}
```