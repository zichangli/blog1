# JavaScript

JS的特点：1、解释性语言，2、单线程

JS三大部分：ECMAScript、BOM、DOM

前端的特点：结构、行为、样式相分离

JavaScript数据动态类弱语言，这里的动态类是指在运行过程中检查数据的类型，而弱语言是指在运行过程中会进行隐式转换

- **弱类型**，意味着你不需要告诉 JavaScript 引擎这个或那个变量是什么数据类型，JavaScript 引擎在运行代码的时候自己会计算出来。
- **动态**，意味着你可以使用同一个变量保存不同类型的数据

## ECMAScript

NaN不等于任何东西，包括自己。

### JS数据类型

（原始值和引用值）其中原始值包括：number, string, boolean, undefined, null，引用值包括：array, object, function

### 逻辑运算符：&&   ||  ！

1、&&（与）：碰到假停，先看第一表达式转换成布尔值的结果，如果结果为真，那么会看第二个表达式转换为布尔值的结果，然后如果两个表达式的话，只看到第二个表达式，就可以返回该表达式的值了。以此类推。在充当判断条件时，可称为并且。

```javascript
var a = 1 && 2;
document.write(a);

2
//undefined, null, NaN, "", 0, false
```

即如果第一个表达式为真，则返回第二个表达式的值，否则返回第一个表达式的值。

2、||（或）：碰到真则停。

```javascript
var num = 0 || 3;
document.write(num);

3
```

3、！（非）：对目标的布尔值取反。

### if语句

基础语法：if(<条件>){######}else if{#####}else{#####}

在使用else if时，<条件>必须互斥。

如：

```javascript
var score = parseInt(window.prompt('input'))
if(score > 90 && score < 100){
	document.write('alibaba');
}else if(score > 80 && score < 90){
    document.write('tencent');
}else{
    document.write('11111');
}
```

### 循环语句

for循环：for(<1>;<跳出条件>;<执行语句>){}其中<1>可放到for前面，<执行语句>可放到{}里。

如：

```javascript
for(var i = 1; i < 10; i ++){
    //省略
}
```

while循环：while(<条件>){<执行条件>}（ 同for循环类似用法）。

如：

```javascript
var i = 0;
while(i < 100){
	if(i % 7 == 0 || i % 10 == 7):
    	document.write(i + " ");
    i ++;
}
```

do···while循环：与while循环类似，不过do...while为先执行再进行条件判断

### 条件语句补充

switch  case：switch(条件){case <判断>}

当执行成功后，后边的语句会继续执行，可用break来跳出循环。

如：

```javascript
var n = 3;
switch(n){
	case 1:
		console.log('a');
        break;
	case 2:
		console.log('b');
	case 3:
		console.log('c');
}
```

break：终止循环。

continue：终止此次循环，执行下次循环。

### 初识引用值

**数组（arr）**：用中括号表示[变量， 变量， 变量······]

长度用arr.length来表示。

**对象（object）**：

如：var obj = {

​		lastName : "111";

}

**typeof：**可用来判断变量的类型，可用来返回number， string， boolean， object， undefined， function。

用法：typeof(变量)或typeof 变量，typeof判断未声明的变量并不会报错，返回undefined

typeof(null)返回object

### 类型转换

类型转换一般没有特别强调都转换为数字，若无法转为数字则转为NaN

除+以外，其余均会把符号两侧的变量转化为数字类型

1、Number(mix)将目标转化为数字类型。

2、parseInt(string， "num")，以目标数字为基底转化为十进制。

3、parseFloat(string)将目标转化为浮点型数字。

4、toString，用法···.toString()，undefined和null不可用。数据.toString(n)， 将num转化为n进制数并转换为字符串。

5、String()，将目标转化为字符串。

6、Boolean()转换位布尔类型。

7、isNaN()，判断数据是否为NaN，执行过程为先对数据进行Number()操作，然后将执行后的数据跟NaN进行对比看是否相同。

### function（函数）

创建方式：

1、function name(形式参数){}；

2、var test = function(形式参数){};又叫函数表达式。

3、var test = function name(形式参数){}; 其中name为test函数的别名，可用test.name进行打印；

调用方法：

test(实际参数)

形参的定义大致同变量的定义（var name）是一个意思。

return：a、终止函数；b、返回值。

str.charAt(num)可用来选择字符串中的某位字符。

str.charcodeAt(num)可以返回字符串中某位字符在ASCII码中的序号，若序号小于等于255则其字节长度为1，否则其字节长度为2。

```javascript
function mul(n) {
    //n的阶乘
    if (n == 0) {
        return 1;
    }
    return n * mul(n - 1);
}

function fb(n) {
    //斐波那契数列
    //fb(n) = fb(n-1) + fb(n-2)
    if (n == 1 || n == 2) {
        return 1;
    }
    return fb(n - 1) + fb(n - 2);
}
```



### 作用域

当出现嵌套函数的时候，内部函数可以访问外部函数，但外部函数不可以访问内部函数。

#### 预编译（JS的特点：单线程、解释性语言）

//函数声明整体提升：即构建的函数会提升到逻辑的最前边

//变量声明提升

1、暗示全局变量：imply global，即如果变量未经声明就赋值，则该变量就为全局对象所有。

2、一切声明的全局变量，都是window的属性。

##### 预编译过程（预编译发生在函数执行的前一刻/同时发生在全局（区别为全局创建的叫GO（Global Object）对象==window，同AO对象一样））

1、创建AO对象（Active Object）（即执行期上下文）

2、找形参和变量声明，值为undefined

3、将实参值和形参值统一

4、在函数体里找函数声明，值赋予函数体

例：

```javascript
function fn(a){
    console.log(a);
    
    var a = 123;
    console.log(a);
    function a(){}
    console.log(a);
    
    var b = function(){}
    
    console.log(b);
    function d(){}
}
fn(1);
//输出为function， 123, 123， function即函数fn中的function a(){}提升到前边
```

#### 作用域精解

AO数据即时的存储空间，在每次函数执行 完成后会被删除

[[scope]]指的就是我们所说的作用域，其中存储了运行期上下文的集合

**查找变量时从作用域链的顶端依次向下查找。**

例：

```javascript
function a(){
	function b(){}
}
var glob = 100;
a();
//当函数执行时，所产生的AO放在作用域链的最顶端，在使用嵌套函数的时候子函数跟父函数使用的是同一个AO，不过在调用子函数的过程中，将AO的引用指向子函数，在子函数执行完后，AO重新指向父函数
```

### 闭包

当内部函数被保存到外部时，会产生闭包，闭包会导致原有作用域链不释放，造成内存泄漏。

例：

```javascript
function a(){
    function b(){
        var bbb = 234;
        console.log(aaa);
    }
    var aaa = 123;
    return b;
}
var glob = 100;
var demo = a();
demo();
```

内存泄漏：即占用内存多了之后，剩余内存减小

闭包的作用：

1、实现公有变量，如：函数累加器

```javascript
function add() {
	var count = 0;

	function demo() {
    	count++;
    	return count;
    }
    return demo;
}
var counter = add();
counter();
```

2、可以做缓存(存储结构)

```javascript
function eater(){
    var food = "";
    var obj = {
        eat : function(){
            console.log("I am eating " + food)
        }
        push : function(myfood){
            food = myfood;
        }
    }
	return obj;
}
var eater1 = eater();

eater1.push('banana');
eater1.eat();
```

3、可以实现封装，属性私有化

```javascript
function Deng(wife){
	var prepareWife = "xiaozhang";
    this.name = wife;
    this.divorce = function(){
        this.wife = prepareWife;
    }
    this.changePrepareWife = function(target){
        prepareWife = target;
    }
    this.sayPrepareWife = function(){
        console.log(prepareWife);
    }
}
var deng = new Deng('xiaohong');
//利用deng.prepareWife无法访问到“xiaozhang”
```

4、模块化开发，防止污染全局变量（命名空间）

闭包在定义过程中不会去访问所要保存的函数体内部的内容，只有在执行的时候才会去访问其函数内部结构。

如：

```javascript
function test(){
    var arr = [];
    for(var i = 0; i < 10; i ++){
        arr[i] = function b(){
            document.write(i + " ");
        }
    }
    return arr;
}
for(var i = 0; i < test().length; i ++){
    test()[i]();
}
```

### 立即执行函数

仅执行一次便释放空间的函数即为立即执行函数。

创建方式

1、（function() {}())

2、（function() {})()

一般建议使用第一种，立即执行函数的原理同（1 + 2）* 3中括号的作用类似，即利用（）的作用来实现执行符号（即函数后边的括号）对函数的调用。

### 对象(Object)

**对象的创建方法：**

1、var obj = {}      plainObject  对象字面量/对象直接量

2、构造函数

​	1) 系统自带的构造函数 Object

​	2) 自定义

​	如：var obj = new Object()；   ------->这里的Object为自定义的构造函数名称

在使用构造函数创建对象的时，也可通过传形参的形式直接给对象的属性进行赋值。

构造函数自定义采用大驼峰式写法：即每个单词的首字母都用大写字母。

如：function Person(){}

```javascript
function Car(){
    this.name = "BMW";
    this.heigth = "1400";
    this.lang = "4900";
    this.weight = 1000;
    this.health = 100;
    this.run = function(){
        this.health --;
    }
}
var car = new Car();
```

对象属性的修改通过obj.\<keyname> = \<newvalue>来进行修改

对象属性的删除可通过delete obj.\<keyname>

在对象内部修改属性值时可用this.\<keyname> = value来实现

**注：在向对象的属性传值时，尽量采用单引号。**

### 构造函数内部原理

1、在函数体最前面隐式的加上this = {}；（即var this = {}）

2、执行this.xxx = xxx；（即this = {xxx: xxx}）

3、隐式的返回this；（即return this）

包装类：使用new创建的数字或字符串、布尔类型数据不仅可当数字或字符串使用，还拥有对象的功能，可以增加属性。

new Number()；new String()；new Boolean()。

### 原型

1、原型是function对象的一个属性，它定义了构造函数制造出的对象的公共祖先。通过该构造函数产生的对象，可以继承该原型的属性和方法，原型也是对象。

原型也是对象即通过<构造函数名>.prototype所打印出的为对象，而用\<构造函数名>.prototype.\<属性名>打印出来的为原型的属性值。

2、利用原型的特点，可以提取公有属性。

3、**注：原型创建后自带constructor属性，其属性值为构造函数名，利用这个属性，可以查看子对象的父类，**

4、**原型中还会自带一个\__proto\__属性，这个属性是用来存储对象的原型，子对象可通过该属性替换父对象，即子对象的\__proto\__指向父对象的prototype，如person.\__proto\__ = \<构造函数名>。**

原型的创建方法，\<构造函数名>.prototype.\<属性名> = “\<属性值>”

也可利用对象赋值方式对原型进行属性赋值

如：Person.prototype = {}

构造函数产生的子对象也可以调用其祖先的原型。

如：

```javascript
Person.prototype.LastName = "Deng";
Person.prototype.say = function(){
    console.log("hehe");
}
function Person(name, age, sex){
    this.name = name;
    this.age = age;
    this.sex = sex;
}
var person = new Person('xuming', 35, 'male');
console.log(preson.LastName);

输出结果：Deng
```

例题：

```javascript
Person.prototype.name = 'sunny';
function Person(){
    
}
var person = new Person();
Person.prototype = {
    name : 'cherry';
}
console.log(person.name);
//输出为sunny
```

