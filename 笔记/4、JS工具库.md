## 一、本地化存储

### Cookie

![cookie原理](D:\Desktop\前端资料\Lesson One课件\3、本地化存储所需资料\01-cookie\images\cookie原理.png)

set-Cookie，服务器端进行设置的

1、cookie不可跨域

2、cookie存储在浏览器里

3、cookie有数量与大小的限制

​	1）数量在50个左右

​	2）大小在4kb左右

4、cookie的存储时间非常灵活

5、cookie不仅可以通过服务器设置，客户端也可以设置

#### cookie的属性

cookie的设置可通过document.cookie = "xxx"来完成，其中" "部分以键值对用=连接

1、name，cookie的名字，具有唯一性

2、value，cookie的值

3、Domain，设置cookie在哪个域下是有效的

4、path，cookie的路径

5、expires，cookie的过期时间，其日期格式为GMT，可用new Date()方法生成

6、max-age，cookie的有效期，与expires的功能一样，与expires的区别（expires为时间点，max-age为以秒为单位的时间段；expires用来设置过期的时间点，max-age用来设置cookie存活的时间）

常见的值：

-1（表示临时cookie，并不会生成）；

0（表示有效期已经到了）；

正数（表示当前cookie可存活的时间）

7、HttpOnly，有这个标记的cookie，前端无法获取

8、Secure，设置cookie只能通过https协议传输

9、SameSite，设置cookie在跨域请求时不能被发送

### Cookie封装及应用

元素拖拽对象封装

```javascript
var drag = {
    init: function (dom){
        this.dom = dom;
        this.bindEvent();
    },
    bindEvent: function (){
        this.dom.onmousedown = this.mouseDown.bind(this);
    },
    mouseDown: function (e){
        document.onmousemove = this.mouseMove.bind(this);
        document.onmouseup = this.mouseUp.bind(this);

        this.disX = e.clientX - this.dom.offsetLeft;
        this.disY = e.clientY - this.dom.offsetTop;
    },
    mouseMove: function (e){
        this.newLeft = e.clientX - this.disX;
        this.newTop = e.clientY - this.disY;

        this.dom.style.left = this.newLeft + 'px';
        this.dom.style.top = this.newTop + 'px';
    },
    mouseUp: function (){
        document.onmousemove = null;
        document.onmouseup = null;
    }
}
```

正常情况下，元素拖拽完成后，浏览器页面刷新会使元素回到原先的位置，通过cookie可以实现元素位置不会重新定位

利用date.setDate(date.getDate() + num)可设置从当前事件起往后推num天的日期（格式为GMT）

```javascript
var manageCookie = {
    set: function(name, value, date){
        //expires,要求用户传入的是一个时间节点(函数默认规定传进来的是天数)
        // var endDate = new Date();
        // endDate.setDate(endDate.getDate() + date);
        // document.cookie = name + "=" + value + "; expires" + endDate;

        //max-age,这里要求用户传入的date参数为秒数
        document.cookie = name + "=" + value + "; max-age=" + date;
    },
    //删除cookie
    remove: function(name){
        //要删掉cookie中的某一部分，可将其值设为空，有效期设为0
        this.set(name, '', 0)
    },
    //获取cookie
    get: function(name){
        var cookies = document.cookie.split(';');
        for (var i = 0; i < cookies.length; i++){
            var item = cookies[i].trim().split("=");
            if(item[0] === name){//说明用户传进来的名字找到了
                return item[1];
            }
        }
    }
}
// manageCookie.set('nzg', 24, 0);
// manageCookie.remove('nzg');
// console.log(manageCookie.get('nzg'));
var drag = {
    init: function (dom){
        this.dom = dom;
        this.bindEvent();

        var l = manageCookie.get('newLeft');
        var t = manageCookie.get('newTop');
        this.dom.style.left = l ? l + 'px' : '100px';
        this.dom.style.top = t ? t + 'px' : '100px';
    },
    bindEvent: function (){
        this.dom.onmousedown = this.mouseDown.bind(this);
    },
    mouseDown: function (e){
        document.onmousemove = this.mouseMove.bind(this);
        document.onmouseup = this.mouseUp.bind(this);

        this.disX = e.clientX - this.dom.offsetLeft;
        this.disY = e.clientY - this.dom.offsetTop;
    },
    mouseMove: function (e){
        this.newLeft = e.clientX - this.disX;
        this.newTop = e.clientY - this.disY;

        this.dom.style.left = this.newLeft + 'px';
        this.dom.style.top = this.newTop + 'px';
    },
    mouseUp: function (){
        document.onmousemove = null;
        document.onmouseup = null;
        //鼠标抬起的时候，需要通过cookie存储一下盒子的位置信息
        manageCookie.set('newLeft', this.newLeft, 10000)
        manageCookie.set('newTop', this.newTop, 10000)
    }
}
drag.init(document.getElementById('box'));
```

### localstorage

Web Storage提供了两个存储对象LocalStorage和sessionStorage，二者均继承了Storage对象上的所有方法，即二者的\__proto__均指向Storage

可能会经常用到JSON.stringify和JSON.parse两个方法

1、length，本地存储的数量

2、key()，通过索引找到存储的数据

3、getItem()，通过键名取到本地存储的数据，所取到的数据均为字符串，若取到的为对象形式的字符串，可通过JSON.parse(target)将其转换为对象格式

4、setItem()，设置一个本地存储数据，需传入两个参数，分别为键和值，若值为数组，则将其转为字符串，若值为对象，则解析成[Object Object]，这里可以利用JSON.stringify()方法来实现数组和对象的存储

5、remove()，删除一个本地存储数据

6、clear()，清空本地存储数据

清空localStorage的方法：

（1）localStorage.clear();

（2）在Application的localStorage处删除

（3）开启网页无痕模式，在浏览器关闭的时候会自动清空本地存储

### LocalStorage应用

购物车案例

HTML部分：整个页面由一个div构成，这个div下包含两个table标签，顶部table用来存储商品信息，其中商品信息通过Ajax请求数据动态加入，底部table用来存放已选择的商品

```html
    <div id="shopping">
        <table class="product">
            <tbody>
                <!-- <tr>
                    <td>
                        <img src="images/img_01-1.png" />
                    </td>
                    <td>
                        <p>打不翻的吸盘碗</p>
                        <div class="color">
                            <span data-id="10001">粉色</span><span data-id="10002">蓝色</span><span data-id="10003">黄色</span><span data-id="10004">绿色</span>
                        </div>
                    </td>
                    <td>21.00元</td>
                    <td>
                        <span>-</span>
                        <strong>0</strong>
                        <span>+</span>
                    </td>
                    <td><button>加入购物车</button></td>
                </tr> -->
            </tbody>
        </table>

        <table class="selected">
            <thead>
                <tr>
                    <th>已选中商品</th>
                    <th colspan="5">
                        <span>应付总额：</span><strong>0.00元</strong>
                    </th>
                </tr>
            </thead>

            <tbody>
                <!-- <tr>
                    <td>
                        <img src="images/img_04-1.png" />
                    </td>
                    <td>
                        <p>中筒皮毛一体雪地靴</p>
                    </td>
                    <td>红色</td>
                    <td>359.00元</td>
                    <td>x2</td>
                    <td><button>删除</button></td>
                </tr> -->
            </tbody>
        </table>
    </div>
```

购物车事件添加，首先通过Ajax对数据发起请求，然后利用回调函数将请求到的数据进行页面渲染（通过字符串的形式为tbody添加innerHTML）。

在页面渲染结束后，利用addEvent()函数为颜色选项卡、数量选择器和加入购物车按钮添加事件，页面数据是按照每个商品一行的形式进行布局的，所以这里按表格的行来完成事件添加，选项卡部分通过控制上一次选中的颜色与当前点击颜色信息的比对来控制颜色选项卡的选中情况，通过className属性进行控制，然后利用当前dom节点的className情况来控制img标签中图片的显示情况，这里通过==dom.dataset==可以查看标签中的自定义属性。数量选择器通过左右按钮控制数量选择器内容，加入购物车按钮需添加条件判断，当未选择颜色时，弹出“请选择颜色”，未选择数量时弹出“请添加购买数量”。

在添加完商品后，将添加的商品信息存储到localStorage中以供下次使用（然后将商品蓝中的颜色选项卡及数量选择器的内容重新初始化），所以需要在开头声明selectData并在点击加入购物车按钮之后将加入购物车的商品的信息存储到selectData中，需注意的是，创建完selectData后需读取localStorage中先前存储的商品信息并将内容赋给selectData

既然有了selectData，就还需要渲染已选商品的dom结构，这里涉及到一个ES7里的方法，Object.values(obj)，可以取出obj中的所有value并放到一个数组里（若考虑兼容性，可直接用for...in循环），这里的dom结构渲染同商品栏类似，然后需要为删除按钮添加事件，这个事件既要删除localStorage里的对应数据，还要删除dom结构中的对应数据，并修改总价格

最后还需要通过addEventListener方法为window添加storage方法，来实现在其它页面动态更新购物车里的数据

```javascript
var selectData = {};  //用来存储所有的商品信息，以及所有的商品
//在页面打开的时候，将原先存储在localStorage里的内容拿到并赋给selectData
function init() {
	//第一次的时候取到的为空,但是JSON.parse返回的结果为Null
	selectData = JSON.parse(localStorage.getItem('shoppingCart')) || {};

	createSelectDom();
}
init();

//请求数据
ajax('js/shoppingData.json', function (data) {
	//console.log(data);
	createGoodsDom(data);
	addEvent();
});

//创建商品结构
function createGoodsDom(data) {
	var str = '';

	data.forEach(function (item) {
		var color = '';  //用来存储每条数据里面的所有颜色信息
		item.list.forEach(function (product) {
			color += '<span data-id="' + product.id + '">' + product.color + '</span>';
		});

		str += '<tr>' +
			'<td>' +
			'<img src="' + item.list[0].img + '" />' +
			'</td>' +
			'<td>' +
			' <p>' + item.name + '</p>' +
			'<div class="color">' + color + '</div>' +
			'</td>' +
			'<td>' + item.list[0].price + '.00元</td>' +
			'<td>' +
			'<span>-</span>' +
			'<strong>0</strong>' +
			'<span>+</span>' +
			'</td>' +
			'<td><button>加入购物车</button></td>' +
			'</tr> ';
	});

	var tbody = document.querySelector('.product tbody');
	tbody.innerHTML = str;
}

//添加商品操作事件
function addEvent() {
	var trs = document.querySelectorAll('.product tr');   //获取到所有的行
	for (var i = 0; i < trs.length; i++) {
		action(trs[i], i);
	}

	function action(tr, n) {
		var tds = tr.children,    //当前行里所有的td
			img = tds[0].children[0], //商品图片
			imgSrc = img.getAttribute('src'), //商品图片的地址
			name = tds[1].children[0].innerHTML,  //商品的名字
			colors = tds[1].children[1].children,  //所有颜色按钮
			price = parseFloat(tds[2].innerHTML), //价格
			spans = tds[3].querySelectorAll('span'), //加减按钮
			strong = tds[3].querySelector('strong'), //数量
			joinBtn = tds[4].children[0], //加入购物车的按钮
			selectNum = 0;    //选中商品的数量

		//颜色按钮点击功能
		var last = null,  //上一次选中的按钮
			colorValue = '',  //选中的颜色
			colorId = ''; //选中商品对应的id
		for (var i = 0; i < colors.length; i++) {
			colors[i].index = i;  //添加一个自定义的属性为索引值
			colors[i].onclick = function () {
				//运算符的优先级要高于=，即&&的优先级高于=，首先判断last是否为空，若为空，则向下执行，否则判断last于当前点击的颜色是否为同一个
				//若为同一个，则向下执行，否则将上一个选中的按钮className设为空
				last && last !== this && (last.className = '');
				//判断当前点击的颜色是否为被选中状态，若为被选中状态则将其className设为空，否则设为active
				this.className = this.className ? '' : 'active';
				colorValue = this.className ? this.innerHTML : '';
				colorId = this.className ? this.dataset.id : '';
				//当前选中图片的名称用行列命名
				imgSrc = this.className ? 'images/img_0' + (n + 1) + '-' + (this.index + 1) + '.png' : 'images/img_0' + (n + 1) + '-1.png';
				img.src=imgSrc;
				last = this;  //把当前次点击的对象赋值给last。当前次点击的对象相对于下次要点击的时候它是上一个对象（last）
			}
		}

		//减按钮点击
		spans[0].onclick = function () {
			selectNum--;
			if (selectNum < 0) {
				selectNum = 0;
			}

			strong.innerHTML = selectNum;
		};

		//加按钮点击
		spans[1].onclick = function () {
			selectNum++;

			strong.innerHTML = selectNum;
		};


		//加入购物车功能
		joinBtn.onclick = function () {
			if (!colorValue) {
				alert('请选择颜色');
				return;
			}

			if (!selectNum) {
				alert('请添加购买数量');
				return;
			}

			//给ectData对象赋值
			selectData[colorId] = {
				"id": colorId,
				"name": name,
				"color": colorValue,
				"price": price,
				"num": selectNum,
				"img": imgSrc,
				"time": new Date().getTime(),
			};
			localStorage.setItem('shoppingCart', JSON.stringify(selectData));

			//加入购物车后让所有已经选择的内容还原
			img.src = 'images/img_0' + (n + 1) + '-1.png';
			last.className = '';
			strong.innerHTML = selectNum = 0;

			createSelectDom();  //存储完数据后要渲染购物车里商品的结构
		}
	}
}

//创建购物车商品结构
function createSelectDom() {
	var tbody = document.querySelector('.selected tbody');
	var totalPrice = document.querySelector('.selected th strong');
	var str = '';
	var total = 0;    //总共多少钱

	//console.log(selectData);
	var goods = Object.values(selectData);    //ES7里面的方法,用来取到对象里的所有value，并把取到的内容放到一个数组里

	//对已选择的商品进行排序（这里采用逆序，即加入时间晚的放在前面）
	goods.sort(function (g1, g2) {
		return g2.time - g1.time;
	});

	//console.log(goods);

	tbody.innerHTML = '';
	for (var i = 0; i < goods.length; i++) {
		str += '<tr>' +
			'<td>' +
			'<img src="' + goods[i].img + '" />' +
			'</td>' +
			'<td>' +
			'<p>' + goods[i].name + '</p>' +
			'</td>' +
			'<td>' + goods[i].color + '</td>' +
			'<td>' + goods[i].price * goods[i].num + '.00元</td>' +
			'<td>x' + goods[i].num + '</td>' +
			'<td><button data-id="' + goods[i].id + '">删除</button></td>' +
			'</tr>';

		total += goods[i].price * goods[i].num;
	}
	tbody.innerHTML = str;
	totalPrice.innerHTML = total + '.00元';

	del();  //结构创建完成了，添加删除功能
}

//删除功能
function del() {
	var btns = document.querySelectorAll('.selected tbody button');
	var tbody = document.querySelector('.selected tbody');

	for (var i = 0; i < btns.length; i++) {
		btns[i].onclick = function () {
			//删除对象里的数据
			delete selectData[this.dataset.id];
			localStorage.setItem('shoppingCart', JSON.stringify(selectData));

			//删除DOM结构
			tbody.removeChild(this.parentNode.parentNode);

			//更新总价格
			var totalPrice = document.querySelector('.selected th strong');
			totalPrice.innerHTML=parseFloat(totalPrice.innerHTML) - parseFloat(this.parentNode.parentNode.children[3].innerHTML)+'.00元';
		};
	}
}
//清空购物车可通过localStorage.clear()直接清空购物车商品数据，dom部分可直接dom.innerHTML = ''进行完成

//storage事件
window.addEventListener('storage',function(ev){
	console.log('我在04页面修改了购物车，这个log会在05的页面打印出来；或者我在05的页面修改了购物车，这个log会在04的页面打印出来！');

	console.log(ev.key);	//修改的是哪一个localstorage（名字key）
	console.log(ev.newValue);	//修改后的数据
	console.log(ev.oldValue);	//修改前的数据
	console.log(ev.storageArea);	//storage对象
	console.log(ev.url);	//返回操作的那个页面的url

	init();
});
```