```javascript
Person.prototype.name = 'sunny';
function Person(){
    
}
var person = new Person();
Person.prototype.name = 'cherry';
console.log(person.name);
//输出为cherry
```

题解：在1中，person对象创建后通过Person.prototype = {name = 'cherry'}相当于将其原型指向了另外一个空间，而在2中，通过Person.prototype.name = 'cherry'是在原对象的原型属性中进行修改，所以创建的对象也会跟着修改。

其思路同以下代码类似：

```javascript
obj = {name : 'a'}
obj1 = obj;
obj.name = 'b'/obj = {name : 'b'}
```

### 原型链

原型连原型再连原型生成的链式结构即为原型链，通过prototype连接，所有对象的最终原型为Object.prototype

如：

```JavaScript
Grand.prototype.lastName = "deng";
function Grand(){
    
}
var grand = new Grand();
Father.prototype = grand;
function Father(){
    
}
var father = new Father();
Son.prototype = father;
function Son(){
    
}
var son = new Son();
console.log(son.lastName);
```

Object.create(原型)其中原型值<构造函数名>.prototype

并不是所有的对象都来自于Object.prototype()，如Object.create(null)构造的对象就没有原型。

toString()方法详解：num.toString() -->new Number(num).toString()，Number.prototype中自带toString()方法而Number.prototype.\__proto__ = Object.prototype

call/apply

call: 作用为改变this指向，使用：<被调用构造函数名>.call(<对象名>， 参数， 参数··· ···)

可通过其来跨级访问原型中的函数如：Object.prototype.toString.call(123)，其执行过程中调用的不是Number.prototype中的toString()方法，而是Object.prototype中的toString()方法。

如：

```javascript
function Person(name, age){
	this.name = name;
	this.age = age
}
var person = new Person('deng', 100);
var obj = {}
Person.call(obj, 'cheng', 300);
//this == obj   相当于利用别人的方法执行自己的功能
```

call也可通过以下方法使用

```javascript
function Person(name, age, sex){
    this.name = name;
    this.age = age;
    this.sex = sex;
}
function Student(name, age, sex, tel, grade){
    Person.call(this, name, age, sex);
    this.tel = tel;
    this.grade = grade;
}
var student = new Student('Sunny', 123, 'male', 122, 2017);
```

apply: 同call的作用基本一样，

call和apply的区别：（传参列表不同）call需要把实参按照形参的个数传进去，而apply需要传一个arguments，即参数的传递必须是数组（将所有参数构造成一个列表进行参数传递）

例题：

```javascript
var a = 5;
function test(){
    a = 0;
    alert(a);
    alert(this.a);
    var a;
    alert(a);
}
//运行test()和new test()的结果分别是什么？
```

Math.ceil(num)向上取整

Math.floor(num)向下取整

toFixed(n)保留n位有效数字，使用方法：num.toFixed(n)其中n为要保留的位数

JavaScript可正常计算的范围，小数点前16位，小数点后16位

### 继承发展史

1、传统方式：原型链

缺点：过多的继承了没用的属性

 2、借用构造函数（call）

缺点：不能继承借用构造函数的原型

每次构造函数都要多走一个函数

3、共享原型

缺点：不能随便改动自己的原型

例：

```javascript
Father.prototype.LastName = "Deng";
function Father(){
    
}
function Son(){
    
}
Son.prototype = Father.prototype;
```

```javascript
function inherit(Target, Origin){
	Target.prototype = Origin.prototype;
}
inherit(Son, Father);
```

4、圣杯模式

通过一个过渡的构造函数去继承原对象的原型，如：

```javascript
Father.prototype.LastName = "Deng";
function Father(){
    
}
function F(){}
F.prototype = Father.prototype;
function Son(){
    
}
Son.prototype = new F();
```

利用函数进行封装

```javascript
function inherit(Target, Origin){
	function F(){}
	F.prototype = Origin.prototype;
    Target.prototype = new F();
    Target.prototype.constructor = Target;//若没此句，目标函数原型中的constructor指向的为Origin（）
    Target.prototype.uber = Origin.prototype;//可用来查看其真正继承来自谁，即它的超类。
}
```

也可用立即执行函数实现属性的私有化，在下边例子中，在生成闭包的过程总，F被私有化。

```javascript
var inherit = (function (){
	var F = function(){};
    return function(Target, Origin){
        F.prototype = Origin.prototype;
        Target.prototype = new F();
        Target.prototype.constructor = Target;
        Target.prototype.uber = Origin.prototype;
    }
}());
```

### 命名空间

管理变量，防止污染全局，适用于模块化开发。

webpack

例

```javascript
var init = (function(){

    var name = 'abc';

    function callName(){
        console.log(name);
    }

    return function(){
        callName();
    }
}())

init();
```

小知识点：函数在执行完后会默认返回一个undefined

若一个对象中存在多个函数，可通过在每个函数中插入return this来实现连续调用执行的效果，如

```javascript
var obj = {
	smoke : function(){
		console.log("smoking....");
		return this;
	},
    perm : function(){
		console.log("perm....");
		return this;
	},
	drink : function(){
		console.log("drink....");
		return this;
	}
}
obj.smoke().perm().drink();
```

对象属性的访问的两种方式，1、obj.<属性名>; 2、obj['属性名']，2中的中括号内必须时字符串，因此可以通过字符串拼接的形式来实现一些特定的功能

### 对象的枚举

小知识：obj.<属性名>------>obj['属性名']

for in循环遍历对象的属性,这里遍历到的属性名权威字符串类型，故在打印过程中必须使用obj['属性名']

1、for in循环遍历会默认将\__proto__属性也遍历

可通过if（obj.hasOwnProperty(<属性名>)）{}来判断遍历到的属性名是否为自身的方法来跳过原型属性

```javascript
var obj = {
    name : '13',
    age : 123,
    sex : "male",
    height : 100,
    weight : 75,
    __proto__ : {
        lastName : "deng"
    }
}
//1、for in循环遍历对象的属性,这里遍历到的属性名权威字符串类型，故在打印过程中必须使用obj['属性名']
for(prop in obj){
    if(obj.hasOwnProperty(prop)){
        console.log(prop + "  " + typeof(prop) + "   " + obj[prop]);
    }  
}
```

2、in，“属性名” in obj，在判断过程中，其原型的属性也被判定时对象自身的属性（使用很少）

3、instanceof，判断A对象是不是B对象构造出来的

A instanceof B，判断A的原型链上是否存在B的原型

### 例题讲解

1、参数传递

```javascript
function foo(){
	bar.apply(null, arguments);
}
function bar(){
    console.log(arguments);
}
foo(1, 2, 3, 4, 5);
//结果为1   2   3   4   5
```

2、我是一行文本，需要水平和垂直居中

```html
<div class="didiv">
    这是一行文本，我要居中
</div>
```

```html
<style type="text/css">
    .didiv{
        height: 500px;
        width: 500px;
        background-color: silver;
        display: table-cell;
        padding: 0px;
        text-align: center;
        vertical-align:middle;
    }
</style>
```

3、parseInt(a, b)，以b为基底将a转化为10进制

16进制：1 2 3 4 5 6 7 8 9 a b c d e f（即f代表15，e代表14，d代表13，c代表12，b代表11，a代表10）

4、if括号中的function会转化为表达式，即立即执行函数，在执行过后就找不到f了

```javascript
var x = 1;
if(function f(){}){
	x += typeof f;
}
console.log(x);
//输出1undefined
```

### this

1、预编译过程，this-->window

2、全局作用域里，this-->window

3、call/apply可以改变函数运行时的this指向

4、obj.func()；func()里面的this指向obj

例：

```javascript
var name = "222";
var a = {
    name : "111",
    say : function(){
        console.log(this.name);
    }
}
var fun = a.say;
fun();//222
a.say;//111
var b = {
    name : "333",
    say : function(fun){
        fun();
    }
}
b.say(a.say);//222
b.say = a.say;
b.say();//333
```

### arguments

1、arguments.callee，代表函数的引用,callee位于哪个函数里边，就指向哪个函数

例：

```javascript
var test = (function(n){
	if(n == 1){
        return 1;
    }
    return n * arguments.callee(n-1);
}(10))
```

2、caller，返回函数的执行环境，若函数的执行环境为window，则返回null

小例题：克隆

```javascript
var obj = {
	name : 'abc',
	age : 123,
	sex : 'female'
}
var obj1 = {};
function clone(origin, target){
    for(var prop in origin){
        if(origin.hasOwnProperty(prop)){
        	target[prop] = origin[prop];
    	}
    }
}
clone(obj, obj1);
```

深度克隆

```javascript
//深度克隆
var obj = {
    name : "abc",
    age : 123,
    card : ['visa', 'master'],
    wife : {
        name : "bcd",
        son : {
            name : "aaa"
        }
    }
}
//1、判断是不是原始值 typeof object
//2、判断是数组还是对象 instanceof()  constructor  toString
//3、建立相应的数组或对象
function deepClone(origin, target){
    var target = target || {},
        toStr = Object.prototype.toString,
        arrStr = "[object Array]";
    for(var prop in origin){
        if(origin.hasOwnProperty(prop)){
            if(origin[prop] !== "null" && typeof(origin[prop]) == 'object'){
                if(toStr.call(origin[prop]) == arrStr){
                    target[prop] = [];
                }else{
                    target[prop] = {};
                }
                deepClone(origin[prop], target[prop]);
            }else{
                target[prop] = origin[prop];
            }
        }
    }
    return target;
}
var obj1 = {};
deepClone(obj, obj1);
```

### 三目运算符

基本写法：条件判断？是：否，并且会返回值

### 数组

创建方式：var arr = []；或var arr = new Array(num)

#### 数组常用方法

##### 改变原数组

push，pop，shift，unshift，reverse

1、push，在数组后添加元素

2、pop，剪切数组的最后一位

3、shift，剪切数组第一位

4、unshift，在数组前添加元素

5、reverse，数组反转

6、splice，arr.splice(从第几位开始，截取多少的长度，在切口处添加新的数据)

数组利用负数访问机制：pos += pos > 0 ？ 0 ： this.length；如负数可以访问倒数第一个位置的元素

7，sort()，arr.sort(function(){})可根据各种规则进行排序 

//1.必须写俩形参

//2.看返回值 1）当返回值为负数时，前面的数放在前面

​					 2）为正数，那么后面的数在前

​					 3）为0，不动

即

```javascript
arr.sort(function(a, b){
	if(a > b){
        return 1;
    }else{
        return -1;
    }
})
```

##### 不改变原数组

1、concat，arr1.concat(arr2)，作用，将两个数组进行连接

2、toString，将数组变成字符串

3、slice，arr.slice(num1, num2)，从num1位开始截取，截取到num2位

​					arr.slice(num)，从num位开始截取，一直截取到最后

​					arr.slice()，截取整个数组

4、join，arr.join(str)，将数组的每一位利用传入的参数连接起来返回一个字符串，同时字符串可利用str.split(str)将字符串拆为数组

字符串是以栈内存存储数据，而数组时以散列形式存储数据的，因此在连接字符串时可利用数组先存储所有字符串，然后通过join函数来实现字符串之间的连接。

### 类数组

类数组：属性要为索引属性，必须有length属性，最好加上push方法（调用Array.prototype.push来添加）

```javascript
//Array中的push方法
Array.prototype.push = function(target){
	this[this.length] = target;
	this.length ++;
}
//当对象调用这个push方法时，this替换为obj
```

当给一个对象添加splice方法（Array.prototype.push）后，该对象打印出来同数组基本一致

1、可以利用属性名模拟数组特性

2、可以动态的增长length属性

3、如果强行让类数组调用push()方法，则会根据length属性值的位置进行属性的扩充。

例题：

```javascript
//数组去重 hash
Array.prototype.unique = function(){
    var temp = {},
        arr = [],
        len = this.length;
    for(var i = 0; i < len; i ++){
        if(!temp[this[i]]){
            temp[this[i]] = 'abc';
            arr.push(this[i]);
        }
    }
    return arr;
}
```

复习：原始值没有属性，str.length--->new String(str).length------------------即为包装类

对象通过在访问原型中的属性时，是通过\__proto__来进行访问的

通过var定义的属性较不可配置属性，如var num = 123,   利用delete window.num不可实现

### try······catch

相当于python中的try······except

用法try{}catch(e){console.log(e)}，try里的代码如果有错误的话并不会报错，会自动跳出，即不执行错误后边的代码

如

```javascript
try{
	console.log('a');
    console.log(b);//此处为错误，在执行过程中不会报错，catch会捕捉这个错误然后将这个错误的信息返回给e
    console.log('c');
}catch(e){
    console.log(e.massage + " " + e.name);//e.name为错误类型，e.massage为错误信息
}
```

错误的类型：Error.name的六种值对应的信息：

1、EvalError：eval()的使用与定义不一致

2、RangeError：数值越界

3、ReferenceError：非法或不能识别的引用数值

4、SyntaxError：发生语法解析错误

5、TypeError：操作数类型错误

6、URIError：URI处理函数使用不当

### es5严格模式

即es3.0和es5.0产生冲突的部分

浏览器是基于es3.0和es5.0新增方法使用的

es5.0严格模式，那么es3.0和es5.0产生冲突的部分就使用es5.0，否则会使用es3.0的

es5.0启用方式，在整个页面的最顶端写上”use strict“;

es5.0启用后，不再兼容es3的一些不规则语法。使用全新的es5方法

es5.0严格模式中变量必须声明，arguments.callee caller等方法不可使用，with(){}不可使用

局部this必须被赋值，赋什么就是什么

拒绝重复属性和参数

### eval()

可将字符串当成代码来使用，如var a = 123;    eval("console.log(a);")   //打印123，但在es5严格模式中也禁止使用

### with(obj){}

with可将括号中的obj放到其作用域链的最顶端，但由于修改作用域链会严重降低效率，故在es5.0严格模式中禁止使用with(){}

## DOM

xml -- html -- html5

1、DOM-->Document Object Model

2、DOM定义了表示和修改文档所需的方法。DOM对象即为宿主对象，由浏览器厂商定义，用来操作html和xml功能的一类对象的集合。也有人称DOM是对HTML以及XML的标准编程接口。

小案例

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS学习（下）</title>
    <style>
        .content{
            display: none;
            width: 200px;
            height: 200px;
            border: 2px solid red;
        }
        .active{
            background-color: yellow;
        }
    </style>
</head>
<body>
    <div class="wrapper">
        <button class="active">111</button>
        <button>222</button>
        <button>333</button>
        <div class="content" style="display: block">帅</div>
        <div class="content">酷</div>
        <div class="content">炫</div>
    </div>
    <script type="text/javascript">
        var btn = document.getElementsByTagName('button');
        var div = document.getElementsByClassName('content');
        for(var i = 0; i < btn.length; i++){
            (function(n){
                btn[n].onclick = function(){
                    for(var j = 0; j < btn.length; j ++){
                        btn[j].className = "";
                        div[j].style.display = "none";
                    }
                    this.className = "active";
                    div[n].style.display = "block";
                }
            }(i))
        }
    </script>
</body>
</html>
```

### DOM基本操作（查找）

document代表整个文档

#### 选择元素方法

document.getElementsById()，利用元素id值选择标签

document.getElementsByTagName()，利用标签名选择标签

document.getElementsByName()，如\<input name="111">\</input>，通过name值可以选择input标签

需注意只有部分标签可以加name属性，如表单，表单元素，img，iframe

document.getElementsByClassName()，

.querySelector()，css选择器，可利用css语法进行选择，如var p = document.querySelect("div > span > p.abc")

.querySelectorAll()，css选择器，同querySelector类似，选择所有符合标准的标签，返回一个数组，但querySelector选择器为静态的，并非实时，因此在用法上有很多局限。

如：

```javascript
var div = document.getElementsByTagName("div")[0];
div.style.width = "100px";
div.style.height = "100px";
div.style.backgroundColor = "red";
```

小提示：写网页尽量不用id选择器（如section标签在代码整合过程中会通过id进行抽取，抽取后更换id名称，多个id可能会导致功能失效）

#### 遍历节点树

.parentNode    -->父节点

.childNode       -->子节点们

.firstChild         -->第一个子节点

.lastChild          -->最后一子节点

.nextSibling     -->下一个兄弟节点

.previousSibling ->前一个兄弟节点

#### 基于元素节点的遍历树

parentElement     -->返回当前元素的父节点

children   -->只返回当前元素的元素子节点

firstElementChild

lastElementChild

nextElementSibling/previousElementSibling

#### 节点的四个属性

nodeName，元素的标签名，以大写的形式表示，只读

nodeValue，Text节点或Comment节点的文本内容，可读写

nodeType，该节点的类型，只读

attributes，Element节点的属性集合

每个节点都有一个方法：Node.hasChildNodes()返回布尔值

#### 节点的类型

元素节点                              --------1（nodeType返回值）

属性节点                              --------2

文本节点                              --------3

注释节点                              --------8

document                           --------9

DocumentFragment           --------11

document  -->  HTMLDocument.prototype  -->  Document.prototype

![image-20201119142644237](C:\Users\木豆\AppData\Roaming\Typora\typora-user-images\image-20201119142644237.png)

1、getElementById方法定义在Document.prototype上，即Element节点上不能使用。

2、getElementsByName方法定义在HTMLDocument.prototype上，即非html中的document以外不能使用(xml document,Element)

3、getElementsByTagName方法定义在Document.prototype 和 Element.prototype上

var div = document.getElementsByTagName("*")可选择所有标签

4、HTMLDocument.prototype定义了一些常用的属性，body,head,分别指代HTML文档中的\<body>\<head>标签。即document.body即为\<body>标签，document.head即为\<head>标签

5、Document.prototype上定义了documentElement属性，指代文档的根元素，在HTML文档中，他总是指代\<html>元素（document.documentElement）

6、getElementsByClassName、querySelectorAll、querySelector在Document,Element类中均有定义

### DOM基本操作（增，插，删，替换）

增

1、document.createElement()，创建元素节点

2、document.createTextNode()，创建文本节点

3、document.createComment()，创建注释节点
插

1、PARENTNODE.appendChild()；向节点中插入子节点

2、PARENTNODE.insetBefore(a, b)，把a插入到b前

删

1、parent.removeChild()，删除子节点（谋杀），也可通过var xxx = parent.removeChild()将节点保存下来

2、child.remove()，节点销毁自身（自尽）

替换

parent.replaceChild(new, origin)，用新元素替换老元素，可返回一个节点，即老元素被剪切出来了

### DOM基本操作（Element）

Element节点的一些属性

1、.innerHTML，可读取或修改元素的html内容

2、.innerText，（火狐浏览器不兼容/textContent（老版本IE不好使））读取或修改元素的文本内容，若元素中还有其他节点，修改元素内容时会覆盖掉原有节点，需谨慎使用

Element节点的一些方法

1、.setAttribute(属性名，属性值)，为元素添加属性

2、.getAttribute(属性值)，获取元素属性，

经典例题

1、利用原型链封装实现元素.insertAfter()，原理为通过insertBefore()完成在当前元素后一个元素节点的前面插入该元素

```javascript
<div>
    <i></i>
    <b></b>
    <span></span>
</div>
<script>
    Element.prototype.insertAfter = function(targetNode, afterNode){
        var beforeNode = afterNode.nextElementSibling;
        this.insertBefore(targetNode, beforeNode);
        if(beforeNode == null){
            this.appendChild(targetNode);
        }
    }
    var div = document.getElementsByTagName('div')[0];
    var b = document.getElementsByTagName('b')[0];
    var p = document.createElement('p');
    var span = document.getElementsByTagName('span')
</script>
```

2、标签逆序

从倒数第二个标签开始利用appendChild()方法依次将元素**剪切**到父元素内

```javascript
<div>
    <i></i>
    <b></b>
    <span></span>
</div>
<script>
    var div = document.getElementsByTagName('div')[0];
    len = div.children.length;
    for(var i = len-2; i >= 0; i --){
        div.appendChild(div.children[i]);
    }
</script>
```

### Date对象

Date对象属性：constructor和prototype

Date对象方法：

Date对象创建后记录创建时的时间

1、Date()，返回当前日期和时间

2、getDate()，返回当日是本月的第几天

3、getDay()，返回当日是本周的第几天，周末返回数字为0

4、getMonth()，返回月份（0~11）

5、getFullYear()，返回年份

6、getHours()，返回Date对象的小时（0~23）

7、getMiniutes()，返回Date对象的分钟（0~59）

8、getSeconds()，返回Date对象的秒（0~59）

9、getTime()，返回1970年1月1日（纪元时间）至今的毫秒数，可用来计算时间戳

上述方法均可用set进行使用，如setDate()，可以设置日期，若超过30则推到下个月

定时器

```javascript
<script type="text/javascript">
    var date = new Date();
    date.setHours(17);
    date.setSeconds(0);
    date.setMinutes(0);
    setInterval(function (){
        if(new Date().getTime() - date.getTime() > 1000){
            console.log('现在是下午五点钟');
        }
    },1000)
</script>
```

10、toString()，将date转化为字符串

11、toTimeString()，将时间部分转化为字符串

12、定时器setInterval(function(){}, time)，每隔time时间就执行一次，其中time以毫秒为单位，且在使用自定义time时，定时器只对time识别一次。

**注：定时器不准**

13、clearInterval每个setInterval()都会返回一个数作为自己的特定标识timer，可通过clearInterval(timer)来停止定时器的运行。如：

```javascript
var i = 0;
var timer = setInterval(function(){
	console.log(i++);
    if(i > 10){
        clearInterval(timer);
    }
}, 10)
```

14、setTimeout(function(){},time)，延时运行，即推迟time时间后再执行

15、clearTimeout(timer)，与clearInterval类似，即去掉延迟运行

**全局window上的方法，this指向window**

例子：计时器（记录三分钟后停止）

```javascript
<style type="text/css">
        input{
            border: 1px solid rgba(0,0,0,0.8);
            text-align: right;
            font-size: 15px;
            font-weight: bold;
        }
</style>
minutes:<input type="text" value="0">
seconds:<input type="text" value="0">
<script type="text/javascript">
    var minutesNode = document.getElementsByTagName("input")[0];
    var secondsNode = document.getElementsByTagName("input")[1];
    var minutes = 0,
        seconds = 0;
    timer = setInterval(function (){
        seconds ++;
        if(seconds === 60){
            seconds = 0;
            minutes ++;
        }
        secondsNode.value = seconds;
        minutesNode.value = minutes;
        if(minutes === 3){
            clearInterval(timer);
        }
    }, 1000);