CSS部分代码

CSS3方法：xx:nth-of-type(n)可匹配同类型标签的第n个
如：.product td:nth-of-type(1)即匹配.product下的第一个td标签

css中border-style的样式有dotted（点状）、solid（实线）、double（双线）和dashed（虚线）

```css
dl,dd,p{
	margin: 0;
}
table{
	border-collapse:collapse;
}
th,td{
	padding:0;
}
input{
	margin: 0;
	padding: 0;
	vertical-align: middle;
}
label{
	vertical-align: middle;
}
body{
	font-family: "微软雅黑";
}
img{
	vertical-align: middle;
}
a{
	text-decoration: none;
}
.clearfix:after{
	content: '';
	display: block;
	clear: both;
}


#shopping{
	width: 1000px;
	margin: 100px auto;
}
#shopping table{
	width: 100%;
}
/* #shopping th{
	height: 50px;
	color: #333;
	font-size: 16px;
	font-weight: normal;
} */
#shopping img{
	background: #f4f4f4;
	border: 1px solid #eaeaea;
}

.product{
	border: 1px solid #ddd;
}
.product tr{
	border-bottom: 1px dashed #ddd;
}
.product tr:hover,.product tr.active{
	background: #fffbf0;
}

.product td{
	text-align: center;
	padding: 15px 0;
}
/*
xx:nth-of-type(n)可匹配同类型标签的第n个
如：.product td:nth-of-type(1)即匹配.product下的第一个td标签
*/
.product td:nth-of-type(1){
	width: 180px;
}
.product td:nth-of-type(2){
	width: 300px;
	text-align: left;
}
.product td:nth-of-type(5) button{
	height: 50px;
	padding: 0 20px;
	background: #cb4a61;
	color: #fff;
	cursor: pointer;
	border: none;
	outline: none;
}
.product p{
	color: #333;
	font-weight: bold;
	margin-bottom: 15px;
}

.product .color span{
	display: inline-block;
	width: 54px;
	height: 26px;
	line-height: 26px;
	text-align: center;
	color: #666;
	font-size: 12px;
	border: 1px solid #ddd;
	margin-right: 10px;
	cursor: pointer;
	box-sizing: border-box;
}

.product .color span.active{
	border:2px solid #b4a078;
	line-height: 24px;
	background: url(../images/ico_02.gif) no-repeat right bottom;
}
.product td:nth-of-type(3){
	color: #d90808;
	width: 200px;
}
.product td:nth-of-type(4){
	font-size: 0;
	width: 200px;
}
.product td:nth-of-type(4) span{
	width: 24px;
	height: 24px;
	display: inline-block;
	border: 1px solid #ddd;
	text-align: center;
	line-height: 24px;
	font-size: 12px;
	vertical-align: middle;
	cursor: pointer;
	font-size: 14px;
}
.product td:nth-of-type(4) strong{
	width: 58px;
	height: 24px;
	line-height: 24px;
	display: inline-block;
	font-size: 12px;
	text-align: center;
	vertical-align: middle;
	border-top: 1px solid #ddd;
	border-bottom: 1px solid #ddd;
}
.product td:nth-of-type(5){
	color: #d90808;
	width: 200px;
}

.selected{
	border: 1px solid #ddd;
	margin-top: 30px;
}
.selected thead{
	background: #f5f5f5;
	border: 1px solid #ddd;
}
.selected th{
	height: 50px;
}
.selected th span,.selected th strong{
	vertical-align: middle;
}
.selected th strong{
	color: #d90808;
	font-size: 24px;
}
.selected tbody{
	font-size: 14px;
	color: #999;
}
.selected tr{
	border-bottom: 1px dashed #ddd;
}
.selected td{
	text-align: center;
	padding: 15px 0;
}
```

### Restful API

是一种互联网软件架构的设计规范、设计指南、设计风格、设计原则

1、API（Application Programming Interface）——应用程序接口（简称“接口”）

2、rest（Resource Representational State Transfer）——表述性状态传递

1）Resourse 资源

​	URI：统一资源标识符，是一个字符串，用来标识互联网资源的名称

​	URL：统一资源定位符，它是一种具体的URI

2）Representational 表现层

​	文本 text\html\xml\json\二进制

3）State Transfer 状态转化

总结：在网络上看得到的看不到的东西都称为资源（如图片、音乐等），每个资源都有一个自己特定的URI，肉眼看到的有一个具体的表现方式即为表现层，前后端可以互相传送这个资源，即为REST，也称为Restful架构

Restful API具体设计规范：

1、协议

​	要求通信方式为HTTPS协议

2、域名

​	尽量把API部署在专门的域名下

​	如：https://api.kaivon.com

​		   https://www.kaivon.com/api/

3、版本

​	https://api.kaivon.com/v1

4、路径（路径中不能出现动词）

​	https://api.kaivon.com/v1/blogs

5、方法

​	1）GET 获取资源

​		GET https://api.kaivon.com/v1/blogs    获取所有的文章

​		GET https://api.kaivon.com/v1/blogs/id 获取到某一篇文章

​	2）POST 添加资源

​		POST https://api.kaivon.com/v1/blogs    增加一篇文章

​	3）PUT 修改资源（客户端需要提供改变后的完整资源）

​		PUT https://api.kaivon.com/v1/blogs/id 修改某一篇文章

​	4）PATCH 更新资源（客户端需要提供改变的属性）

​	5）DELETE 删除资源

​		DELETE https://api.kaivon.com/v1/blogs/id   删除某一篇文章

6、数据过滤

​	1）?limit=10   指定返回数据的数量

​		GET https://api.kaivon.com/v1/blogs?limit=10

​	2）?offset=10  指定一个偏移量

​		GET https://api.kaivon.com/v1/blogs?offset=10

​	3）?page=2&per_page=10 指定第几页，以及每页的数量

​		GET https://api.kaivon.com/v1/blogs?offset=10

​	4）?sortby=time&order=arc  指定返回结果按照哪个属性排序，以及排序的方式

​		GET https://api.kaivon.com/v1/blogs?sortby=time&order=arc

7、状态码

8、返回结果

​	1）GET      资源对象列表（数组），如果取的是一条数据，那返回一个对象

​	2）POST      返回添加后的资源对象，以及有可能会加上是否成功

​	3）PUT         返回修改后的资源对象，以及有可能会加上是否成功

​	4）PATCH     返回更新后的资源对象，以及有可能会加上是否成功

​	5）DELETE   返回空，以及有可能会加上是否成功

9、返回的数据格式

​	尽量使用JSON，避免使用XML

**总结：**

**1、看URL就知道要什么**

**2、看HTTP method就知道干什么**

**3、看HTTP status code就知道结果如何**

## 二、JQuery

### JQuery简介

在JQuery中用`$`代替JQuery进行使用，使用方法$(document).ready(function(){})，ready方法可保证函数在页面dom结构加载完之后才执行，使用ready方法可将\<script>放在\<head>标签中

ready和onload方法的区别：当页面dom元素加载完成后就会触发ready，而onload是在整个页面渲染完成后才会触发，如下：

```javascript
$(document).ready(function(){
    console.log('ready完成了');
});
window.onload=function(){
    console.log('load完成了');
};
document.addEventListener('DOMContentLoaded',function(){
    console.log('dom内容加载完毕');
});
//以页面请求外网链接为例，打印结果为
//dom内容加载完毕
//ready完成了
//load完成了
```

JQuery可通过$('选择器')来选择标签，同时可用$('标签名')来创建标签，可通过.css()方法来为创建的标签添加样式，css方法传入参数可以是一个对象，写法同css文件内写法一样，当仅添加一个属性时，可通过传入属性名和属性值来完成，传入均为字符串，用,隔开，可用a.appendTo('标签名')将某个标签添加为b标签的子节点

```javascript
$('#btn').click(function(){
    $('div').css({
        width:'100px',
        height:'100px',
        background:'green'
    }).appendTo('body');
});
```

多样的选择器

```javascript
var li=$('ul li:nth-child(1)');//选择ul下的第一个li标签
var li=$('#ulId li:first-child');//选择#ulId列表下的第一个li标签
var li=$('.ulClass li:first-child + li');//选择.ulClass下的第二个li标签
var li=$('li[name=blue]');//选择name=blue的li标签
```

链式操作

```javascript
$('.text').html('陈学辉').css('border','1px solid #000').width(200).height(200).css('background','grey');
//html()方法相当于innerHTML
```

原生JS获取的dom节点不可以使用JQuery中的方法，JQuery中获取的dom节点同样也不可以使用原生js中的方法，原生JS获取到的dom节点和JQuery获取到的dom节点不同，原生JS获取的dom节点为对象，而JQuery获取的dom节点为一个数组，二者可以相互转换

```javascript
//原生转jquery
$(h1).css('color','blue');

//jquery转原生
$h1[0].style.color='purple';
```

### JQuery选择器

#### 基础选择器

```javascript
/* 基础选择器 */
$('#list1').css('background','green');//id选择器
$('.red').css('background','grey');//class选择器
$('li').css('border','2px solid pink');//标签选择器
//$('*').css('border','2px solid orange');//通配符选择器
$('li,p').css('font-size','20px');//群组选择器
```

#### 层级选择器

```javascript
//层级选择器
$('div a').css('color','green');//选择div后代节点中的所有a标签
$('div>a').css('color','red');//选择div子结点中的所有a标签
$('div a.link + a').css('color','purple');//即div下class为link的下一个a标签
$('div a.link ~ a').css('color','blue');//~代表a.link后的所有兄弟a标签
```

#### 属性选择器

|=表示以xx为前缀，用-隔开

^=表示已xx开头，不需要用-隔开

$=表示以xx结尾

*=属性中包含某字串

~=属性中包含某单词，用空格隔开

```javascript
//属性选择器
$('#ulColor li[class]').css('background','pink');  //选择所有带有class属性的li标签
$('#ulColor li[title=blue]').css('background','grey');  //选择title属性值为blue的li标签
$('#ulColor li[title!=blue]').css('background','yellowgreen');  //选择title属性值不为blue的所有li标签
$('#ulColor li[title|=css]').css('background','darkgreen'); //选择title属性值为以css作为前缀的li标签   //前缀是用-隔开的
$('#ulColor li[id^=color]').css('background','hotpink');   //以属性值为开始（不需要-隔开）
$('#ulColor li[id$=color]').css('background','purple');  //id为以color结尾的li标签
$('#ulColor li[lang*=cn]').css('background','olive');   //属性中包含cn字符串
$('#ulColor li[lang~=cn]').css('background','skyblue');    //属性中包含cn单词，用空格隔开
$('#ulColor li[class=cl][name=kaivon]').css('background','teal');  //属性中包含cn单词，用空格隔开
```

#### 基础过滤选择器

```javascript
//基础过滤选择器
$('#olColor li:eq(1)').css('border','5px solid pink');//选择索引值为1的li标签，index为2即第二个li标签
$('#olColor li:gt(1)').css('border','5px solid grey'); //大于
$('#olColor li:lt(3)').css('border','5px solid yellowgreen');  //小于
$('#olColor li:not(#olColor li:eq(2))').css('border','5px solid darkgreen');   //排除
$('#olColor li:even').css('border','5px solid hotpink');   //偶数
$('#olColor li:odd').css('border','5px solid purple'); //奇数
$('#olColor li:first').css('border','5px solid olive');    //第一个
$('#olColor li:last').css('border','5px solid skyblue');   //最后一个
$('#olColor li:lang(en)').css('border','5px solid teal');  //lang属性
$('#olColor li:target(tar)').css('border','5px solid yellow'); //tatget属性
$(':root').css('border','2px solid blue'); //根节点
$(':header').css('border','5px solid darkgreen');  //所有的h标签
```

#### 子元素过滤器

```javascript
//子元素过滤器
$('#paragraph p:first-child').css('color','pink'); //第一个子元素必需是p标签
$('#paragraph span:first-of-type').css('color','yellowgreen'); //选择到第1个span标签
$('#paragraph span:last-child').css('color','darkgreen');//最后一个子元素必须是span标签
$('#paragraph p:last-of-type').css('color','hotpink');
$('#paragraph p:nth-child(2)').css('color','blue');    //选择到第2个子元素，并且这个子元素必需是p标签
$('#paragraph span:nth-of-type(2)').css('color','olive');//选择到#paragraph下的第二个span标签
$('#only p:only-child').css('color','skyblue');    //选择到只有一个子元素的标签
$('#only-two span:only-of-type').css('font-size','30px');  //选择到只有一个span子元素的标签，所选到的标签是span标签
```

#### 内容过滤选择器

```javascript
//内容过滤选择器
$('#content:contains(kaivon)').css('color','blue');//选择内容为kaivon的标签
$('div:empty').css({
   width:'100px',
   height:'100px',
   background:'green'
});//选择内容为空的div标签
$('#has:has(p)').css('border','1px solid #000');//选择其中包含p标签的标签（子节点孙节点无所谓，只要有）
$('#title:parent').css('border','1px solid #f00');//选择所有含有子元素或者文本的父级元素
```

#### 表单过滤选择器

```javascript
//表单过滤选择器
$(':button').css('border','2px solid #f0f');   //选择到所有的按钮
$('#sex input:checkbox').wrap('<span></span>').parent().css('border','2px solid purpLe');//选择到类型为checkbox的input标签，并为其增加父级span标签，然后对添加的span标签进行样式设置
$(':checked').wrap('<span></span>').parent().css('border','2px solid blue');//选择已被选中的input标签
```

### DOM操作

#### 操作class

```javascript
//操作class
$('.setClass li:first').addClass('red');   //添加class
$('.setClass li:eq(1)').removeClass('green');  //移除class(不给参数，移除所有class)
console.log(	//是否包含某个class
    $('.setClass li:last').hasClass('green'),	//true
    $('.setClass li:last').hasClass('orange'),	//false
);
//toggleClass 切换class。在添加、删除间切换
$('.setClass p').click(function(){
    $(this).toggleClass();
});
```

#### 插入元素

```javascript
//插入元素（内部插入）
$('.insideAdd').append('<h2>append方法插入</h2>');	//append()，在元素里面的末尾插入DOM。这个与appendChild的方法是一样的。
$('.insideAdd').append($('.insideAdd p'));
$('<h2>appendTo方法插入</h2>').appendTo('.insideAdd');	//将匹配的元素插入到目标元素的最后面。这个与append一样，只不过内容和目标的位置相反。append方法里直接写一个标签的字符串，就相当于创建一个DOM对象
$('.insideAdd').prepend('<h2>prepend方法插入</h2>');	//prepend()，与append的语法一样，只不过是插入到父级元素的前面
$('<h2>prependTo方法插入</h2>').prependTo('.insideAdd');	//prependTo()，与appendTo是一样的，不同的也是插入的位置是在前面

//插入元素（外部插入，插入为兄弟节点）
$('.outsideAdd h2').after('<p>after方法添加到h2后面</p>');	//after()（语法类似于append）
$('<p>insertAfter方法添加到h2后面</p>').insertAfter('.outsideAdd h2');	//insertAfter()（语法类似于appendTo）
$('.outsideAdd h2').before('<p>before方法添加到h2前面</p>');	//before()（语法类似于prepend）
$('<p>insertBefore方法添加到h2前面</p>').insertBefore('.outsideAdd h2');	//insertBefore()（语法类似于prependTo）

//插入元素，html与text方法。相当于原生的innerHTML、innerText属性
console.log($('.htmlText').html());//获取
$('.htmlText').html('<h2>这是html方法添加的标题</h2><p>这是html方法添加的内容</p>');	//设置
console.log($('.htmlText').text());	//获取
$('.htmlText').text('<h2>这是text方法添加的标题</h2><p>这是<em>text</em>方法添加的内容</p>');	//设置
```

#### 包裹元素

```javascript
//包裹元素
$('.wrap span').wrap('<li>');  //wrap()，在每个匹配的元素外层包上一个html元素
$('.wrap li').wrapAll('<ul>'); //wrapAll()，在所有匹配元素外面包一层HTML元素
$('.wrap span').wrapInner('<strong>'); //wrapInner()，在匹配元素里的内容外包一层结构
$('.wrap li').unwrap();    //.unwrap()，将匹配到的元素的父级删除
```

#### 删除元素

```javascript
//删除元素
//$('.del .title').remove();   //remove()，移除自己
$('div').remove('.title'); //也可以添加参数。从div中移除一个.title的div
$('.del ul').empty();  //empty()，清空子元素
$('.del .end').click(function(){
   alert(1);
});
//detach()，与remove()一样，这两个方法都有一个返回值，返回被删除的DOM。它们的区别就在这个返回值身上
//detach在删除元素后，保留其所删元素的事件，而remove将元素的事件一并删除
var end=$('.del .end').detach();   //再次添加后是有事件的
//var end=$('.del .end').remove(); //再次添加后没有事件
setTimeout(function(){
   $('.del').append(end); //1s后，被删除的那个元素会被重新添加上
},1000);
```

#### 克隆与替换元素

```javascript
//克隆与替换元素
$('.clone p').click(function(){
   alert(2);
});
//$('.clone p').clone().appendTo('.clone');
$('.clone p').clone(true).appendTo('.clone');  //clone的参数为true时表示，会把事件也克隆了
$('<h3>使用replaceAll方法主动替换</h3>').replaceAll('.clone .replaceAll');//创建一个元素然后用它替换掉其它元素
$('.clone .name2').replaceAll('.clone .name1');//使用已有的元素替换掉其它元素（剪切操作）
$('.clone .replaceWidth').replaceWith('<h3>使用replaceWidth方法被动替换</h3>');
```

#### 属性操作——通用属性操作

```javascript
//属性操作-通用属性操作
console.log($('.attr img').attr('src'));   //images/img_01.jpg（如果有多个img的话，它返回的是第一个img的src值）//取到的src值为相对地址
$('.attr img').attr('title','这是一张美食图片');   //如果有多个img的话，设置的是所有的img
$('.attr img').attr({  //同时设置多个属性
   class:'delicious',
   alt:'美食'
});
console.log($('.attr img').prop('src'));//取到的为绝对地址
console.log(
   $('.attr img').attr('kaivon'), //liu
   $('.attr img').prop('kaivon'), //undefined，prop无法获取标签的自定义属性
);
$('.attr img').prop({
   id:'food',
   kaivon:'liuliu',   //自定义的属性prop并没有添加到DOM标签身上，但是它会添加到DOM对象身上   
});
$('.attr img').attr('kaivon','liuliuliu');
$('.attr img').removeAttr('kaivon');
$('.attr img').removeProp('id');   //删除的是DOM对象身上的属性，并不是DOM标签身上的属性
$('.attr img').prop('index',5);
console.log($('.attr img').prop('index')); //5 这条属性是添加在DOM对象身上
$('.attr img').removeProp('index');
console.log($('.attr img').prop('index')); //undefined removeProp是删除DOM对象身上的属性
console.log($('.attr input').val());   //这是一个正经的输入框
$('.attr input').val('这不是一个输入框');//返回匹配元素集合中第一个元素的当前值或设置匹配元素集合中每个元素的值
```

#### 属性操作——css类属性操作

```javascript
//属性操作-css类属性操作
console.log(
   $('.css').css('border'),   //2px solid rgb(0, 0, 0)
   $('.css').css('height'),   //19.9125px
);
$('.css h2').css('width','200px').css('height','100px').css('background','#ccc').text('插入一个标题');//链式操作
$('.css h2').css({
   color:'green',
   fontSize:'30px',
   'line-height':'100px',
});
$('.css p').css({
   width:'200px',
   height:'200px',
   padding:'20px',
   margin:'20px auto',
   border:'2px solid #f00',
});
console.log(
   $('.css p').width(),   //200
   $('.css p').height(),  //200
   $('.css p').innerWidth(),  //240  获取包含padding的宽度（clientWidth）
   $('.css p').innerHeight(), //240
   $('.css p').outerWidth(),  //244  获取包含border的宽度（offsetWidth）
   $('.css p').outerHeight(), //244
);
$('.css p').width(300).height(100).innerWidth(400).outerWidth(500);    //给width与给innerWidth设置的都是width属性的值。但是innerWidth与outerWidth都会动态的算出一个宽度值，赋给width属性

$('.css').css('position','relative');  //先把父级设置成相对定位
$('.css div').css({
   width:'100px',
   height:'100px',
   background:'green',
   position:'absolute',
   left:'100px',
   top:'200px'
});
//相对于document
console.log(
   $('.css div').offset().left,   //110   offset()是相对于document的偏移
   $('.css div').offset().top,       //1648.3499755859375
   //此方法没有.right与.bottom
);
$('.css div').offset({
   left:200,
   top:1800,
});

//获取相对于有定位的父级的位置信息
console.log(
    $('.css div').position().left,
    $('.css div').position().top,
);

console.log(
    $(document).scrollTop(),//页面垂直滚动条
    $(document).scrollLeft(),//页面水平滚动条
);
setTimeout(function(){
    $(document).scrollTop(300);
},2000);
```

### 遍历

#### 获取后代元素

```javascript
//获取后代元素
$('.child').children().css('border-color', 'green');//获取子元素
$('.child').children(':eq(1)').css('border-width', '3px');
$('.child').find('span').css({
    'color': 'red',
    'font-size': '30px',
});//在子元素中查找所有的span标签
```

#### 获取祖先元素

```javascript
//获取祖先元素
$('.parent li ul li:first').parent().css('border', '1px solid blue');//获取父级标签，可传入参数限制条件
// $('.parent li ul li:first').parents().css('border', '2px solid purple');//获取所有祖先标签，可传入参数限制条件
$('.parent li ul li:first').parents('ul').css('border', '2px solid purple');
$('.parent li ul li:first').parentsUntil('li').css('border', '5px solid pink');//获取祖先元素，指定范围限制
$('.parent li ul li:first').offsetParent().css('border', '10px solid skyblue');//获取有定位的祖先元素

//获取祖先元素.closest()
$('.closest li ul li').closest('li').css('border', '5px solid skyblue');//从当前元素开始向上获取第一个指定祖先元素（包括自己）
```

#### 获取兄弟元素

```javascript
//获取后面的兄弟元素
// $('.next li:first').next().css('background', 'cyan');
$('.next li:first').nextAll('li').css('border', '2px solid black');//获取后面所有的兄弟元素，可通过传入参数来限制条件
$('.next li:first').nextUntil('div').css('border', '2px solid cyan');//通过条件限制获取当前元素后面的兄弟节点

//获取前面的兄弟节点
$('.prev li:last').prev('li').css('background', 'cyan');//获取前一个兄弟节点，可通过传入参数来限制条件
$('.prev li:eq(3)').prevAll().css('background', 'green');//获取当前节点前边的所有节点，可通过参数限制
$('.prev li:eq(3)').prevUntil('p').css('background', 'pink');//获取当前节点前边的标签，通过参数限制，获取结果不包含限制条件里的标签

//获取所有的兄弟节点
$('.sibling li:eq(2)').siblings().css('border', '5px solid skyblue');//获取所有兄弟节点，不包含自己
$('.sibling li:eq(2)').siblings('.select').css('background', 'yellow');//获取所有兄弟节点，不包含自己
```

### 事件

#### 绑定事件

通过事件名称绑定