</script>
```

### DOM基本操作（理解，后续使用会封装）

1、查看滚动条的滚动距离，

window.pageXOffset/pageYOffset     IE8及IE8以下不兼容

2、IE8及IE8以下不兼容（a兼容时b必不兼容，b兼容时a必不兼容因此用的过程中通过取两个值相加来解决兼容性混乱的问题）：

a），document.body.scrollLeft/scrollTop

b），document.documentElement.scrollLeft/Top

如（将获取滚动距离封装成一个函数）：

```javascript
function getScrollOffset(){
    if(window.pageXOffset){
        return {
            x : window.pageXOffset,
            y : window.pageYOffset
        }
    }else{
        return {
            x : document.body.scrollLeft + document.documentElement.scrollLeft,
            y : document.body.scrollTop + document.documentElement.scrollTop
        }
    }
}
```

3、查看可视区窗口尺寸

window.innerWidth/window.innerHeight             IE8以及IE8以下不兼容

document.documentElement.clientWidth/clientHeight            标准模式下任意浏览器都兼容

document.body.clientWidth/clientHeight               适用于怪异/混杂模式下的浏览器

例：查看可视区窗口尺寸封装函数

```javascript
function getViewportOffset(){
	if(window.innerWidth){
        return {
            w : window.innerWidth,
            h : window.innerHeight
        }
    }else{
        if(document.compatMode === "BackCompat"){
            return {
                w : document.body.clientWidth,
                h : document.body.clientHeight
            }
        }else{
            return {
                w : document.documentElement.clientWidth,
                h : document.documentElement.clientHeight
            }
        }
    }
}
```

4、查看元素的几何尺寸

domEle.getBoundingClientRect()      兼容性很好，返回结果并不是实时的

5、查看元素的尺寸

dom.offsetWidth/Heigth     获取元素的宽和高

6、查看元素的位置

dom.offsetTop/Left，嵌套div所求的结果为相对于其所在父级元素的位置，而不是整个页面

对于无定位父级的元素，返回相对文档的坐标。对于**有定位**父级的元素，返回相对于最近的有定位的父级的坐标

html中会有一个默认的margin = 8px

dom.offsetParent，返回最近的**有定位**的父级，如无，返回body，body.offsetParent返回null

7、让滚动条滚动

window上有三个方法：window.scroll()，window.scrollTo()，scrollBy()，其中scroll和scrollTo没有任何差别，都是滚动到（x, y）距离，而scrollBy()是相对于当前位置进行滚动，即累加滚动距离

例：自动阅读实现

```javascript
<div class="stop" style="width: 100px; height: 100px; background-color: red;
color: black; font-size: 40px; text-align: center; line-height: 100px;
position: fixed; bottom: 200px; right: 50px; border-radius: 50%; opacity: 0.7;
font-weight: bold">
stop
</div>
<div class="start" style="width: 100px; height: 100px; background-color: chartreuse;
color: black; font-size: 40px; text-align: center; line-height: 100px;
position: fixed; bottom: 320px; right: 50px; border-radius: 50%; opacity: 0.7;
font-weight: bold">
start
</div>
<script>
    var start = document.getElementsByClassName('start')[0];
	var timer = 0;
	var key = true;
	start.onclick = function(){
    	if (key){
        	timer = setInterval(function (){
            	window.scrollBy(0, 5);
        	}, 100)
        	key = false;
    	}
	}
	var stop = document.getElementsByClassName('stop')[0];
	stop.onclick = function(){
    	clearInterval(timer);
    	key = true;
	}
</script>
```

### 脚本化CSS（须记）

1、读写css属性

dom.style.prop

可读写行间样式，没有兼容性问题，碰到样式名称是用“-”连接的，改用小驼峰式，碰到float这样的保留字属性，前面加css，如：dom.style.float应该为dom.style.cssFloat

复合属性必须拆解，**组合单词变成小驼峰式写法**

**写入的值必须是字符串形式**

2、查询计算样式（获取css属性）

window.getComputedStyle(ele, null)，返回的为最终展示样式值，以类数组形式返回，括号中的第二个参数null位置为伪元素

计算样式只读，返回的计算样式的值都是绝对值，没有相对单位，IE8以及IE8以下不兼容

ele.currentStyle，计算样式只读，返回的计算样式的值不知经过转换二等绝对值，**IE独有的属性** 

函数封装(获取css属性)

```javascript
function getStyle(elem, prop){
    if(window.getComputedStyle){
        returm window.getComputedStyle(elem, null)[prop];
    }else{
        return elem.currentStyle[prop];
    }
}
```

window.getComputedStyle(ele, null)可用来查看为元素的样式属性，其中null为伪元素的名称

例子：小方块运动

```javascript
<div style="width: 100px; height: 100px; background-color: red; position: absolute; left: 0; top: 0"></div>
<script type="text/javascript">
    function getStyle(elem, prop){
        if(window.getComputedStyle){
            return window.getComputedStyle(elem, null)[prop];
        }else{
            return elem.currentStyle[prop];
        }
    }
    var div = document.getElementsByTagName('div')[0];
    var speed = 1;
    var timer = setInterval(function(){
        speed += speed/20;
        div.style.left = parseInt(getStyle(div, 'left')) + speed + 'px';
        if (parseInt(div.style.left) > 500){
            clearInterval(timer);
        }
    }, 10)
</script>
```

### 事件

#### 如何绑定事件处理函数

1、ele.onxxx = function(event){}

兼容性很好，但是一个元素的同一个事件上只能绑定一个处理函数

基本等同于写在HTML行间上

2、obj.addEventListener(事件类型, function, false)；第一个参数为事件类型（事件类型以字符串类型传入），第二个参数为事件处理函数

IE9以下不兼容，一个事件可以绑定多个事件处理函数，并且按照绑定顺序依次执行

3、attachEvent('on' + 事件类型, function)

IE独有，一个事件同时可以绑定多个处理程序

例：绑定事件用到for循环时，需考虑闭包问题，可采用立即执行函数来解决

```javascript
<ul>
    <li>a</li>
    <li>a</li>
    <li>a</li>
    <li>a</li>
</ul>
<script>
    var liCol = document.getElementsByTagName('li'),
        len = liCol.length;
    for (var i = 0; i < len; i++){
        (function(i){
            liCol[i].addEventListener('click', function(){
                console.log(i + 1);
            })
        }(i))
    }
</script>
```

#### 事件处理程序的运行环境

1、ele.onxxx = function(event){}

程序this指向的是dom元素本身

2、obj.addEventListener(type, fn, false)

程序this指向的是dom元素本身

3、obj.attachEvent('on' + type, fn)

程序this指向window

封装兼容性的handle()方法

```javascript
function addEvent(elem, type, handle){
    if(elem.addEventListener){
        elem.addEventListener(type, handle, false)
    }else if(elem.attachEvent){
        elem.attachEvent('on' + type, function(){
            handle.call(elem);
        })
    }else{
        elem['on' + type] = handle
    }
}
function handle(){}//handle函数为要绑定的函数
```

#### 解除绑定事件处理函数

1、ele.onclick = null/false

2、ele.removeListener(type, fn, false)

3、ele.detachEvent('on' + type, fn)

若绑定函数为匿名函数，则无法解除

#### 事件处理模型--冒泡、捕获

1、事件冒泡：**结构上（非视觉上）**嵌套关系的元素，会存在事件冒泡的功能，即同一事件，自子元素冒泡向父元素。（自底向上）

如：三个嵌套的div各自绑定自己的事件函数，在执行最内部事件函数时，会自底向上冒泡，分别执行外部事件函数

2、事件捕获： **结构上（非视觉上）**嵌套关系的元素，会存在事件捕获的功能，即同一时间，自父元素捕获至子元素（事件源元素）。（自顶向下）

IE没有捕获事件，事件捕获实现方法，将addEventListener中的false改为true

3、触发顺序，先捕获后冒泡

4、focus，blur，change，submit，reset，select等事件不冒泡

#### 取消冒泡和阻止默认事件

1、取消冒泡

W3C标准“：ele.stopPropagation()，但不支持ie9以下版本

IE独有：event.cancelBubble = true；

封装取消冒泡的函数stopBubble(event)

```javascript
function stopBubble(event){
    if(event.stopPropagation){
        event.stopPropagation();
    }else{
        event.cancelBubble = true;
    }
}
```

2、阻止默认事件

1、return false，以对象属性的方式注册的事件才生效。如：oncontextmenu，右键出菜单方法

```javascript
<script type="text/javascript">
    document.oncontextmenu = function(){
        console.log('a');
        return false;
    }
</script>
```

2、event.preventDefault()，W3C标准，IE9以下不兼容

3、event.returnValue = false，兼容IE

封装阻止默认事件的函数cancelHandle(event)

```javascript
function cancelHandle(event){
    if(e.preventDefault){
        event.preventDefault();
    }else{
        event.returnValue = false;
    }
}
```

\<a>标签中可以直接写入JavaScript代码如

```javascript
<a href="javascript:alert('a')">demo</a>   
<a href="javascript:void('a')">demo</a>     //在void中写入内容相当于写返回值，作用同return一样,可通过void(false)来取消默认事件
<a href="#">demo</a>            //当<a>标签中的地址名称为#时，该标签有一个默认事件为跳转到当前页，可通过void(false)来取消默认事件
```

####  事件对象

event || window.event用于IE

事件源对象：即事件对象中的target属性

#### 事件委托

利用事件冒泡，和事件源对象进行处理

优点：

1、性能   不需要循环所有元素一个个绑定事件

2、灵活   当有新的子元素时不需要重新绑定事件

例子

```javascript
<script>
    var ul = document.getElementsByTagName('ul')[0];
    ul.onclick = function (e){
        var event = e || window.event;
        var target = event.target || event.srcElement;
        console.log(target.innerText);
    }
</script>
```

#### 事件分类

1、鼠标事件

click、mousedown、mouseup、mousemove（鼠标移动事件）、contextmenu（右键产生菜单事件）、mouseover、mouseout、mouseenter、mouseleave

click相当于mousedown加上mouseup

onmouseover为鼠标放到元素上时产生的事件，onmouseout为当鼠标离开元素时产生的事件

HTML5新规范中将onmouseover和onmouseout改名为onmouseenter和onmouseleave

2、用button来区分鼠标的左右键（0/1/2，仅onmousedown和onmouseup可区分）：e.button的值0时为鼠标左键，e.button的值为1时为滚轮，e.button的值为2时为右键，DOM3标准规定：click事件只监听左键

```javascript
<script>
    document.onmousedown = function (e){
        if (e.button === 0){
            console.log('left');
        }else if(e.button === 1){
            console.log('middle');
        }else{
            console.log('right');
        }
    }
</script>
```

例子：解决mousedown和click的冲突（利用点击时长通过key来控制事件的发生）

```javascript
<script type="text/javascript">
    var key = false;
    var firstTime = 0;
    var lastTime = 0;
    document.onmousedown = function (){
        firstTime = new Date().getTime();
    }
    document.onmouseup = function () {
        lastTime = new Date().getTime();
        if(lastTime - firstTime < 300){
            key = true;
        }
    }
    document.onclick = function (){
        if (key){
            console.log('click');
            key = false;
        }
    }