```javascript
$('#btn1').mouseover(function () {
   $(this).css('background', 'orange');
}).mouseout(function () {
   $(this).css('background', 'grey');
});
```

通过on添加

可接收四个参数，当为四个参数时，第一个参数为事件名称，第二个元素为选择添加到哪个标签上，第三个参数为事件对象，第四个参数为事件函数

```javascript
$('#btn2').on('click', function () {
   $(this).css('background', 'blue');
});
$('#btn2').on('click', { name: 'kaivon' }, function (event) {
   console.log(event.data.name);
});//event这里表示{name: 'kaivon'}
$('#btn3').on('click', 'h2', { color: 'red' }, function (event) {
   //$(this)指向h2
   $(this).css('border', '1px solid ' + event.data.color);
});
```

on可以添加多个事件

```javascript
$('#btn2').on({
   mouseover: function () {
      $(this).css('background', 'pink');
   },
   mouseout: function () {
      $(this).css('background', 'cyan');
   }
});
```

通过one绑定事件

通过one绑定的事件只会触发一次

```javascript
$('#btn4').one('click', 'button', { color: 'purple' }, function (event) {
   $(this).css('background', event.data.color);
   console.log('只会打印一次');
});
```

#### 解除事件

off() 

```javascript
//解除事件：off()
$('#btn5').click(function () {
   //$('#btn1').off();    //没有参数，会把所有的事件全部解除
   $('#btn1').off('mouseover');
});
```

#### 手动触发事件

```javascript
//手动触发绑定事件：trigger()
$('#btn6').on({
   click: function () {
      console.log('btn6的点击事件');
   },
   mouseover: function (event, name, age) {
      console.log('btn6的鼠标移入事件：' + name + ' : ' + age);
      $(this).css('background', 'brown');
   },
   end: function () {
      console.log('这是一个自定义的事件');
   }
});

setTimeout(function () {
    $('#btn6').trigger('click');
}, 500);
setTimeout(function () {
    $('#btn6').trigger('mouseover',['kaivon',18]);
}, 1000);
setTimeout(function(){
    $('#btn6').trigger('end');
},1500);
```

#### trigger()和triggerHandler()

区别1：默认行为

```javascript
//区别1：trigger()会触发事件的默认行为；triggerHandler()不会触发事件的默认行为
$('input').focus(function(){
   console.log('获取到焦点了');
});
$('#trigger').click(function(){
   $('input').trigger('focus');
});
$('#triggerHandler').click(function(){
   $('input').triggerHandler('focus');
});
```

区别2：执行事件的元素的数量

```javascript
//区别2：trigger()会执行所有选中元素的事件；triggerHandler()只会执行第一个元素的事件
$('#color li').click(function(){
   console.log($(this).html()+' '+$(this).index());
});
setTimeout(function(){
   //$('#color li').trigger('click');//会执行选中的所有li标签的事件
   $('#color li').triggerHandler('click');//仅执行选中的li标签中第一个li标签的事件
},2000);
```

区别3：是否冒泡

```javascript
//区别3：trigger()会冒泡；triggerHandler()不会冒泡
$('#bubble h2').click(function(){
   console.log('h2被点击了');
});
$('#bubble span').click(function(){
   console.log('span被点击了');
});
setTimeout(function(){
   //$('#bubble span').trigger('click');
   $('#bubble span').triggerHandler('click');
},2500);
```

区别4：是否可以使用链式操作

```javascript
//区别4：trigger()可以使用链式操作；triggerHandler()不可以使用链式操作
$('#btn7').on({
   mouseover:function(){
      $(this).css('width','200px');
      return $(this);
   },
   mouseout:function(){
      $(this).css('height','200px');
   }
});
setTimeout(function(){
   // $('#btn7').trigger('mouseover').trigger('mouseout');
   $('#btn7').triggerHandler('mouseover').triggerHandler('mouseout');
},3000);//可通过在传入事件函数中加入return $(this)来实现tirggerHandler()的链式操作
```

#### 事件对象

JQuery事件对象中用pageX和pageY来表示事件发生时鼠标的位置

```javascript
/* 事件对象 */
$('#btn8').click(function(event){
   console.log(event);
});
$('#btn9')[0].onclick=function(ev){
   console.log(ev);
};
```

### 内置特效

#### 基本特效

hide(隐藏) | show(显示) | toggle(隐藏显示来回切换)，盒模型的尺寸全部变化

```javascript
//基本特效
//隐藏hide
$('#hide').click(function () {
   //$('#box').hide('fast');//快速
   //$('#box').hide('nomal');//正常速
   //$('#box').hide('slow');//慢速
   $('#box').hide(4000, function () {
      console.log('隐藏动画完成了');
   });//自定义速度，第一个参数为特效完成的时间，第二个参数为动画完成后的回调函数
});
//显示show
$('#show').click(function () {
   $('#box').show('nomal', function () {
      console.log('显示动画完成了');
   });
});
//隐藏显示来回切换toggle
var n = 0;
$('#toggle').click(function () {
    $('#box').toggle('nomal', function () {
        var re = n++ % 2;
        //console.log(n++ % 2);

        if (re === 0) {
            console.log('隐藏动画结束了');
        } else {
            console.log('显示动画结束了');
        }
    });
});
```

#### 滑动特效

slideUp(滑动隐藏) | slideDown(滑动显示) | slideToggle(滑动显示/隐藏相互切换)

```javascript
//滑动特效
$('#slideUp').click(function () {
   $('#box').slideUp(4000, function () {
      console.log('滑动隐藏动画结束了');
   });
});
$('#slideDown').click(function () {
   $('#box').slideDown('nomal', function () {
      console.log('滑动显示动画结束了');
   });
});

var n = 0;
$('#slideToggle').click(function () {
   $('#box').slideToggle('nomal', function () {
      var re = n++ % 2;
      //console.log(n++ % 2);

      if (re == 0) {
         console.log('滑动隐藏动画结束了');
      } else {
         console.log('滑动显示动画结束了');
      }
   });
});
```

#### 渐变特效

fadeOut(淡出隐藏) | fadeIn(淡入显示) | fadeToggle(淡入/淡出显示/隐藏切换) | fadeTo(改变透明度)

opacity变化

```javascript
//渐变特效
$('#fadeOut').click(function () {
   $('#box').fadeOut('slow', function () {
      console.log('淡出隐藏动画结束了');
   });
});
$('#fadeIn').click(function () {
   $('#box').fadeIn('slow', function () {
      console.log('淡入显示动画结束了');
   });
});

var n = 0;
$('#fadeToggle').click(function () {
   $('#box').fadeToggle('nomal', function () {
      var re = n++ % 2;
      //console.log(n++ % 2);

      if (re == 0) {
         console.log('淡出隐藏动画结束了');
      } else {
         console.log('淡入显示动画结束了');
      }
   });
});

$('#fadeTo').click(function () {
   $('#box').fadeTo('nomal', 0.5, function () {
      console.log('透明度变化完成了');
   });
})
```

#### 自定义特效

animate()，传入三个参数，第一个参数为所要变化的属性，第二个参数为变化的速度，可以用时间长度来进行替代，第三个参数为回调函数

toggle可用作属性的值，表示元素从当前属性值到0的来回变换

```javascript
//自定义
$('#animate').click(function () {
   $('#box').animate({
      width: 200,
      left: '+=50',
      height: 'toggle',//来回切换
      'border-radius': 50
   }, 500, function () {
      console.log('运动结束了');
   });
});
//链式操作
$('#animate').click(function () {
    $('#box').animate({ width: 200 }, 'fast')
        .delay(2000)	//让后面的动画延迟执行
        .animate({ height: 200 }, 'slow')
        .delay(1000)
        .animate({ opacity: 0.5 }, 1000);
});
```

#### 控制动画

stop()，暂停动画

finish()，完成动画   

delay(2000)，延迟动画，常用在链式操作

```javascript
//控制动画
$('#stop').click(function () {
   $('#box').stop();
});
$('#finish').click(function () {
   $('#box').finish();
});
```

### Ajax

后端要求请求方式为什么，前端在请求过程中就必须使用什么请求方式

#### get请求

$.get(url, data, function(data){}, dataType)请求有四个参数，第一个参数为所要请求页面的url，第二个参数为发送给服务器的字符串或键值对，第三个参数为请求成功后要执行的回调函数，其参数为data等，第四个参数为预期的返回数据类型，一般为（xml、json、script或html）

```javascript
//get请求
$('#get').click(function () {
   $.get('http://api.duyiedu.com/api/student/findAll', { appkey: 'kaivon_1574822824764' }, function (data) {
      console.log(data);
   }, 'json');
});
```

#### ajaxGet请求

$.ajax({url, type, data: {}, dataType, success: function(data){}})

所要传入的参数通过对象的形式进行传入，其中参数包括url、请求类型（可用对象和字符串两种类型来完成）、要向服务器发送的字串、预期数据类型和回调函数

```javascript
$('#ajaxGet').click(function () {
   $.ajax({
      url: 'http://api.duyiedu.com/api/student/findAll',
      type: 'get',
      /* data:{
         appkey:'kaivon_1574822824764',
      }, */
      data: 'appkey=kaivon_1574822824764',
      dataType: 'json',
      success: function (data) {
         console.log(data);
      }
   });
});
```

#### Post请求

$.post(url, data: {}, function(data){}, datatype(如'json'))

所需参数同get请求基本一致，当用于页面登录时，向服务器发送的字串还需要传入账号和密码

```javascript
//post请求
$('#loginBtn1').click(function () {
   var username = $('#login input[name=account]').val();//账号输入框中的内容
   var password = $('#login input[name=password]').val();//密码输入框中的内容

   $.post('http://api.duyiedu.com/api/student/stuLogin', {appkey: 'kaivon_1574822824764', account: username, password: password}, function (data) {
      console.log(data);
   }, 'json');

   //console.log(username,password);
});
```

#### Ajaxpost请求

$.ajax({url, type, data:{}, dataType, success: function(data){}, error: function(status){}})

```javascript
$('#loginBtn2').click(function () {
   $.ajax({
      url: 'http://api.duyiedu.com/api/student/stuLogin',
      type: 'post',
      data: {
         appkey: 'kaivon_1574822824764',
         account: $('#login input[name=account]').val(),
         password: $('#login input[name=password]').val()
      },
      dataType: 'json',
      success: function (data) {
         console.log(data);
      },
      error: function (status) {
         console.log('错误原因：' + status);
      }
   });
});
```

#### JSON请求

$.getJSON()所传参数列表不再需要传入预期数据类型

```javascript
$('#json').click(function () {
   $.getJSON('http://api.duyiedu.com/api/student/findAll', { appkey: 'kaivon_1574822824764' },function(data){
      console.log(data);
   });
});
```

### JQuery插件

#### JSONP

JSONP：Json Width Padding

在跨域过程中，jsonp发送请求是通过script的方式进行发送请求的，而script标签、带有src或带有href等标签的请求方式只能是get方式，不能是post方式，因此jsonp只能接受get请求方式，若请求页面与目标页面同源，则可用post请求

```javascript
$('#jsonp').click(function(){
    $.ajax({
        url: 'http://developer.duyiedu.com/edu/testJsonp',
        dataType: 'jsonp',
        success: function (data){
            console.log(data);
        },
    })
})
```

Jsonp原理

```javascript
var jp = {
    ajax: function(options){
        //实际操作过程中需判断用户传入的参数中是否包含这些数据
        var url = options.url;
        var dataType = options.dataType;//请求方式

        var targetProtocol = '';//用户地址的协议
        var targetHost = '';//用户地址的域名和端口

        if (url.indexOf('http://') === 0 || url.indexOf('https://') === 0){
            //若此条件成立，说明用户传入的地址为绝对地址
            var targeturl = new URL(url);//把用户传进来的字符串地址解析为真正的地址对象
            targetProtocol = targeturl.protocol;
            targetHost = targeturl.host;
        }else{
            //代码走到这里的话，说明用户传入的地址为相对地址
            targetProtocol = location.protocol;
            targetHost = location.host;
        }
        if (dataType !== 'jsonp'){
            //这个条件成立说明用户的请求方式不是jsonp，那就直接返回
            return;
        }
        if (location.protocol === targetProtocol && location.host === targetHost){
            //这个条件成立，说明现在是同域，jsonp会当作普通的ajax做请求
            //...
        }else{
            var callback = 'cb' + Math.floor(Math.random() * 1000000) + '_' + (new Date()).getTime(); //随机生成一个函数名

            //将生成的方法定义为window身上
            window[callback] = options.success;

            //生成script标签
            var script = document.createElement('script');
            if (url.indexOf('?') > 0){
                //这个条件成立，说明用户传的url里面有参数（带问好了）
                script.src = url + 'callback=' + callback;
            }else{
                script.src = url + '?callback=' + callback;
            }
            script.id = callback;
            document.head.appendChild(script);
        }
    }
}
jp.ajax({
    url: 'http://developer.duyiedu.com/edu/testJsonp',
    dataType: 'jsonp',
    success: function(data){
        console.log(data);
    }
})
```

####  官方网站插件

1、https://plugins.jquery.com

2、https://jq22.com

3、https://alvarotrigo.com/fullPage/zh

https://github.com/alvarotrigo/fullPage.js/tree/master/lang/chinese#fullpagejs

动画插件案例

通过参数配置来完成插件特效的应用

文件位于F:\Webstromfiles\JS工具库\JQuery\8-JQuery插件

```javascript
<script src="js/jquery-3.4.1.js"></script>
<script src="js/fullpage.js"></script>
<script>
   $('#fullpage').fullpage({
      sectionsColor:['#693AB8','#a74a2a','#48B7DB','#b32e37'], //配置块的颜色
      navigation:true,
      navigationTooltips:['第一屏','第二屏','第三屏','第四屏'],
      anchors:['fist','second','third','fourth'],
      menu:'#menu'
   });
</script>
```

### 自定义插件

1、给jquery对象本身扩展方法

$.extend()

```javascript
//给jquery对象本身扩展方法
$.extend({
    lg: function(value){
        console.log(value);
    }
});
```

2、给jquery DOM对象扩展方法

$.fn.extend()

若要实现链式操作，需在扩展的方法最后返回$(this)

```javascript
    //给jquery DOM对象扩展方法
    $.fn.extend({
        changeColor: function (){
            $(this).css('color', 'red');
            return $(this);
        }
    });
    $.fn.changeSize = function(){
        $(this).css('fontSize', 100);
        return $(this);
    };
    $('h1').changeColor().changeSize();
```

自定义拖拽插件

```javascript
//封装拖拽的插件
$.fn.draggable = function(options){
    options = options || {};
    options.limit = options.limit || false;
    var This = $(this);

    //修改DOM元素的样式
    $(this).css({
        position: 'absolute',
        left: 0,
        top: 0,
        cursor: 'move',
    })

    $(this).mousedown(function(ev){
        var disX = ev.pageX - $(this).offset().left,
            disY = ev.pageY - $(this).offset().top;

        $(document).mousemove(function(ev){
            var l = ev.pageX - disX,
                t = ev.pageY - disY;
            //如果用户穿了limit为true这个参数，就要处理拖拽范围了
            if (options.limit){
                if (l < 0){
                    l = 0;
                }else if(l > $(document).innerWidth() - This.outerWidth()){
                    l = $(document).innerWidth() - This.outerWidth();
                }
            }
            if (options.limit){
                if (t < 0){
                    t = 0;
                }else if(t > $(document).innerHeight() - This.outerHeight()){
                    t = $(document).innerHeight() - This.outerHeight();
                }
            }
            This.css({
                left: l,
                top: t
            })
        })
        $(document).mouseup(function(){
            $(this).off();//解除事件
        });

        return false;
    });
};

$('#box').draggable({
    limit: true,
});
```

## 三、lodash

中文网站：http://lodash.think2011.net/

lodash用_表示

### Array方法

chunk()，将数组拆分成一个二维数组，拆分后的第一个数组的长度为第二个参数的值

```javascript
console.log(_.chunk(['a', 'b', 'c', 'd'], 2));
//[["a", "b"],["c", "d"]]
```

compact()，过滤数组里的非真数据（即转布尔值后为false的数据被过滤掉）

```javascript
console.log(_.compact([0, 1, false, 2, '', 3, null, NaN, undefined])); 
//[1, 2, 3]
```

concat()，连接两个数组

```javascript
console.log(_.concat(['a', 'b'], ['d', 3]));
//['a', 'b', 'd', 3]
```

difference()，在第一个数组中把第二个数组中的数据都排除掉

```javascript
console.log(_.difference([1, 3, 5, 7, 9], [3, 7])); 
//[1, 5, 9]
```

differenceBy()，与上面的方法一样，只不过它可以再接收一个迭代器的函数作为参数，先对两个数组中的所有元素进行迭代，然后在进行排除，注意其排除结果中的数据为未经过迭代器的元数据

```javascript
console.log(_.differenceBy([3.1, 2.2, 1.3], [4.4, 2.5], Math.floor));   //[3.1, 1.3],上述代码先进行向下取整，再进行排除
```

differenceWith()，与上面的方法一样，只不过它可以接收一个比较器的函数作为参数，对每个数据都要比较一下

```javascript
var objects = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }];
console.log(_.differenceWith(objects, [{ 'x': 1, 'y': 2 }], _.isEqual));
//[{ 'x': 2, 'y': 1 }]
```

drop()，切掉数组的前n（第二个参数，默认为1）位

```javascript
console.log(_.drop(['a', 'b', 'c', 'd', 'e'], 2));  
//['c', 'd', 'e']
```

dropRight()，切掉数组的后n位

```javascript
console.log(_.dropRight(['a', 'b', 'c', 'd', 'e'], 2));  
//['a', 'b', 'c']
```

dropWhile()，用传入的第二个参数判断数组中每个元素的布尔值，去掉从开始到布尔值为false元素的前一位，与Array对象身上的filter()方法一样，dropRightWhile()，从右开始

```javascript
_.dropWhile([1, 2, 3, 4, 5], function(0){return o < 3})
//["3", "4", "5"]
```

take()，提取数组的前n（第二个参数，默认位1）位，与drop方法相反

```javascript
console.log(_.take(['a', 'b', 'c', 'd', 'e'], 2));
//["a", "b"]
```

takeRight()/takeWhile()/takeRightWhile()与前面的方法一样

flatten()，减少一级数组嵌套深度，与Array的flat()方法相似

```javascript
console.log(_.flatten(['a', ['b', ['c', ['d']]]]));
//["a", "b", ["c", ["d"]]]
```

flattenDeep()，把数组递归为一维数组。相当于[].flat(Infinity)

```javascript
console.log(_.flattenDeep(['a', ['b', ['c', ['d']]]])); 
//["a", "b", "c", "d"]
```

flattenDepth()，减少n（第二个参数）层数组的嵌套，相当于[].flat(2)

```javascript
console.log(_.flattenDepth(['a', ['b', ['c', ['d']]]], 2)); 
//["a", "b", "c", ["d"]]
```

fromPairs()，把数组转换为一个对象，与Object.fromEntries()方法一样

```javascript
console.log(_.fromPairs([["a", 1], ["b", 2]]))
//{a:1, b:2}
```

head()/first()，获取数组中的第一位元素，就是取下表为0的那个数据

last()，获取数组中的最后一位元素，取下标为length-1的那个数据

indexOf()，查找数据，并返回数据对应的索引值，与Array对象身上的indexOf方法一样

lastIndexOf()，从右边开始查找数据，并返回数据对应的索引值，与Array对象身上的lastIndexOf方法一样

initial()，获取数组里除了最后一位的所有数据，相当于删除数组里的最后一个数据，与Array身上的pop()方法一样，区别在于pop方法回改变原数组，而这个方法不会改变原数组

tail()，获取除了array数组第一个元素意外的全部元素，相当于Array对象身上的shift()，与initial()，相反

intersection()，取数组的交集

```javascript
console.log(_.intersection(['a', 'b'], ['b', 'c'], ['e', 'b']));
//['b']
```

intersectionBy()/intersectionWith()与前面的类似

union()，取数组的并集（合并起来，去掉重复的）

```javascript
console.log(_.union([2], [1, 2]));  
//[2, 1]
```

unionBy()/unionWith()

xor()，删除数组的交集，留下非交集的部分

```javascript
console.log(_.xor(['a', 'b'], ['b', 'c'], ['e', 'b']));
//["a", "c", "e"]
```

join()，将数组转成字符串，这个方法原生的Array对象也有

```javascript
console.log(_.join([1, 2, 3, 4]));
//"1,2,3,4"
```

nth()，取数组里的某个数据，就是通过下表取到某个数据，只不过他的数字可以为负，表示倒着找

```javascript
var array = ['a', 'b', 'c', 'd'];
console.log(
   _.nth(array, 1),   //b
   _.nth(array, -2),  //c
);
```

remove()，根据函数删除原数组里的数据

```javascript
var arr1 = ['a', 'b', 'c', 'd', 'e'];
_.remove(arr1, function (value, index, array) {
   return index > 2;
});
console.log(arr1); 
//["a", "b", "c"]
```

without()，根据给的参数（参数为数据）删除原数组里的对应数据

```javascript
var arr = ['a', 'b', 'c', 'd', 'e'];
console.log(
   _.without(arr, 'b', 'c'),
   arr
);
//["a", "d", "e"]    ["a", "b", "c", "d", "e"]
```

uniq()，数组去重

```javascript
console.log(_.uniq([1, 2, 2, 1]));  
//[1, 2]
```

zip()，把个数组中索引值相同的数据放到一起，组成新数组

```javascript
console.log(_.zip(['小明', '小红', '小刚'], ['男', '女', '男'], [12, 13, 14]));  
//[["小明", "男", 12],["小红", "女", 13],["小刚", "男", 14]]
```

zipObject()，与上面方法一样，区别是它输出的是对象

```javascript
console.log(_.zipObject(['小明', '小红', '小刚'], ['男', '女', '男'], [12, 13, 14]));    
//{小明: "男", 小红: "女", 小刚: "男"}
```

zipWith()，与zip()类似，它可以多传入一个参数，对合并后的数组进行再处理

```javascript
console.log(_.zipWith(["aa", "a1"], ["bb", "b1"], ["cc", "c1"], function(a, b, c){return a + b + c}))
//["aabbcc", "a1b1c1"]
```

unzip()，这个方法与zip相反，把每个数组里索引值一样的数据放在一起

```javascript
console.log(_.unzip([["小明", "男", 12],["小红", "女", 13],["小刚", "男", 14]]));
//[['小明', '小红', '小刚'], ['男', '女', '男'], [12, 13, 14]]
```

unzipWith()，与zipWith()一样，接收了一个迭代器的函数

### Collection

countBy()，按照一定规则统计数量，key循环次数，value为匹配到的数量

```javascript
console.log(_.countBy(['one', 'two', 'three'], 'length'));  
//{3: 2, 5: 1} 按每个字符串的length进行统计，length为3的有两个数据。length为5的有1个数据
```

groupBy()，按照一定规则进行分许，key为循环次数，value为匹配到的数组

```javascript
console.log(_.groupBy(['one', 'two', 'three'], 'length'));
//{3: ["one", "two"], 5: ["three"]}
```

each()/foreach()，循环，与原生Array.forEach一样

eachRight()/forEachRight()，倒着循环

every()，与原生Array.every方法一样

filter()，过滤数组，与Array对象上的filter()方法一样

find()，查找数据，与Array对象上的find()方法一样

findLast()，与上面一样，区别在于它是从右往左查

flatMap()，生成一个扁平化的数组，与原生的flatMap()方法一样

flatMapDeep()，与上面一样，不过它可以递归

flatMapDepth()，与上面一样，它可以递归，并且可以指定递归的深度

includes()，与Array对象上的includes()方法一样

注：以上均可以查看Lodash中文网详细介绍

invokeMap()，使用第二个参数（方法）去处理数组，返回处理后的结果（数组）

```javascript
console.log(
   _.invokeMap([[5, 1, 7], [3, 2, 1]], 'sort'),   //[ [1, 5, 7],[1, 2, 3]]
   _.invokeMap([123, 456], String.prototype.split, ''),   //[["1", "2", "3"],["4", "5", "6"]]
);
```

keyBy()，创建一个对象，里面的key由第二个参数决定，value为原数组里对应的那条数据

```javascript
var array = [
   { 'dir': 'left', 'code': 97 },
   { 'dir': 'right', 'code': 100 }
];
var result = _.keyBy(array, function (o) {
   return String.fromCharCode(o.code);    //key为使用fromCharCode解析后的字符。value为它所在数组里的那条数据
});
console.log(result);
//{
//	a: {'dir': 'left', 'code': 97},
//    b: {'dir': 'right', 'code': 100}
//}
//key为dir，value为key所在原数组里的那条数据
console.log(_.keyBy(array, 'dir'));
//{
//    left: {'dir': 'left', 'code': 97},
//    right: {'dir': 'right', 'code': 100}
//}
```

orderBy()，排序，既能升序又能降序

```javascript
var users = [
   { 'user': 'fred', 'age': 48 },
   { 'user': 'barney', 'age': 34 },
   { 'user': 'fred', 'age': 40 },
   { 'user': 'barney', 'age': 36 }
];
console.log(
   _.orderBy(users, 'age', 'asc'),    //以age属性的值进行升序排序
   _.orderBy(users, 'user', 'desc'),  //以user属性的值进行降序排序
);
//asc为升序，desc为降序
```

sortBy()，排序，只能升序

```javascript
console.log(
   _.sortBy(users, function (o) {
      return o.user;
   }),
);
```

partition()，根据第二个参数把一个数组分拆成一个二维数组

```javascript
var users = [
   { 'user': 'barney', 'age': 36, 'active': false },
   { 'user': 'fred', 'age': 40, 'active': true },
   { 'user': 'pebbles', 'age': 1, 'active': false }
];
console.log(
   _.partition(users, function (o) {  //active为true的放在一起，active为false的放在一起
      return o.active;
   }),
   _.partition(users, { 'age': 1, 'active': false }),//把第二个参数对应的数据放一起，其余的放一起
);
```

reduce()，与原生Array对象上的reduce()方法一样