</script>
```

#### 键盘类事件

1、keydown、keypress、keyup

keydown>keypress>keyup

keydown和keypress的区别：keydown可以响应任意按键，keypress只能响应字符类按键

若要监听字符串按键且要区分大小写，用keypress

keypress返回ASCII码，可转为字符串

#### 文本类事件

1、input.oninput，input输入框内文本变化时触发

2、input.onchange，输入框在聚焦和上次失去焦点这两个状态内容有所不同时触发 若输入框内容无变化则不触发

3、onfocus，输入框聚焦点时触发，onblur，输入框失去聚焦点时触发

例：输入框的实现

```javascript
<input type="text" value="请输入关键字" name="SerchKey" class="inp-txt"
       onfocus="if (this.value=='请输入关键字'){this.value='';}"
       onblur="if(this.value==''){this.value='请输入关键字';}">
```

窗体操作类（window上的事件）

scroll，滚动条滚动时触发

load，window.onload = function 页面加载完再执行

### json

 JSON是一种传输数据的格式（以对象为样板，本质上就是对象，但用途有区别，对象就是本地用的，json是用来传输的）

JSON.parse()，string——>json

JSON.stringfy()，json——>string

htmlTree遵循广度优先原则，domTree遵循深度优先原则，cssTree也为深度优先原则

domTree + cssTree = renderTree

domTree或cssTree的改变都会使renderTree改变，

1、reflow 重排，dom节点的删除，添加，dom节点的宽高变化，位置变化，display none——>block，offsetWidth  offsetLeft

2、repaint 重绘，css颜色等的改变，重绘浪费的效率较少 

dom树的完成代表所有节点的解析完毕

### 异步加载JS

js加载的缺点：加载工具方法没必要阻塞文档，过多js加载会影响页面效率，一旦网速不好，那么整个网站将等待js加载而不进行后续渲染工作等。

有些工具方法需要按需加载，用到在加载，不用不加载。

异步加载的三种方案：

1、defer异步加载，但要等到dom文档全部解析完才会被执行，只有IE能用，也可以将代码写到内部。

用法：在script标签中添加defer="defer"如：

```javascript
<script type="text/javascript" defer="defer"></script>
```

2、async异步加载，加载完就执行，async只能加载外部脚本，不能把js写在script标签里。

用法：在script标签中添加async="async"如：

```javascript
<script type="text/javascript" async="async"></script>
```

1和2执行不阻塞页面。

3、**创建script，插入到DOM中，加载完后callBack。**如：为确保加载完script文件后再执行，可利用onload方法，而IE中则通过readyState状态码来解决。

```javascript
<script>
    function loadScript(url, callback){
        var script = document.createElement('script');
        script.type = 'text/javascript';
        if(script.readyState){
            script.onreadyStatechange = function(){
                if(script.readyState == "complete" || script.readyState == "loaded"){
                	callback();//IE中的解决方案
            	}
            }
        }else{
                script.onload = function(){
                    callback();    //test为tool.js中的方法，load事件IE中不兼容
             }
        }
    	script.src = url;
    	document.head.appendChild(script);
     }
	loadScript('demo.js','function(){test();}');
</script>
```

### js加载时间线

**1、创建Document对象，开始解析web页面，解析HTML元素和他们的文本内容后添加Element对象和Text节点到文档中，这个阶段document.readyState = 'loadiing'。**

**2、遇到link外部css，创建线程加载，并继续解析文档。**

**3、遇到script外部js，并且没有设置async、defer，浏览器加载，并阻塞，等待js加载完成并执行该脚本，然后继续解析文档。**

**4、遇到script外部js，并且设置有async、defer，浏览器创建线程加载，并继续解析文档。**

**对于async属性的脚本，脚本加载完成后立即执行，而defer属性的脚本要等待解析完才执行。（异步禁止使用document.write()）**

**5、遇到img等，先正常解析dom结构，然后浏览器异步加载src，并继续解析文档。**

**6、当文档解析完成，document.readyState = 'interactive'。**

**7、文档解析完成后，所有设置有defer的脚本会按照顺序执行。（注意与async的不同，但同样进制使用document.write()）**

**8、document对象触发DOMContentLoaded事件，这也标志着程序执行从同步脚本执行阶段，转化为事件驱动阶段。**

**9、当所有async的脚本加载完成并执行后、img等加载完成后，document.readyState = 'complete'。window对象触发load事件。**

**10、从此，以异步相应方式处理用户输入，网络事件等。**

## 正则表达式（RegExp）

补充：

1、转义字符：\，可将反斜杠后的字符转为文本"ab\\'cd"打印为ab'cd，\n为换行，\t代表table（一个table相当于四个空格），\r为行结束。

2、多行字符串，

3、字符串换行符，\n

正则表达式两种创建方式

1、直接量

如：var reg = /xxxxxx/;再第二个/后可加i/g/m，其中i（ignorCase）为忽视大小写；g为全局匹配；m为多行匹配。

2、new RegExp();

如：var reg = new RegExp("xxx", "i/g/m");

var reg1 = new RegExp(reg);会创建两个相互独立的正则表达式，去掉new的话，reg和reg1就指向同一个正则表达式。

注：推荐直接量

正则表达式的作用：匹配特殊字符或有特殊搭配原则的字符的最佳选择。

正则表达式中的方法：

reg.test();返回Boolean值，表示是否匹配到      str.match();返回匹配到的所有字符串，以数组的形式展示。

1、方括号（表达式，每个方括号代表一个字符）

^放到表达式外表示以什么开头，放到[ ]里表示非，如[abc]查找方括号之间的任何字符, \[^abc]查找任何不在方括号之间的字符，(red|blue|green)查找任何指定的选项。表达式里还可写入元字符

n$表示以某字符结尾

2、元字符（拥有特殊含义的字符）

\w，相当于[0-9A-z_]

\W，相当于\[^\w]

\d，相当于[0-9]

\D，相当于\[^d]

\s，查找空白字符，空白字符可以是（空格符，制表符\t，换行符\n，换页符\f，回车符\r）

\b，单词边界

\B，非单词边界

\v，查找垂直制表符

\uxxxx，匹配Unicode编码字符

.     ，匹配字符串中除了行结束符\n和换行符\r外的所有字符

可利用 [\w\W] 或 [\d\D] 或 [\b\B] 等组合方法来匹配任意字符

3、量词

n+，匹配1-正无穷个某字符

n*，匹配0-正无穷个某字符（正则表达式遵循贪婪原则，匹配不到字符时，会返回一个逻辑空串）

如：

```javascript
<script>
	var reg = /\d*/g;
	var str = "ab12c3d";
	console.log(str.match(reg));
</script>
//返回为["", "", "12", "", "3", "", ""]
```

n？，匹配0-1个某字符

n{X}，匹配x个连续的某字符

n{x, y}，匹配x-y个连续的某字符

n{x, }，匹配x-正无穷个连续某字符

4、RegExp对象属性

global，RegExp对象是否具有标志g

ignoreCase，RegExp对象是否具有i

multiline，是否有多行

source，返回正则表达式源文本

lastIndex，

5、RegExp方法

test，检验一个字符串中是否有与所创建的正则表达式相匹配的部分，返回为Boolean值

exec，与reg.lastIndex配合使用，当进行全局匹配时，游标（lastIndex）随着匹配次数向后推移，如：

```javascript
var reg = /ab/g;
var str = "abababab";
console.log(reg.lastIndex);
console.log(reg.exec(str));
console.log(reg.lastIndex);
//返回为0        lastIndex为0的类数组         2
```

compile，可用来在编译过程中改变和重新编译正则表达式

扩展：()可用作子表达式，如reg = /(\w)\1\1\1/，用来匹配连续的四个0-9A-z的字串，其中\n表示与第n个子表达式中一样的元素。

在使用exec匹配时，返回类数组中会罗列出每个子表达式匹配到的字串

6、string方法

match，全局匹配时，只返回匹配结果，非全局匹配时，返回同exec方法的返回结果类似。

search，若能匹配到，返回匹配到的字串在源字符串中的位置，否则，返回-1

split，拆分字符串，括号中可写入RegExp，当括号中为正则表达式子表达式时，返回数组中同样会返回子表达式中的内容。

replace(str1, str2)，将字符串中的str1元素替换为str2元素，正常情况下只会匹配字符串中的第一个元素，而当str1为全局匹配的正则表达式时，会被全替换掉

如：var str = "aa"; str.replace("a", "b")，打印结果为ba，str.replace(/a/g, "b")，打印结果为bb

```javascript
var reg = /(\w)\1(\w)\2/;
var str = "aabb";
str.replace(reg, "$2$2$1$1")
//打印结果为bbaa，其中$n表示第n个子表达式所匹配到的内容，若要将字符串中的内容替换为$，应写为$$，与\\类似
```

toUpperCase()和toLowerCase()，为字符串大写化和字符串小写化

```javascript
var str = "the-first-name";
var reg = /-(\w)/g;
console.log(str.replace(reg, function($, $1){
	return $1.toUpperCase();
}))
```

正向预查/正向断言，如：

```javascript
var str = "abaaaaa";
var reg = /a(?=b)/g;//匹配任何其后紧接指定字符串n的字符串
console.log(str.match(reg));
//打印为["a"]
var reg = /a(?!b)/g;//匹配任何其后没有紧接指定字符串n的字符串
console.log(str.match(reg));
//打印为["a", "a", "a", "a", "a"]
```

贪婪匹配和非贪婪匹配，如n+，n*，n？，{x, y}，遵循尽可能多的匹配，/n+?/，表达式中的?作用为打破贪婪匹配，即遵循尽可能少的匹配

例题：

```javascript
<script>
    var str = "100000000000";
    var reg = /(\B)(?=(\d{3})+$)/g;
    console.log(str.replace(reg, ","));
</script>
//打印结果为100,000,000,000
```

## BOM(浏览器对象模型)

主要处理浏览器窗口(window)和框架(iframe) ，描述了与浏览器进行交互的方法和接口，可以对浏览器窗口进行访问和操作，不过通常浏览器特定的Javascript扩展都被看做BOM的一部分。扩展如下：

1、弹出新的浏览器窗口

2、移动、关闭浏览器窗口以及调整窗口大小

3、提供Web浏览器详细信息的定位对象

4、提供用户屏幕分辨率详细信息的屏幕对象

5、对cookie的支持

6、IE扩展了BOM，加入了ActiveXObject类，可以通过JavaScript实例化ActiveX对象

### BOM核心-window

window对象是BOM的顶层（核心）对象，玩转BOM，就是玩转window的属性和方法

window对象它具有双重角色，既是通过js访问浏览器窗口的一个接口，又是一个全局对象。这意味着在网页中定义的任何对象，变量和函数，都是window的属性

### BOM和DOM的关系

JavaScript语法的标准化组织是ECMA

DOM的标准化组织是W3C

### BOM的组成

window JavaScript层级中的顶层对象表示浏览器窗口

Navigator包含客户端浏览器信息

History包含了浏览器窗口访问过的URL

Location包含了当前URL的信息

Screen包含客户端显示屏的信息（用处较小）--窗口距离屏幕边框的距离

#### 详解

**注：详情见语雀BOM（https://duyiedu.yuque.com/docs/share/17c3a868-d15a-448f-ad3c-93a7cb554f4f?#WvAW4）**

１、window

window.self/top/parent（以iframe为例，self可以获取当前window，top可获取顶层window，parent可获取父级window）

一些属性具有不可配置性，即只可读，详情可查看权威指南前几章中属性的四大特性

window.prompt('str')，其中的str为输入框（弹框）的提示信息

window.open(url, name, value)，当不写value参数时，新窗口在当前窗口打开，否则打开新窗口

window.close()，只能关闭当前页面打开的页面

２、Navigator

可进行浏览器的嗅探，比如打印浏览器类型及版本等信息（appVersion、userAgent）

cookieEnabled，查看当前网页cookie是否打开

3、History

属性：

length，当前页面历史列表中的URL数量

方法：

back()，加载history列表中前一个URL

forward()，加载history列表中的下一个URL、

go()，加载history列表中的某个具体页面

4、Screen

5、Location（当前网页的协议、域名等信息）

常用属性：

href，设置或返回完整的URL

hash，设置或返回从井号（#）开始的URL（锚）-------在单页面应用中起重要的作用

protocol，设置或返回当前URL的协议

常用方法：

assign()，加载新文档

reload()，重新加载当前文档。参数可选，不填或填false则取浏览器缓存的文档，当参数为true时，则重新从服务器获取

## JS必会常用知识点

### 浏览器的组成

1、用户界面

2、浏览器引擎，

3、渲染引擎

渲染：在电脑绘图中是指用软件从模型生成图像的过程

渲染引擎：其职责就是渲染，即在浏览器窗口中显示所请求的内容

过程：解析html从而构建DOM树->CSS Rule树->构建Render树->绘制Render树

4、网络

5、UI后端

6、Js引擎

7、数据存储

从请求网页到看到整个网页所经历的过程：

1、url   ->   ip

2、tcp

3、返回js  html  css  img

4、js时间线

5、渲染过程   ->   渲染引擎

6、tcp四次挥手

当用户输入url发起请求时，计算机是不认识这个域名的，首先会查看浏览器的缓存，然后查看本机host，如果均找不到，则会访问路由器，如果路由器也不认识，则访问上层路由器，当访问到城市级别的路由器时，即为DNS服务器，DNS服务器又会逐层向上进行访问，直到获取到目标IP地址，DNS服务器返回ip地址后，通过TCP/IP协议三次握手来与目标地址建立连接，返回js、html、css、img等文件，然后按照js加载时间线加载页面并进行渲染，在完成页面选然后，TCP/IP四次挥手结束本次请求

### 属性和特性

系统自定义的某些属性即为特性，如input标签的value/id/type等属性，即为特性

在DOM中获取的对象所有特性都为一一映射，而自定义属性只能通过obj.getAttribute来访问

### 图片预加载和懒加载

预加载原理：采用onload方法来完成，即等待网页全部解析完之后再将图片插入

懒加载：监控滑轮事件，不断判断当前div的位置，采取预加载，把图片正式的添加到页面之中

### Math.random()应用

获得[0, 1)之间的随机数字（左闭又开），在使用过程中可以通过各种运算符的组合来获得自己所需的数值区间

数字取整Math.ceil/floor()

### 文档碎片-虚拟DOM

后续会详细介绍

### 封装byClassName

```javascript
Element.prototype.getElementsByClassName =  Document.prototype.getElementsByClassName = document.getElementsByClassName || function () {
    //获取document下的所有标签
    var allDomArray = document.getElementsByTagName('*');
    var lastDomArray = [];
    function trimSpace (strClass){
        var reg = /\s+/g;
        var newStrClass = strClass.replace(reg, ' ');
        return newStrClass;
    }
    for(var i = 0; i < allDomArray.length; i++){
        var lastStrClass = trimSpace(allDomArray[i].className).trim();
        //trim()去除首尾空格
        var classArray = lastStrClass.split(' ');
        for(var j = 0; j < classArray.length; j++){
            if (classArray[j] == _className){
            	lastDomArray.push(allDomArray);
                break;
            }
        }
    }
    return lastDomArray;
}
```

### 错误调试

debugger增加断点

也可直接在控制台source中直接加入断点进行调试

## JS运动

div.style.left打印出来为带有px的字符串，可通过parseInt(div.style.left)来将其转为数字形式，也可通过div.offsetLeft来直接获取div的左边距数值。

### 匀速运动函数封装

```javascript
function startMove(dom, target) {
    clearInterval(timer);
    var speed = target - dom.offsetLeft > 0 ? 7 : -7;
    timer = setInterval(function() {
        if (Math.abs(target - dom.offsetLeft) < Math.abs(speed)) {
            clearInterval(timer);
            dom.style.left = '300px';
        } else {
            dom.style.left = dom.offsetLeft + speed + 'px';
        }
    }, 30);
}
```

### 缓冲运动

可用来制作页面浮动窗口（如鼠标经过时，元素缓慢移入，鼠标离开，元素缓慢离开，通过onmouseenter和onmouseleave完成两次运动来实现）

```javascript
function startMove(dom, target) {
    clearInterval(timer);
    var iSpeed = 0;
    timer = setInterval(function() {
        iSpeed = (target - dom.offsetLeft) / 6;
        iSpeed = iSpeed > 0 ? Math.ceil(iSpeed) : Math.floor(iSpeed);
        console.log(iSpeed);
        if (oDiv.offsetLeft == target) {
            clearInterval(timer);
        } else {
            oDiv.style.left = oDiv.offsetLeft + iSpeed + 'px';
        }
    }, 30)
}
```

### 通过查询计算样式来实现运动

获取元素当前样式的函数封装：

```javascript
function getStyle(elem, prop){
    if(window.getComputedStyle){
        return window.getComputedStyle(elem, null)[prop];
    }else{
        return elem.currentStyle[prop];
    }
}
```

获取实时位置完成运动：

```javascript
//这里以opacity为例，故传入参数扩大100被，再计算过程中也通过扩大一百倍和缩小一百倍来进行实现
function startMove(dom, target){
    clearInterval(timer);
    var iSpeed = null, iCur = null;
    timer = setInterval(function(){
        iCur = parseFloat(getStyle(dom, 'opacity')) * 100;
        iSpeed = (target - iCur) / 7;
        iSpeed = iSpeed > 0 ? Math.ceil(iSpeed) : Math.floor(iSpeed);
        if(iCur == target){
            clearInterval(timer);
        }else{
            dom.style.opacity = (iCur + iSpeed) / 100;
        }
    }, 30)
}
```

### 多物体运动

为每个元素绑定自己的定时器（通过elem.timer来实现为元素创建自己的定时器），如下demo中，通过为每个元素创建自己的定时器来保证每个元素的定时器不会相互影响。

```javascript
function startMove(dom, target) {
    clearInterval(dom.timer);
    var iSpeed = 0,
        iCur = null;
    dom.timer = setInterval(function() {
        iCur = parseInt(getStyle(dom, 'width'));
        iSpeed = (target - iCur) / 6;
        iSpeed = iSpeed > 0 ? Math.ceil(iSpeed) : Math.floor(iSpeed);
        if (iCur == target) {
            clearInterval(dom.timer);
        } else {
            dom.style.width = iCur + iSpeed + 'px';
        }
    }, 30)
}
```

### 多物体多值运动

通过对多物体运动进行改进可完成同一个函数控制不同种属性的运动，如下demo用一个函数实现对元素的宽、高、边框宽度和透明度四种属性改变的封装

```javascript
var oDivArray = document.getElementsByTagName('div');
var timer = null;
oDivArray[0].onclick = function() {
    startMove(this, 'width', 400);
}

oDivArray[1].onclick = function() {
    startMove(this, 'height', 400);
}

oDivArray[2].onclick = function() {
    startMove(this, 'borderWidth', 20);
}

oDivArray[3].onclick = function() {
    startMove(this, 'opacity', 50);
}

function getStyle(elem, prop) {
    if (window.getComputedStyle) {
        return window.getComputedStyle(elem, null)[prop];
    } else {
        return elem.currentStyle[prop];
    }
}

function startMove(dom, attr, target) {
    clearInterval(dom.timer);
    var iSpeed = 0,
        iCur = null;
    dom.timer = setInterval(function() {
        if (attr == 'opacity') {
            iCur = parseFloat(getStyle(dom, attr)) * 100;
        } else {
            iCur = parseInt(getStyle(dom, attr));
        }
        iSpeed = (target - iCur) / 7;
        iSpeed = iSpeed > 0 ? Math.ceil(iSpeed) : Math.floor(iSpeed);
        if (iCur == target) {
            clearInterval(dom.timer);
        }
        if (attr == 'opacity') {
            dom.style.opacity = (iCur + iSpeed) / 100;
        } else {
            dom.style[attr] = iCur + iSpeed + 'px';
        }
    }, 30)
}
```

### 多物体多值运动+回调机制

所谓回调机制，即在调用函数过程中，将某个函数当成参数传入该函数，待前边所需过程执行完后，再执行传入的函数，可通过（利用逻辑运算符判断第三个参数的类型，若第三个参数的类型函数，则执行）来实现。

```javascript
<script>
    //js div
    var topDiv = document.getElementById('topDiv');
	var botDiv = document.getElementById('bottomDiv');
	topDiv.onclick = function() {
    	startMove(this, {
        	width: 200,
        	height: 200,
        	opacity: 50,
        	marginTop: 200
    	}, function() {
        	startMove(botDiv, {
            	width: 200,
            	height: 200,
            	opacity: 50,
            	marginTop: 200
        	}, function() {
            alert('over');
        })
    })
}

function startMove(dom, attrObj, callback) {
    var key = true;
    var iSpeed = null,
        iCur = null;
    clearInterval(dom.timer);
    if (key) {
        dom.timer = setInterval(function() {
            var bStop = true;
            for (var attr in attrObj) {
                if (attr == 'opacity') {
                    iCur = parseFloat(getStyle(dom, attr)) * 100;
                } else {
                    iCur = parseInt(getStyle(dom, attr));
                }
                iSpeed = (attrObj[attr] - iCur) / 7;
                iSpeed = iSpeed > 0 ? Math.ceil(iSpeed) : Math.floor(iSpeed);
                if (attr == 'opacity') {
                    dom.style.opacity = (iCur + iSpeed) / 100;
                } else {
                    dom.style[attr] = iCur + iSpeed + 'px';
                }
                if (iCur != attrObj[attr]) {
                    bStop = false;
                }
            }
            if (bStop) {
                clearInterval(dom.timer);
                typeof callback == 'function' && callback();
            }
        }, 30)
    }
    if (!key) {

    }
}

function getStyle(elem, prop) {
    if (window.getComputedStyle) {
        return window.getComputedStyle(elem, null)[prop];
    } else {
        return elem.currentStyle[prop];
    }
}
</script>
```

### 加速度运动

1、加速度不变的加速运动

2、加速度不变的减速运动

### 弹性运动

原理：当元素位于目标左侧时，元素的运动为加速度减小的加速运动，加速度方向为右，当元素位于目标右侧时，元素的运动为加速度增大的减速运动，加速度方向向左，随着时间的变化，元素运动的速度逐渐减小，当元素的速度小于某一值时且元素的位置距目标距离小于某一值时，关闭定时器，使元素停止运动。

案例：

基本思路：

1、实现元素到目标点对侧的匀速运动

2、通过a值来实现元素运动速度的变化

3、利用一个系数来控制speed的变化，使speed随时间变化不断减小

4、利用一个条件判断来决定元素的运动何时停止

元素样式代码

```css
<style>
	*{
    	margin: 0;
    	padding: 0;
	}
	ul{
    	position: relative;
    	list-style: none;
    	margin: 100px auto 0;
    	width: 800px;
    	height: 200px;
	}
	.ele{
    	float: left;
    	width: 198px;
    	height: 98px;
    	line-height: 98px;
    	text-align: center;
    	background-color: orange;
    	border: 1px solid black;
	}
	.bg{
    	position: absolute;
    	left: 0;
    	top: 0;
    	width: 200px;
    	height: 100px;
    	opacity: 0.4;
    	background-color: deeppink;
	}