reduceRight()，与Array对象上的reduceRight()方法一样

reject()，与filter()方法相反，即返回检查为非真的元素、

```javascript
var users = [
   { 'user': 'barney', 'age': 36, 'active': false },
   { 'user': 'fred', 'age': 40, 'active': true }
];
console.log(
   _.reject(users, function (o) {
      return o.active;   //barney
   }),
   _.reject(users, { 'age': 36, 'active': false }),   //fred
   _.reject(users, ['user', 'fred']), //barney
   _.reject(users, 'age'),    //[]
);
```

sample()，从数组中随机取一个数据

```javascript
console.log(_.sample(['a', 'b', 'c', 'd', 'e']));
```

sampleSize()，随机获得n（传入的第二个参数）个随机数据

```javascript
console.log(_.sampleSize(['a', 'b', 'c', 'd', 'e'], 3));
```

shuffle()，随机排序

```javascript
console.log(_.shuffle(['a', 'b', 'c', 'd', 'e']));
```

size()，返回集合长度

```javascript
console.log(
   _.size(['a', 'b', 'c', 'd', 'e']), //5
   _.size({ a: 1, b: 2 }),    //2
   _.size('kaivon'),  //6
);
```

some()，与Array对象上的some()方法一样

### Function

#### 节流

定义：将高频率时间转变为低频率事件

定时器实现思路：当触发事件的时候，我们设置一个定时器，再次触发事件的时候，如果定时器存在，就不执行，直到delay时间后，定时器执行执行函数，并且清空定时器，这样就可以设置下个定时器。当第一次触发事件时，不会立即执行函数，而是在delay秒后才执行。而后再怎么频繁触发事件，也都是每delay时间才执行一次。当最后一次停止触发后，由于定时器的delay延迟，还会执行一次函数。

时间戳实现思路：使用事件戳来计算时间，时间差大于delay则执行

应用场景：鼠标不断点击触发，mousedown（单位时间内只触发一次）

​				  监听滚动事件，比如是否滑倒底部自动加载更多

#### 防抖

定义：将高频率事件只在最后一次触发，即不关心中间过程，只注重结果

实现思路：利用定时器，触发时不断清空上一个定时器，只有当不触发的时候才会执行函数fn

应用场景：search输入框，在输入完之后请求接口

页面滚动特定距离显示

页面resize触发事件

#### 柯里化

定义：柯里化是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数且返回结果的新函数的技术。

特性：提高了代码的合理性，更重的突出一种思想——降低适用范围，提高适用性

​			对于一个已有函数，对其约定好其中的某些参数输入，然后生成一个更好的更符合业务逻辑的函数

​			提高针对性

​			延迟执行（只有在最后一次才执行）

​			固定易变因素

实现思路：利用闭包，只要有参数就保存参数，并返回函数，无参数时则执行

defer()，延迟执行方法

```javascript
_.defer(function(text){
	console.log(text)
}, '第二次事件循环');
console.log('第一次事件循环')
//第一次执行   第二次执行     即defer中的函数会在第二次事件循环时才会执行
```

delay()，延迟一段时间后执行

```javascript
_.delay(function(text){
    console.log(text)
}, 1000, '延迟1秒执行')
```

flip()，对函数参数调用顺序进行反转

```javascript
function fn1(){
    console.log(arguments);
}
fn1 = _.flip(fn1);
fn1(1, 2, 3);
```

negate()，取反

```javascript
function fn2(n){
    return n % 2 == 0;
};
console.log(_.filter([1, 2, 3, 4, 5, 6], _.negate(fn2)))
//1, 3, 5
```

once()，使函数只能执行一次

```javascript
function fn3(){
    console.log('fn3');
}
var newFn3 = _.once(fn3);
newFn3();
newFn3();
//结果这种只打印一次fn3
```

### Lang

castArray()，强制转为数组，直接在外层加上' '

```javascript
console.log(_.castArray('a'))//[a]
```

clone()，克隆，浅拷贝

```javascript
var obj = {
    a:1,
    b:{
        c:2
    }
};
var obj2 = _.clone(obj1)
```

cloneDeep()，深拷贝

cloneWith()，cloneDeepWith()

conformsTo()，检测对象中的属性值是否满足条件

```javascript
var object = {'a': 1, 'b': 2};
console.log(
	_.conformsTo(object, {'b': function(n){return n > 1}})
)
```

eq()，比较是否相同

```javascript
console.log(_.eq(12, 12))//true
console.log(_.eq({a: 1}, {a: 1}))//false
```

gt()，比较两个数字

```javascript
console.log(_.gt(3, 1))//true
```

gte()，比较是否大于等于

lt()，比较是否小于

lte()，比较是否小于等于

isArray()，判断是否为数组

isObject()，判断是否为对象，null返回为false

toArray()，转换为数组

```javascript
_.toArray({a: 1, b: 2}//[1, 2]
```

### Object

at()，可通过键查找对象中的值

defaults()，合并对象，遇到同名键，用前面的覆盖后面的，与assign（用后面的覆盖前面的）相反

findKey()，通过属性值查找对象中对应的key值

forIn()，遍历属性

forOwn()，只能遍历自身的属性

setWith()，

```javascript
console.log(_.setWith({}, '[0][1]', 'a', Object))//Object换成Array最后生成的即为数组
```

has()，检查属性是否为自身属性

invert()，键值对进行反转

```javascript
var object = {'a': 1, 'b': 2, 'c': 1};
console.log(_.invert(Object));//{1: "c", 2: "b"}
```

omit()，删掉在指定键值对

pick()，筛选出指定键值对

result()，若第二个参数的值为函数，则会自动执行这个函数

toPairs()，将实例化对象转为数组

toPairsIn()，将继承的属性也包含在内

unset()，删掉指定属性

update()，对元对象中的值进行逻辑处理并更新

updateWith()，可多添加一个参数来控制输出类型

### String

camelCase()，将字符串转为标准驼峰式

capitalize()，转换为首字母大写，剩下的为小写

endsWith()，检查字符串是否以某个字符结束

escape()，将转义字符转换为html实体字符

unescape()，与escape正好相反

kebabCase()，在字符中间加上-

pad()，将字符串利用指定字符填充到指定长度

padEnd()，与pad类似，在字符串末尾添加

padStart()，与padEnd相反，在字符串前面添加

repeat()，将字符串重复指定次数

replace()，替换字符串，将某个字符串中的指定内容替换为自定义的内容

snakeCase()，用下划线连接字符

startCase()，将字符串中的特殊字符转为空格，空格后的第一个字符转为大写

template()，模板字符串

trim()，去除收尾空格或某些特殊字符

truncate()，对指定字符串进行特殊处理，返回所需部分加...

## 四、mock

**生成随机数据，拦截Ajax请求**

包下载地址：https://github.com/nuysoft/Mock/tree/refactoring/dist

### 生成规则

**生成规则的含义需要依赖属性值的类型才能确定**

1、属性值是字符串String

```javascript
Mock.mock({
    'data1|1-4': '陈学辉',//随机重复1-4次
    'data2|3': '好帅',//固定重复3次
})
```

2、属性值是数字Number

```javascript
console.log(
   Mock.mock({
      'number1|+1': 100, //整数，自动加1并且初始值为100
      'number2|1-100': 12,   //整数，1-100之间的随机数，包括1和100（1=<数字<=100）   12用来确定是数据为数字类型
      'number3|1-100.5': 12, //小数，整数部分为为1-100间随机数，包括1和100；小数部分为固定5位随机数
      'number4|1-100.1-10': 12,  //小数，整数部分为为1-100间随机数，包括1和100；小数部分为1-10个随机数（位数随机，数字也随机）
      'number5|123.1-10': 12,    //数字123后面随机添加1-10位小数
      'number6|123.10': 12,  //数字123后面固定添加10位小数，但小数的值是随机的
   })
);
```

3、属性值是布尔型Boolean

```javascript
console.log(//后边跟true和false一样，与数字12是一个意思
   Mock.mock({
      'b1|1': false, //随机生成一个布尔值，true与false的概率各为一半
      'b2|1-5': true,    //随机生成一个布尔值，值为value的概率是min / (min + max)，值为!value的概率是max / (min + max)
   })
);
```

4、属性值是对象Object

```javascript
console.log(
   Mock.mock({
      'num1|1-3': { a: 10, b: 20, c: 30, d: 40 },    //随机选取对象里1-3个属性
      'num2|2': { a: 10, b: 20, c: 30, d: 40 },  //随机选取对象里2个属性
   })
);
```

5、属性值是数组Array

```javascript
console.log(
   Mock.mock({
      'arr1|1': ['a', 'b', 'c', 'd', 'e'],   //随机选取数组里1个数据，当|后面不为1时，为重复整个数组
      'arr2|1-3': ['a', 'b', 'c', 'd', 'e'], //通过重复属性值生成一个新数组，min<=重复次数<=max
   })
);
```

6、属性值是函数Function

```javascript
console.log(
   Mock.mock({
      'result': function () { return 1 + 2 } //把函数的返回值当作属性的结果
   })
)
```

7、属性值是正则表达式RegExp

```javascript
console.log(
   Mock.mock({
      'reg1': /[a-z][A-Z][0-9]/,
      'reg2': /\w\W\s\S\d\D/,
      'reg3': /\d{5,10}/
   })
)
```

### Mock.Random

var Random = Mock.Random

#### 1、Basics

Random.boolean()，随机一个布尔值

```javascript
var Random = Mock.Random
console.log(
   Random.boolean(),
   Random.boolean(1, 9, true),//  九分之一的概率为true
   Random.boolean(1, 2, false),
);
```

Random.naturan()，随机一个自然数（大于等于0的整数）

```javascript
var Random = Mock.Random
console.log(
   Random.natural(),
   Random.natural(100),//传一个参数为最小值
   Random.natural(0, 50),//传两个参数为二者之间的随机数
);
```

Random.integer()，随机一个整数（包含负数）

```javascript
var Random = Mock.Random
console.log(
   Random.integer(),
   Random.integer(-100),
   Random.integer(-50, 50),
);
```

Random.float()，随机一个小数

```javascript
var Random = Mock.Random
console.log(
   Random.float(),
   Random.float(0),//小数部分为0位
   Random.float(-10, 10),//限制整数部分的范围
   Random.float(-10, 10, 3),//限制整数部分的范围，并限制小数部分位数最少位3位
   Random.float(-10, 10, 2, 5),//限制整数部分的范围，并限制小数部分位数位2-5位
);
```

Random.character()，随机一个字符

```javascript
var Random = Mock.Random
console.log(
   Random.character(),
   Random.character('abc123'),
   Random.character('lower'),
   Random.character('symbol'),
);
//字符池lower：小写a-z   upper：大写A-Z  number：数字   symbol：特殊字符
```

Random.string()，随机一个字符串

```javascript
var Random = Mock.Random
console.log(
   Random.string(),
   Random.string(5),
   Random.string(7, 10),//长度位7-10的随机字符串
   Random.string('symbol', 5),
   Random.string('abc123', 1, 3),
);
```

Random.range()，随机一个整数数据的数组

```javascript
var Random = Mock.Random
console.log(
   Random.range(7),//[0, 1, 2, 3, 4, 5, 6]
   Random.range(3, 7),//[3, 4, 5, 6]
   Random.range(1, 10, 2),//起始值位1，结尾位9，步长为2
);
```

#### 2、Date

Random.date()，随机一个日期

```javascript
console.log(
   Random.date(),
   Random.date('yyyy-MM--dd : HH-m-ss'),
);//yyyy, yy, y, MM, M, dd, d, HH, H, hh, h, mm, m, ss, s, SS, S, A, a, T
```

Random.time()，随机一个时间

```javascript
console.log(
   Random.time(),
   Random.time('A HH:mm:ss:SS'),
);
```

Random.datetime()，随机一个日期+时间

```javascript
console.log(
   Random.datetime(),
);
```

Random.now()，返回当前的日期和时间字符串

```javascript
//week，定到这周的第一天
console.log(
   Random.now(),
   Random.now('minute'),
);
```

#### 3、Image

Random.image()，生成一个随机的图片地址

```javascript
console.log(
   Random.image(),//随机生成一个图片地址
   Random.image('200x100'),//称号用字母x来表示，指定图片的大小
   Random.image('200x100', '#ffcc33', '#FFF', 'png', 'kaivon'),//图片的内容，颜色，大小
);
```

Random.dataImage()，生成一段随机的Base64图片编码

```javascript
console.log(
   //Random.dataImage(),
   Random.dataImage('200x100'),
)
```

#### 4、Color

Random.color()，随机一个16进制的颜色

```javascript
console.log(
   Random.color(),
);
```

Random.hex()，

```javascript
console.log(
   Random.hex(),
);
```

Random.rgb()，随机生成一个rgb格式的颜色

Random.rgba()，随机生成一个rgba格式的颜色

Random.hsl()，随机生成一个hsl格式的颜色（色相，饱和度，亮度）

#### 5、Text

Random.paragraph()，随机生成一段文本

```javascript
console.log(Random.paragraph());
console.log(Random.paragraph(2));//随机生成两句话
console.log(Random.paragraph(1, 3));//随机生成1-3句话
```

Random.cparagraph()，随机生成一段中文文本