</style>
```

html+js代码

```html
<ul>
    <li class="ele">cst</li>
    <li class="ele">cg</li>
    <li class="ele">dg</li>
    <li class="ele">dcg</li>
    <li class="bg"></li>
</ul>
<script>
    var timer = null;
    var oLiArray = document.getElementsByTagName('li');
    var oLibg = oLiArray[oLiArray.length - 1];
    for (var i = 0; i < oLiArray.length - 1; i ++){
        oLiArray[i].onmouseenter = function(){
            startMove(oLibg, this.offsetLeft);
        }
    }
    function startMove(dom, target){
        clearInterval(timer);
        var speed = 0,
            a = 3;
        timer = setInterval(function (){
            a = (target - dom.offsetLeft)/10;
            speed += a;
            speed *= 0.95
            if(Math.abs(speed) < 1 && Math.abs(target - dom.offsetLeft) < 2){
                clearInterval(timer);
                dom.style.left = target + 'px';
            }
            dom.style.left = dom.offsetLeft + speed + 'px';
        }, 50)
    }
</script>
```

### 模拟重力场

原理：多方向运动+碰撞检测+重力加速度+能量损失

可通过判断元素当前距离窗口边界的距离同最大所需移动距离的大小关系来改变元素的移动方向，最大所需距离（document.documentElement.clientHeight - dom.clientHeight）

自制小demo-**碰撞检测+多方向运动**（html部分为两个嵌套的div，且均为绝对定位），当speed较大时，需考虑元素到达边界时元素的偏移量会超过容器高度或宽度，解决方法为判断元素当前偏移距离，当该距离大于最大所需距离时，将偏移距离设置为最大所需距离并对运动方向进行反向

```javascript
var oDiv = document.getElementById('demo');
var speedX = -1;
var speedY = -1;
oDiv.onclick = function(){
    speedX = 0 - speedX;
    speedY = 0 - speedY;
    startMove(this, speedX, speedY);
}

function startMove(dom, speedX, speedY){
    clearInterval(dom.timer);
    dom.timer = setInterval(function(){
        dom.style.left = parseInt(getStyle(dom, 'left')) + speedX + 'px';
        dom.style.top = parseInt(getStyle(dom, 'top')) + speedY + 'px';
        if(parseInt(getStyle(dom, 'bottom')) === 0 || parseInt(getStyle(dom, 'top')) === 0){
            speedY = 0 - speedY;
        }
        if(parseInt(getStyle(dom, 'left')) === 0 || parseInt(getStyle(dom, 'right')) === 0){
            speedX = 0 - speedX;
        }

    }, 10)
}
function getStyle(elem, prop){
    if(window.getComputedStyle){
        return window.getComputedStyle(elem, null)[prop];
    }else{
        return elem.currentStyle[prop];
    }
}
```

重力场函数（通过g控制y方向上的加速度，在碰到容器壁时，改变运动方向，并通过能量损耗系数来减小元素运动速度的大小）

```javascript
function startMove(dom){
    clearInterval(dom.timer);
    var speedX = 8;
    var speedY = 6;
    var g = 3;
    dom.timer = setInterval(function(){
        speedY += g;
        dom.style.left = parseInt(getStyle(dom, 'left')) + speedX + 'px';
        dom.style.top = parseInt(getStyle(dom, 'top')) + speedY + 'px';
        if(parseInt(getStyle(dom, 'top')) >= document.documentElement.clientHeight - dom.clientHeight){
            dom.style.top = document.documentElement.clientHeight - dom.clientHeight + 'px';
            speedY = 0 - speedY;
            speedY *= 0.8;
            speedX *= 0.8;
        }
        if(parseInt(getStyle(dom, 'top')) < 0){
            dom.style.top = 0 + 'px';
            speedY = 0 - speedY;
            speedY *= 0.8;
            speedX *= 0.8;
        }
        if(parseInt(getStyle(dom, 'left')) >= document.documentElement.clientWidth - dom.clientWidth){
            dom.style.right = document.documentElement.clientWidth - dom.clientWidth + 'px';
            speedX = 0 - speedX;
            speedY *= 0.8;
            speedX *= 0.8;
        }
        if (parseInt(getStyle(dom, 'left')) <= 0){
            dom.style.left = 0 + 'px';
            speedX = 0 - speedX;
            speedY *= 0.8;
            speedX *= 0.8;
        }
        if(Math.abs(speedX) < 1){
            speedX = 0;
        }
        if (Math.abs(speedY) < 1){
            speedY = 0;
        }
        if(speedX === 0 && speedY === 0 && parseInt(getStyle(dom, 'top')) === document.documentElement.clientHeight - dom.clientHeight){
            clearInterval(dom.timer);
            console.log('over');
        }
    }, 30)
}
function getStyle(elem, prop){
    if(window.getComputedStyle){
        return window.getComputedStyle(elem, null)[prop];
    }else{
        return elem.currentStyle[prop];
    }
}
```

### 拖拽实例

效果，通过鼠标拖拽元素，当鼠标拖拽结束后，元素在重力场中运动

拖拽结束元素会有一个初始速度，该速度可通过用两个变量来存储定时器运行过程中元素的所在位置，用这个位置同下次定时器计算出的元素的新位置来求得当前元素的运动速度，在document.onmouseup执行时，将X和Y方向上的速度党曾参数传入函数中，从而实现有初速度的运动效果。

```javascript
var oDiv = document.getElementById('demo');
var lastX = 0,
    lastY = 0;
var speedX = 0,
    speedY = 0;
oDiv.onmousedown = function (e){
    clearInterval(this.timer);
    var event = event || e;
    disX = event.clientX - this.offsetLeft;
    disY = event.clientY - this.offsetTop;
    var self = this;
    document.onmousemove = function(e){
        var event = event || e;
        var newLeft = event.clientX - disX;
        var newTop = event.clientY - disY;
        self.style.left = newLeft + 'px';
        self.style.top = newTop + 'px';
        speedX = newLeft - lastX;
        speedY = newTop - lastY;
        lastX = newLeft;
        lastY = newTop;
    }
    document.onmouseup = function(){
        document.onmouseup = null;
        document.onmousemove = null;
        console.log(speedX, speedY)
        startMove(self, speedX, speedY);
    }
}

function startMove(dom, speedX, speedY){
    clearInterval(dom.timer);
    var g = 3;
    dom.timer = setInterval(function(){
        speedY += g;
        dom.style.left = parseInt(getStyle(dom, 'left')) + speedX + 'px';
        dom.style.top = parseInt(getStyle(dom, 'top')) + speedY + 'px';
        if(parseInt(getStyle(dom, 'top')) >= document.documentElement.clientHeight - dom.clientHeight){
            dom.style.top = document.documentElement.clientHeight - dom.clientHeight + 'px';
            speedY = 0 - speedY;
            speedY *= 0.8;
            speedX *= 0.8;
        }
        if(parseInt(getStyle(dom, 'top')) < 0){
            dom.style.top = 0 + 'px';
            speedY = 0 - speedY;
            speedY *= 0.8;
            speedX *= 0.8;
        }
        if(parseInt(getStyle(dom, 'left')) >= document.documentElement.clientWidth - dom.clientWidth){
            dom.style.left = document.documentElement.clientWidth - dom.clientWidth + 'px';
            speedX = 0 - speedX;
            speedY *= 0.8;
            speedX *= 0.8;
        }
        if (parseInt(getStyle(dom, 'left')) <= 0){
            dom.style.left = 0 + 'px';
            speedX = 0 - speedX;
            speedY *= 0.8;
            speedX *= 0.8;
        }
        if(Math.abs(speedX) < 1){
            speedX = 0;
        }
        if (Math.abs(speedY) < 1){
            speedY = 0;
        }
        if(speedX === 0 && speedY === 0 && parseInt(getStyle(dom, 'top')) === document.documentElement.clientHeight - dom.clientHeight){
            clearInterval(dom.timer);
            console.log('over');
        }else{

        }
    }, 30)
}
function getStyle(elem, prop){
    if(window.getComputedStyle){
        return window.getComputedStyle(elem, null)[prop];
    }else{
        return elem.currentStyle[prop];
    }
}
```

### 轮播图

图片的运动通过之前的缓冲运动来进行实现，若参与运动的图片数量为n，则ul中的li数量应为n+1，其中第一个li和最后一个li中存放的图片应为同一张图片，通过判断ul的当前位置，若整个ul运动到最后一张图片时，将整个ul的left设为0px，即可实现重复运动

csdn链接：https://blog.csdn.net/Bean_s/article/details/112002232

## 数组扩展

### 数组方法补充

1、forEach，定义于Array.prototype，参数为函数，而传入函数需传入三个参数，第一个为当前元素，第二个为当前元素的索引，第三个为arr本身

如：arr.forEach(function(ele, index, self){})

```javascript
var oUl = document.getElementById('oul');
var arr = [
    {name: '李俊奇', src:'', des: '坤', job: 'java'},
    {name: '罗凯', src:'', des: '篮球', job: 'java'},
    {name: '陈旭东', src:'', des: '唱', job: 'web'},
    {name: '刘晓', src:'', des: '跳', job: 'web'},
    {name: '魏嘉楠', src:'', des: 'rap', job: 'web'},
];
function deal(ele, index, self){
    var li = document.createElement('li');
    li.innerText = ele.name;
    oUl.appendChild(li);
}
arr.forEach(deal);
```

forEach还可以传入第二个参数，在forEach函数执行过程中的this即为传入的参数，若只传入一个参数，则函数执行过程中的this指向window。

forEach实现原理

目的：数组实例可以调用该方法，要达到循环遍历的作用

参数：需要一个函数，最后实现我们一系列的功能，函数在执行的时候也会接收参数ele、index和self

```javascript
Array.prototype.myforEach = function(){
    var len = this.length;
    var _this = arguments[1] != undefined ? arguments[1] : window;
    for(var i = 0; i < len; i++){
        //通过apply进行调用，_this作为执行函数中的this，[this[i], i, this]作为参数列表
        arguments[0].apply(_this, [this[i], i, this]);
    }
}
function deal(ele, index, self){
    console.log(ele, index, self, this);
}
```

2、filter，对数组过滤的作用，执行过程是基于遍历的，在遍历过程中，若函数返回值为true则当前遍历元素被留下，否则当前元素被过滤掉

filter参数：函数=>定义ele， index， self，其参数同forEach基本一致

filter执行完以后会返回一个新的数组

```javascript
var newArr = arr.filter(function(ele, index, self){
    //也可直接写为return ele.sex == 'm';
    if(ele.sex == 'm'){
    	return true;
    }else{
        return false;
    }
})
```

filter实现原理

```javascript
Array.prototype.myFilter = function(func){
    var arr = [];
    var len = this.length;
    var _this = arguments[1] || window;
    for(var i = 0; i < len; i++){
        func.apply(_this, [this[i], i, this]) && arr.push(this[i]);
    }
    return arr;
}
```

3、map，映射作用，返回新数组，所需传入参数同filter和forEach一致

传入function中的返回值会作为新数组中对应的值

```javascript
var arr = [
    {name: '李俊奇', src:'', des: '坤', job: 'java'},
    {name: '罗凯', src:'', des: '篮球', job: 'java'},
    {name: '陈旭东', src:'', des: '唱', job: 'web'},
    {name: '刘晓', src:'', des: '跳', job: 'web'},
    {name: '魏嘉楠', src:'', des: 'rap', job: 'web'},
];
var obj = {};
var newArr = arr.map(function(ele, index, self){
    return ele.name;
}, obj)
console.log(newArr)
//返回结果为['李俊奇', '罗凯', '陈旭东', '刘晓', '魏嘉楠']
```

实现原理

```javascript
Array.prototype.myMap = function(func){
    var arr = [];
    var len = this.length;
    var _this = arguments[1] || window;
    for(var i = 0; i < len; i++){
        arr.push(func.call(_this, this[i], i, this));
    }
    return arr;
}
```

4、every，用于判断数组中的元素是否全都符合指定条件，若所有元素都符合指定条件，则返回true，否则返回false

传入参数依然为函数（ele, index, self），同样可传入第二个参数

返回值为true或false，所有元素都符合条件，则返回true，否则返回false

目的：判断数组中的元素是否都符合条件

```javascript
var flag = arr.every(function(ele, index, self){
    if(ele.age > 18){
        return true;
    }else{
        return false;
    }
})
```

实现原理：

```javascript
Array.prototype.myEvery = function(func){
    var flag = true;
    var len = this.length;
    var _this = arguments[1] || window;
    for(var i = 0; i < len; i++){
        if(func.apply(_this, this[i], i, this)){
            flag = false;
            break;
        }
    }
    return flag;
}
```

5、some，同every类似，用于检测数组中的是否有满足指定条件的元素，如果有一个元素满足条件，则返回true，若所有元素都不满足指定条件，则返回false

其实现原理同every类似

```javascript
Array.prototype.myEvery = function(func){
    var flag = false;
    var len = this.length;
    var _this = arguments[1] || window;
    for(var i = 0; i < len; i++){
        if(func.apply(_this, this[i], i, this)){
            flag = true;
            break;
        }
    }
    return flag;
}
```

6、reduce，对数组进行遍历/reduceRight，对数组从右向左遍历

arr.reduce(function(prevValue, icurValue, index, self){}, obj)，其中icurValue即为前边的ele，prevValue的初始值即为reduce的第二个参数，在遍历过程中，prevValue会逐级继承下去，在第二个元素遍历过程中的prevValue值即为第一个元素遍历结束后的prevValue值

```javascript
var arr = [
    {name: '李俊奇', src:'', des: '坤', job: 'java'},
    {name: '罗凯', src:'', des: '篮球', job: 'java'},
    {name: '陈旭东', src:'', des: '唱', job: 'web'},
    {name: '刘晓', src:'', des: '跳', job: 'web'},
    {name: '魏嘉楠', src:'', des: 'rap', job: 'web'},
];
var obj = '';
var str = arr.reduce(function(prevValue, icurValue, index, self){
    prevValue += icurValue.des;
    return prevValue;
}, obj);
console.log(str);
//打印结果为坤篮球唱跳rap
```

```javascript
var cookieStr = 'yuque_ctoken=3ba511D-X2ndU_Iifzot-mUA; lang=zh-cn; UM_distinctid=1761c2177893dc-0361cb0b40821b-c791e37-1fa400-1761c21778adef; CNZZDATA1272061571=2146048705-1606788700-%7C1609389419';
function parseCookieStr(str){
    var obj = {};
    var cookieArr = str.split('; ')
    return cookieArr.reduce(function(prevValue, icurValue, index, self){
        var arr = icurValue.split('=');
        prevValue.arr[0] = arr[1];
        return prevValue;
    }, {});
}
var cookieObj = parseCookieStr(cookieStr);
```

实现原理：

```javascript
Array.prototype.myReduce = function(func, initialValue){
    var len = this.length, _this = arguments[2] || window, nextValue = initialValue;
    for(var i = 0; i < len; i++){
        nextValue = func.apply(_this, [nextValue, this[i], i, this]);
    }
    return nextValue;
}
```

### 数组方法小demo

html由两部分组成，分别为顶部搜索框部分和下面的好友列表部分

代码（静态THML页面）：

```html
<div class="wrapper">
    <div class="filterFunc">
        <input type="text" class="search"></input>
        <span class="btn">Java</span>
        <span class="btn">Web</span>
        <span class="btn active">All</span>
    </div>
    <div class="friendList">
        <ul>
            <!-- 
            <li>
                <img src="./arr-img/1.png" alt="">
                <p class="name">111</p>
                <p class="des">111</p>
            </li>
            <li>
                <img src="./arr-img/2.png" alt="">
                <p class="name">222</p>
                <p class="des">222</p>
            </li>
            <li>
                <img src="./arr-img/3.png" alt="">
                <p class="name">333</p>
                <p class="des">333</p>
            </li>
            <li>
                <img src="./arr-img/4.png" alt="">
                <p class="name">444</p>
                <p class="des">444</p>
            </li>
            <li>
                <img src="./arr-img/5.png" alt="">
                <p class="name">555</p>
                <p class="des">555</p>
            </li>
            -->
        </ul>
    </div>
</div>
```

css部分，在好友列表样式部分，布局可通过以下方式实现，img标签以absolute方式进行定位，文字标签则利用li的padding属性进行位置控制，**通过padding-top和padding-bottom使文字部分在垂直方向上居中，通过padding-left属性将文字部分挤到右边，使其不被图片覆盖**

```css
/* css reset */
*{
    padding: 0;
    margin: 0;
    list-style: none;
}

.wrapper{
    padding: 10px 15px;
    width: 400px;
    margin: 100px auto 0;
    border: 1px solid #ccc;
    border-radius: 5px;
}
/* filterFunc section style */
.wrapper .filterFunc{
    margin-bottom: 20px;
}

.wrapper .filterFunc .search{
    width: 250px;
    height: 20px;
    border: 1px solid #999;
    padding-left: 10px;
    outline: none;
    border-radius: 4px;
}

.wrapper .filterFunc .btn{
    color: #3c8dff;
    cursor: pointer;
    padding: 0 5px;
    border-radius: 4px;
}

.wrapper .filterFunc .active{
    color: #fff;
    background-color: #3c8dff;
}

/* friendList section style */
.wrapper .friendList ul li{
    position: relative;
    padding-top: 10px;
    padding-bottom: 10px;
    padding-left: 50px;
    border-bottom: 1px solid #999;
}

.wrapper .friendList ul li img{
    position: absolute;
    left: 0;
    top: 10px;
    width: 40px;
    height: 40px;
}

.wrapper .friendList ul li .name{
    margin-bottom: 10px;
}

.wrapper .friendList ul li .des{
    color: #666;
    font-size: 12px;
}
```

同时也可以利用JS来动态地为页面ul加载li标签及内部标签，这里主要通过forEach方法实现，代码如下：

```javascript
//原始数据
var personArr = [
    {name: '坤子锅', src:'./arr-img/1.png', des: '坤坤爱学习', job: 'Java'},
    {name: '凯子锅', src:'./arr-img/2.png', des: '篮球爱学习', job: 'Java'},
    {name: '东子锅', src:'./arr-img/3.png', des: '唱', job: 'Web'},
    {name: '晓子锅', src:'./arr-img/4.png', des: '跳', job: 'Web'},
    {name: '楠子锅', src:'./arr-img/5.png', des: 'rap爱学习', job: 'Web'},
];
//获取ul标签
var oUl = document.getElementsByClassName('friendList')[0].getElementsByTagName('ul')[0];
renderPage(personArr);
//通过遍历数组来为ul部分添加内容
function renderPage(data){
    var htmlStr = '';
    data.forEach(function (ele, index, self) {
        //ele.name => p.name
        //ele.src => img src
        //ele.des => p.des
        htmlStr += '<li><img src="' + ele.src + '"><p class="name">' + ele.name + '</p><p class="des">' + ele.des + '</p></li>';
    });
    oUl.innerHTML = htmlStr;
}
```

然后为input和span标签添加事件来实现关键字查询功能，其中查询筛选功能主要通过filter方法来进行实现，在筛选操作结束后，再利用前面的renderPage来对页面进行渲染即可实现关键字或属性的查询功能，在上述操作中，按钮事件的绑定通过forEach函数实现，dom获取节点返回的结果为类数组（实质上即为对象），为使类数组可以使用forEach方法，这里可通过slice方法来将类数组转为数组，代码如下：

```javascript
var state = {
    text : '',
    job : 'All'
};
//获取输入框标签
var oInput = document.getElementsByClassName('search')[0];
//合并筛选函数
var lastFilterFunc = combineFilterFunc({text: filterArrayByText, job: filterArrayByJob})
//为输入框绑定oninput事件，当input框内的内容发生变化时，检测并执行事件内容
oInput.oninput = function () {
    state.text = this.value;
    renderPage(lastFilterFunc(personArr));
}

// 为所有按钮绑定事件
var oButtonArr = document.getElementsByClassName('btn');
//通过slice方法来将类数组转换为数组
oButtonArr = Array.prototype.slice.call(oButtonArr);
var lastActiveBtn = oButtonArr[oButtonArr.length - 1];
oButtonArr.forEach(function (ele, index, self){
    ele.onclick = function(){
        //在点击事件执行后，执行更换类名函数
        changeActive(this);
        state.job = this.innerText;
        //通过两次过滤来完成两种属性筛选
        renderPage(lastFilterFunc(personArr));
    }
});

//替换标签类名
function changeActive(ele){
    // 初始的active按钮为all，其位置为oButtonArr.length-1，将当前点击标签的className变为active，并将上一个active标签类名中的active清掉，最后将lastActiveBtn变为当前点击的标签，以供下次点击使用
    ele.className = 'btn active';
    lastActiveBtn.className = 'btn';
    lastActiveBtn = ele;
}
```

为优化代码，这里将通过输入框筛选的函数filterArrayByText和通过按钮筛选的函数filterArrayByJob进行封装，最后采用一个lastFilterFunc来完成筛选，筛选函数的代码如下：

```javascript
//根据输入框内容筛选
function filterArrayByText(data, text){
    // 若input中的内容为空，则不进行过滤
    if (!text){
        return data;
    }
    return data.filter(function (ele, index, self) {
        return ele.name.indexOf(text) !== -1;
    });
}

//根据工作筛选
function filterArrayByJob(data, job){
    // 若input中的内容为空，则不进行过滤
    if (job === 'All'){
        return data;
    }
    return data.filter(function (ele, index, self) {
        return ele.job.indexOf(job) !== -1;
    });
}

//conbineFilterFunc
//合并filterArrayByText  filterArrayByJob ......
function combineFilterFunc(config){
    return function (data){
        var lastArr = data;
        for (var prop in config){
            //prop = 'text' 'job'
            //config[prop] = filterArrayByText
            //config[prop] = filterArrayByJob
            lastArr = config[prop](lastArr, state[prop])
        }
        return lastArr;
    }
}
```