```javascript
console.log(Random.cparagraph());
console.log(Random.cparagraph(2));
console.log(Random.cparagraph(1, 3));
```

Random.sentence()，随机生成一个句子，句子首字母大写

```javascript
console.log(Random.sentence());
console.log(Random.sentence(5));//五个单词
console.log(Random.sentence(1, 5));
```

Random.csentence()，随机生成一段中文文本

```javascript
console.log(Random.csentence());
console.log(Random.csentence(5));
console.log(Random.csentence(1, 5));
```

Random.word()，随机生成一个单词

```javascript
console.log(Random.word());
console.log(Random.word(5));
console.log(Random.word(1, 5));
```

Random.cword()，随机生成一个汉字

```javascript
console.log(Random.cword());
console.log(Random.cword(5));//随机生成5个汉字
console.log(Random.cword(1, 5));
console.log(Random.cword('零一二三四五六七八九十', 3));
console.log(Random.cword('零一二三四五六七八九十', 5, 7));
```

Random.title()，随机生成一个标题

```javascript
console.log(Random.title());
console.log(Random.title(3));//规定单词的数量
console.log(Random.title(1, 5));
```

Random.ctitle()，随机生成一个中文标题

#### 6、Name

Random.first()，随机生成一个常见的英文名

Random.last()，随机生成一个常见的英文姓

Random.name()，随机生成一个常见的英文姓名，当参数为true是，即添加一个中间值

```javascript
console.log(Random.name(true)); //是否添加一个中间值
```

Random.cfirst()，随机生成一个常见的中文姓

Random.clast()，随机生成一个常见的中文名

Random.name()，随机生成一个常见的中文姓名

#### 7、Web

Random.url()，随机生成一个URL

```javascript
console.log(Random.url());
console.log(Random.url('http'));   //指定协议
console.log(Random.url('http', 'kaivon.cn'));  //指定域名
```

Random.protocol()，随机生成一个URL协议

（'http'`、`'ftp'`、`'gopher'`、`'mailto'`、`'mid'`、`'cid'`、`'news'`、`'nntp'`、`'prospero'`、`'telnet'`、`'rlogin'`、`'tn3270'`、`'wais'）

Random.domain()，随机生成一个域名

Random.tld()，随机生成一个顶级域名

Random.email()，随机生成一个邮件地址

```javascript
console.log(Random.email());
console.log(Random.email('kaivon.cn'));    //指定@后的域名
```

Random.ip()，随机生成一个IP地址

```javascript
console.log(Random.ip());
```

#### 8、Address

Random.region()，随机生成一个（中国）大区

Random.province()，随机生成一个（中国）省（或直辖市、自治区、特别行政区）

Random.city()，随机生成一个（中国）市

```javascript
console.log(Random.city());
console.log(Random.city(true));    //是否生成所属的省
```

Random.county()，随机生成一个（中国）县

```javascript
console.log(Random.county());
console.log(Random.county(true));  //指示是否生成所属的省、市
```

Random.zip()，随机生成一个邮政编码

#### 9、Helper

帮助类里的方法，共5个

Random.capitalize()，把字符串的第一个字母转换为i大写

Random.upper()，把字符串转换为大写

Random.lower()，把字符串转换为小写

Random.pick()，从数组中随机选取一个元素

Random.shuffle()，打乱数组中元素的顺序

#### 10、Miscellaneous

其他类里的方法，共三个

Random.guid()	，随机生成一个GUID

Random.id()，随机生成一个18为身份证

### 数据占位符

占位符只是在属性值字符串中占个位置，并不出心啊在最终的而属性值中

注意：

1、用@来标识其后的字符串是占位符

2、占位符引用的二十Mock.Random中的方法

3、通过Mock.Random.extend()来扩展自定义占位符

4、占位符也可以引用数据模板中的属性

5、占位符会优先引用数据模板中的属性

6、占位符支持相对路径和绝对路径

```javascript
console.log(
   Mock.mock('@EMAIL'),
   Mock.mock('@CITY(true)'),
   Mock.mock('@cword("陈学辉好帅", 1, 3)'),
);
```

```javascript
//扩展方法
Random.extend({
   constellation: function (date) {
      var constellations = ['白羊座', '金牛座', '双子座', '巨蟹座', '狮子座', '处女座', '天秤座', '天蝎座', '射手座', '摩羯座', '水瓶座', '双鱼座'];
      return this.pick(constellations)
   }
});
console.log(Random.constellation());
console.log(Mock.mock('@constellation'))
```

可视化接口管理平台：https://github.com/YMFE/yapi

## 五、moment

日期处理类库

### 解析

#### moment()

moment()，返回一个日期对象，以当前计算机时间为准

#### moment(String)

moment('2013-02-08')，返回2013年2月8日的日期对象

moment('2013-039')，返回2013年的第39天，即2013年2月8号

moment('2013050')，返回2013年的第50天，即2013年2月19号

moment('2013W065')，返回2013年第6个星期的第5天，W表示星期

moment('2013-02-08T09')，返回2013年2月8号9点（T表示时间）

#### moment(String)带格式

moment("12-25-1995", "MM-DD-YYYY")，根据后边的参数将前边的时间匹配为标准时间

moment("12/25/1995", "LL")，具体格式参考官网令牌http://momentjs.cn/docs/#/parsing/

#### moment(String)多个格式

moment("29-06-1995", ["MM-DD-YYYY", "DD-MM", "DD-MM-YYYY"])，会依次对格式数组中的每一项进行匹配，若成功则停止并返回

#### moment(String) 特殊格式 

moment("2010-01-01T05:06:07", moment.ISO_8601)

#### moment(Object)

moment({ year: 2010, month: 3, day: 5, hour: 15, minute: 10, second: 3, millisecond: 123 })

#### moment(Number)

moment(1318781876406)，该参数为毫秒数

#### unix()

moment.unix(1318781876406 / 1000)，该参数为秒数

#### moment(Date)

moment(new Date(2011, 9, 16))

#### moment(Number[])	

参数为一个数组	[year, month, day, hour, minute, second, millisecond]

moment([2010, 1, 14, 15, 25, 50, 125])，注意月份是从0开始的，所以这里为2月

#### moment(JSONDate) 

moment("/Date(1198908717056-0700)/")，前面的一串数字是时间戳，后面的为时区

#### moment(Moment)

参数为一个moment对象，用于克隆

```javascript
var a=moment([2012]);
var b=moment(a);
console.log(a.valueOf()===b.valueOf());
```

clone()也可以使用clone去克隆

```javascript
var a=moment([2008]);
var b=a.clone();
console.log(a,b);
```

#### utc()

GMT	世界时，格林尼治标准时间 
UTC	协调世界时，世界统一时间、世界标准时间

```javascript
console.log(moment().format()); //GMT //默认为本地当前时间，东八区的时间（+08:00）
console.log(moment.utc().format());    //UTC     //UTC的时间（世界标准时间，位于0时区，时区用Z表示，它与北京时间相差8个小时）
```

#### isValid()

```javascript
console.log(
   moment([2015, 25, 35]).isValid(),  //返回传入参数是否能解析false
   moment([2015, 10, 35]).invalidAt(),    //返回不能解析的位置2
);
```

### 取值/赋值

```javascript
console.log(moment().seconds() === new Date().getSeconds());    //true
console.log(moment.utc().seconds() === new Date().getUTCSeconds());    //true
```

#### millisecond()

```javascript
//millisecond()/milliseconds()  获取或设置毫秒
console.log(moment().millisecond());
console.log(moment().milliseconds());
console.log(moment().millisecond(100).valueOf());//设置
console.log(moment().milliseconds(100).valueOf());
```

```javascript
//second()/seconds()    获取/设置秒
//minute()/minutes()   获取/设置分
//hour()/hours()      获取/设置小时
//date()/dates()      获取/设置日期
//day()/days()       获取/设置星期
console.log(
   moment().day(),    //1
   moment().day('Sunday'),//设置星期的时候可以传入一个星期的英文单词
);
```

#### weekday()

```javascript
//weekday() 根据语言环境获获取/设置星期，根据语言环境获取或设置星期几
moment.locale('zh-cn');    //把当前的语言环境设置为中文
console.log(
   moment().weekday(),    //0    
   moment().weekday(0),   //0    //英文下是周日，中文下是周一
);
```

#### dayOfYear()

```javascript
//dayOfYear()   获取或设置年份的日期（今天是今年的第几天）
console.log(moment().dayOfYear()); //111
console.log(moment().dayOfYear(1));
```

#### week()/weeks()

```javascript
//week()/weeks()    获取或设置年份的星期（当前星期是今年的第几个星期）
console.log(moment().week());  //17
console.log(moment([2021, 4, 20]).week()); //20
```

#### month()/months()

```javascript
//month()/months()  获取或设置月份，设置时范围为0-11，还支持月份名称
console.log(moment().month()); //3
console.log(moment().month('July'));
```

#### quarter()/quarters()

```javascript
//quarter()/quarters()  获取或设置季度
console.log(moment().quarter());   //2 
console.log(moment().quarter(4));
```

#### year()/years()

```javascript
//year()/years()    获取或设置年份
console.log(moment().year());
console.log(moment().year(2088));
```

#### weekYear()

```javascript
//weekYear()
console.log(moment([2020, 0, 1]).weekYear());
console.log(moment([2020, 11, 31]).weekYear());
```

#### weeksInYear()

```javascript
//weeksInYear() 根据语言环境获取当前 moment 年份的周数
console.log(moment().weeksInYear());   //52
```

#### get()

```javascript
//get() 获取日期
console.log(moment().get('year'));
console.log(moment().get('M'));
console.log(moment().get('date'));
```

#### set()

```javascript
//set() 设置日期
console.log(
   moment().set('year', 2030),
   moment().set('month', 8),
   moment().set({
      'year': 2008,
      'month': 7,
      'date': 8
   }),
);
```

#### max()/min()

```javascript
//max() 对比多个日期，返回最大的那个日期
//min()    对比多个日期，返回最小的那个日期
var a = moment('2019-10-15');
var b = moment({ year: 2010, month: 3, date: 5 });
var c = moment([2020, 10, 20]);
console.log(moment.max(a, b, c));  //c
console.log(moment.min(a, b, c));  //b
```

### 操作

注意：moment 是可变的。 调用任何一种操作方法都会改变原始的 moment。

#### add()

```javascript
//add()    增加日期
console.log(moment().add(7, 'days'));//以今天的日期往后加7天
console.log(moment().add(5, 'M'));//以今天的日期往后加5个月。这里第二个参数使用的是快捷键
console.log(moment().add(365, 'days').add(1, 'months'));
console.log(moment().add({ days: 365, months: 1 }));

//注意：如果原始日期中的日期大于最终月份的天数，则跳到最后一天
console.log(moment([2010, 0, 31]).add(1, 'months'));
```

#### subtract()

```javascript
//subtract() 减少日期
console.log(moment().subtract(7, 'days'));
console.log(moment().subtract(1.5, 'months').valueOf() === moment().subtract(2, 'months').valueOf());  //true 如果传小数的话，会被四舍五入
```

#### startOf()

```javascript
//startOf() 把日期设置成参数的开始值
console.log(moment().startOf('year'));//设置成今年第一天
console.log(moment().startOf('month'));//设置成当月第一天
console.log(moment().startOf('hour'));//设置成当前小时的最开始那一秒
console.log(moment().minutes(0).seconds(0).milliseconds(0));
```

#### endOf() 

```javascript
//endOf() 把日期设置成参数的结束值
console.log(moment().endOf('year'));//置成今年的最后一天的最后一刻
console.log(moment().endOf('month'));//设置为当月的最后一天的最后一刻
```

#### local()

```javascript
//local()   在日期上设置个标记，以使用本地时间
var a = moment.utc([2011, 0, 1, 8]);
console.log(a.hours());    //8
a.local();
console.log(a.hours());    //16
```

#### utcOffset()

```javascript
//utcOffset() 获取本地时间与UTC时间的偏移量（差值）以分钟数为单位
console.log(moment().utcOffset()); //480
console.log(moment().utcOffset(10));   //把本地时间与UTC时间的偏移量设置成10，也就是本地时间比UTC时间多10个小时
```

### 显示

#### format()

```javascript
//format()  格式化时间，它的参数非常丰富
console.log(moment().format());    //2020-04-21T11:38:30+08:00
console.log(moment().format('DDDo, W, MMMM Do YYYY, h:mm:ss a - ZZ'));
//令牌参考官网使用

//本地化的格式，它定义了一些常用格式，这些格式会根据语言环境来决定显示的内容
moment.locale('zh-cn');
console.log(moment().format('LLLL'));
```

#### fromNow()

```javascript
//fromNow() 相对于现在的时间
console.log(
   moment([2008]).fromNow(),  //12 年前    2008年相对于今天是12年前的时间
   moment([2008]).fromNow(true),  //12 年
);
```

#### from()

```javascript
//from() 一个时间相对于另一个时间的时间
var a = moment([2007, 0, 15]);
var b = moment([2007, 0, 29]);
console.log(
   a.from(b), //a相对于b是14天前的时间
   a.from(b, true),
);
```

#### toNow()

```javascript
//toNow() 到现在的时间
console.log(
   moment([2008]).toNow(),    //12 年内
   moment([2008]).toNow(true),    //12 年
);
```

#### to()

```javascript
//to()  一个时间到另一个时间的间隔
var a = moment([2007, 0, 15]);
var b = moment([2007, 0, 29]);
console.log(
   a.to(b),   //14 天内    a到b的时间在14天内
   a.to(b, true), //14 天
);
```

#### calendar()

```javascript
//calendar()    返回一个相对于参数（默认为当前时间）的日历时间。最终的结果它会根据两个时间的接近程度来决定。一共定义了6个档（读一下文档）最大的范围限制在一个星期，超过一个星期就会显示为“其它”
console.log(moment().calendar());
console.log(moment().calendar(moment([2020, 3, 15]))); //下星期二11:54 当前的日期为参数的日期的下星期2
console.log(moment().calendar(moment([2020, 3, 20]))); //明天11:56
console.log(moment().calendar(moment([2020, 4, 20]))); //2020/04/21
```

#### diff()

```javascript
//diff()    返回两个时间的差值
var a=moment([2007, 0, 29]);
var b=moment([2007, 0, 28]);
console.log(a.diff(b));    //86400000 默认取两个时间差的毫秒数
console.log(a.diff(b, 'days'));    //1
```

#### valueOf()

```javascript
//valueOf()   返回对象的原始值
console.log(
   moment().valueOf(),
   new Date().valueOf(),
);
```

#### unix()

```javascript
//unix()
console.log(moment().unix());
```

#### daysInMonth()

```javascript
//daysInMonth()    获取某月的天数
console.log(moment().daysInMonth());   //30
console.log(moment('2020-02').daysInMonth());  //29

console.log(moment().toDate());//返回标准时间
console.log(moment().toArray());
console.log(moment().toObject());//toObject()  把日期的各个组成部分拆分成了属性，返回整个对象
```

### 查询

#### isBefore()

```javascript
//isBefore()    检查一个时间是否在另一个时间之前，默认是都转成毫秒数进行计算
console.log(moment('2010-10-20').isBefore());  //true 没给参数默认为现在的时间
console.log(moment('2010-10-20').isBefore('2010-10-19'));  //false    第一个日期是否在第二个日期之前
console.log(moment('2009-10-20').isBefore('2010-10-19', 'year'));  //true    二个参数为对比的单位，可以给的有year month week isoWeek day hour minute second
console.log(moment('2010-10-20').isBefore('2008-12-31', 'month')); //false，当为month时，会比较年和月，其余也一样
```

#### isSame()

```javascript
//isSame()  检查两个时间是否相同
console.log(moment('2010-10-20').isSame('2010-10-20'));
console.log(moment('2010-10-20').isSame('2010-12-20', 'year'));
```

#### isAfter()

```javascript
//isAfter() 检查一个时间是否在另一个时间之后
console.log(moment('2010-10-20').isAfter('2010-09-19'));   //true
```

#### isSameOrBefore()

```javascript
//isSameOrBefore()  检查一个时间是否在另一个时间之前或者与之相同（<=）
console.log(
   moment('2010-10-20').isSameOrBefore('2010-10-21'), //true
   moment('2010-10-20').isSameOrBefore('2010-10-20'), //true
   moment('2010-11-20').isSameOrBefore('2010-10-20'), //false
);
```

#### isSameOrAfter()

```javascript
//isSameOrAfter() 检查一个时间是否在另一个时间之后或者与之相同（>=）
console.log(
   moment('2010-11-20').isSameOrAfter('2010-10-21'),  //true
   moment('2010-10-20').isSameOrAfter('2010-10-20'),  //true
   moment('2010-10-19').isSameOrAfter('2010-10-20'),  //false
);
```

#### isBetween()

```javascript
console.log(
   moment('2010-10-20').isBetween('2010-10-19', '2010-10-25'),    //true
   moment('2010-10-20').isBetween('2010-10-19', undefined),   //true undefined等于moment(),就是当前的时间
   moment('2010-10-20').isBetween('2009-10-19', '2012-01-01', 'year'),    //true

   //第四个参数为包容性，第三个参数为null，表示对比单位为默认毫秒数，即数学中区间的闭合
   moment('2016-10-30').isBetween('2016-10-30', '2016-12-30', null, '()'),    //false
);
```

#### isLeapYear()

```javascript
//isLeapYear()  检测是否为闰年
console.log(
   moment().isLeapYear(), //true
   moment([2019]).isLeapYear(),   //false
);
```

#### isMoment()

```javascript
//isMoment() 检测变量是否为moment对象
console.log(
   moment.isMoment(), //false
   moment.isMoment(new Date()),   //false
   moment.isMoment(moment()), //true
);
```

#### isDate()

```javascript
//isDate()  检测变量是否为原生的Date对象
console.log(
   moment.isDate(),   //false
   moment.isDate(new Date()), //true
   moment.isDate(moment()),   //false
);
```

### 国际化

#### 设置语言环境（全局）

```javascript
//moment.locale('zh-cn');
console.log(moment.locale());  //en //返回当前的语言环境

console.log(
    moment().weekday(0),	//根据语言环境获取或设置（传参）星期几。英文环境为星期天，中文环境为星期一
    moment().format('LLLL'),	//格式化时间，参数为本地化格式。英文环境与中文环境都不同

    moment().month(),
);
```

#### 设置语言环境（局部）

```javascript
var myMoment = moment();
myMoment.locale('ar-dz');

console.log(moment().format('LLLL'));
console.log(myMoment.format('LLLL'));
```

#### months()/weekdays()

```javascript
//months()/weekdays() 
/* moment.locale('ru');
moment.locale('zh-hk'); */
console.log(
   moment.months(),
   moment.monthsShort(),
   moment.weekdays(),
   moment.weekdaysShort(),
   moment.weekdaysMin(),
);
```

#### localeData()

```javascript
//localeData()
console.log(
   moment.localeData(),
   moment.localeData().monthsShort(),
);
```

### 自定义

例子

```javascript
moment.updateLocale('zh-cn', {
   //设置月份名称
   months: '一月_二月_三月_四月_五月_六月_七月_八月_九月_十月_十一月_十二月'.split('_'),

   //设置月分名称的缩写
   monthsShort: '1月_2月_3月_4月_5月_6月_7月_8月_9月_10月_11月_12月'.split('_'),

   //设置星期名称
   weekdays: '星期日_星期一_星期二_星期三_星期四_星期五_星期六'.split('_'),

   //设置星期名称的缩写
   weekdaysShort: '周日_周一_周二_周三_周四_周五_周六'.split('_'),

   //设置星期名称的最小缩写
   weekdaysMin: '日_一_二_三_四_五_六'.split('_'),


   //设置长日期格式，是个对象
   longDateFormat: {
      LT: 'Ah点mm分',
      LTS: 'Ah点m分s秒',
      L: 'YYYY-MM-DD',
      LL: 'YYYY年MMMD日',
      LLL: 'YYYY年MMMD日Ah点mm分',
      LLLL: 'YYYY年MMMD日ddddAh点mm分',
      l: 'YYYY-MM-DD',
      ll: 'YYYY年MMMD日',
      lll: 'YYYY年MMMD日Ah点mm分',
      llll: 'YYYY年MMMD日ddddAh点mm分'
   },

   //设置相对时间，from()与to()的方法返回的值就是从这里取的
   relativeTime: {
      future: '%s内',
      past: '%s前~~~',
      s: '几秒',
      m: '1 分钟',
      mm: '%d 分钟',
      h: '1 小时',
      hh: '%d 小时',
      d: '1 天',
      dd: '%d 天',
      M: '1 个月',
      MM: '%d 个月',
      y: '1 年',
      yy: '%d 年'
   },

   //设置时间段，参数：小时,分钟,大小写
   meridiem: function (hour, minute, isLower) {
      const hm = hour * 100 + minute;
      if (hm < 600) {
         return '凌晨';
      } else if (hm < 900) {
         return '早上';
      } else if (hm < 1130) {
         return '上午';
      } else if (hm < 1230) {
         return '中午';
      } else if (hm < 1800) {
         return '下午@@@@';
      } else {
         return '晚上';
      }
   },

   //设置日历
   calendar: {
      sameDay: function () {
         return this.minutes() === 0 ? '[今天]Ah[点整]' : '[今天]LT';
      },
      nextDay: function () {
         return this.minutes() === 0 ? '[明天]Ah[点整]' : '[明天]LT';
      },
      lastDay: function () {
         return this.minutes() === 0 ? '[昨天]Ah[点整]' : '[昨天]LT';
      },
      nextWeek: function () {
         let startOfWeek, prefix;
         startOfWeek = moment().startOf('week');
         prefix = this.diff(startOfWeek, 'days') >= 7 ? '[下]' : '[本####]';
         return this.minutes() === 0 ? prefix + 'dddAh点整' : prefix + 'dddAh点mm';
      },
      lastWeek: function () {
         let startOfWeek, prefix;
         startOfWeek = moment().startOf('week');
         prefix = this.unix() < startOfWeek.unix() ? '[上]' : '[本]';
         return this.minutes() === 0 ? prefix + 'dddAh点整' : prefix + 'dddAh点mm';
      },
      sameElse: 'LL'
   },
   week: {
      dow: 1,    //星期的第一天是周1
      doy: 4
   }
});

console.log('今天是：' + moment().format('MMMM') + ' ' + moment().format('dddd'));
console.log('今天是：' + moment().format('MMM') + ' ' + moment().format('ddd'));

console.log(moment().format('LLLL'));

console.log(moment([2008]).from());

console.log(moment().calendar(moment([2020, 3, 15])));
```

### 时长

```javascript
console.log(moment.duration());

console.log(
    moment.duration(100),//给一个参数表示为毫秒
    moment.duration(2, 'seconds'),//时长为2s
    moment.duration(3, 'minutes'),//时长为3min
    moment.duration(1, 'M'),

    //参数也可以是一个对象
    moment.duration({
        seconds: 1,
        minutes: 2,
        hours: 3,
        days: 4,
        weeks: 5,
        months: 6,
        years: 7
    }),

    //ASP.NET 风格的时间跨度
    moment.duration('23:59:59'),	//时:分:秒
);
```

#### 方法

```javascript
//clone() 克隆一个时长对象
var d1 = moment.duration();
var d2 = d1.clone();
d1.add(1, 'second');
console.log(d1, d2);

moment.locale('zh-cn');
//humanize()   显示一段时长
console.log(
   moment.duration(1, 'minutes').humanize(),
   moment.duration(24, 'hours').humanize(),
   moment.duration(1, 'minutes').humanize(true),  //1 分钟内
   moment.duration(-1, 'minutes').humanize(true), //1 分钟前
);

//milliseconds()   此方法会计算溢出
//asMilliseconds()
console.log(
   moment.duration(500).milliseconds(),   //500
   moment.duration(1500).milliseconds(),  //500
   moment.duration(15000).milliseconds(), //0
   //moment.duration(1500)

   moment.duration(500).asMilliseconds(), //500
   moment.duration(1500).asMilliseconds(),    //1500
   moment.duration(15000).asMilliseconds(),//15000
);

//seconds()
//asSeconds()
console.log(
   moment.duration(500).seconds(),       //0
   moment.duration(1500).seconds(),   //1
   moment.duration(15000).seconds(),  //15

   moment.duration(500).asSeconds(),  //0.5
   moment.duration(1500).asSeconds(), //1.5
   moment.duration(15000).asSeconds(),//15
);


//add()    增加时长，这个方法可以添加多种类型的参数
var a = moment.duration(1, 'd');   //时长为1天
var b = moment.duration(2, 'd');   //时长为2天
console.log(
   a.add(b).days(),
   moment.duration().add(1, 'd').days()   // 1
);

//subtract()   减少时长
var a = moment.duration(3, 'd');
var b = moment.duration(2, 'd');
console.log(
   a.subtract(b).days(),  //1
   moment.duration(5, 'd').subtract(1, 'd').days()    //4
);

//duration(x.diff(y))  获取两个时长的差值
var a = moment([2018, 10, 21, 10, 05]);
var b = moment([2018, 10, 21, 10, 06]);
console.log(
   moment.duration(b.diff(a)),
);

//as()
console.log(
   moment.duration(1000).as('milliseconds'),//1000
   moment.duration(1000).as('seconds'),//1
)

//get()
var d = moment.duration({
   seconds: 1,
   minutes: 2,
   hours: 3,
   days: 4,
   months: 5,
   years: 6
});
console.log(d);
console.log(
   d.get('seconds'),
   d.get('minutes'),
   d.get('hours'),
   d.get('days'),
   d.get('months'),
   d.get('years'),
)
```

### 日历组件案例

弹性盒模型的子元素（行级元素）也可设置宽高

flex弹性盒模型中，可利用justify-content属性来设置对齐方式

日历制作过程中：用当日创建moment对象，构建当月日历，首先获取当前月总共多少天（moment.daysInMonth()），然后获取当前月第一天为周几（moment.startOf('month').weekday()）来确定当前月日历的位置，

切换月份：在点击切换按钮时，利用today来切换到上个月的当天或者下个月的当天

切换农历：利用calendar库来实现公历和农历的转换