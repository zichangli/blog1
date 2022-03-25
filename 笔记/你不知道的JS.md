## JS执行机制

### 浏览器常驻的线程 

js引擎线程（解释执行js代码、用户输入、网络请求）

GUI线程（绘制用户界面、与js主线程是互斥的）

http网络请求线程（处理用户的get、post等请求，并返回结果后将回调函数推入任务队列）

定时触发器线程（setTimeout、setInterval等待结束后把执行函数推入任务队列中）

浏览器事件处理线程（将click、mouse等交互事件发生后这些事件放入事件队列中）



JS引擎线程和GUI线程是互斥的：JS可以操作DOM元素，进而会影响到GUI的渲染结果，因此JS引擎线程与GUI渲染线程是互斥的。也就是说当JS引擎线程处于运行状态时，GUI渲染线程将处于冻结状态。

JS执行机制-单线程：同一时间只能做一件事情

**大量数据操作怎么办？**

单线程计算能力有限，大量数据需要计算渲染的话，我们可以配合后端进行操作，比如我们后期进阶班里讲到的VUE与nodejs配合，也就是传说中的SSR技术

### JS执行机制

Javascript是基于单线程运行的，同时又是可以异步执行的，一般来说这种既是单线程又是异步的语言都是基于事件来驱动的，恰好浏览器就给Javascript提供了这么一个环境

DOM事件、ajax请求、setTimeout均属于异步任务，Promise的后续操作也属于异步，但存放在微队列

## bind的模拟与实现

call和apply实现原理

call和apply的实现原理大致相似，可以分为三个步骤

- 将调用的函数添加为目标this的属性
- 以obj.fn()的方式执行该函数
- 删除目标对象上的该属性，即所使用的函数

需注意一下几点，call的传参是通过传参列表实现的，而apply则为传入一个数组，当call和apply的参数传入为null，则this指向window

```js
Function.prototype.myCall = function(context){
    context = context || window;
    context.fn = this;
    let args = [];
    for(let i = 0; i < arguments.length; i++){
        args.push("arguments[" + i + "]");
    }
    var result = eval('context.fn(' + args +')');

    delete context.fn
    return result;
}
```

### 使用

Func.bind(arg1, arg2, arg3......)

bind可以返回一个新的函数，this指向传入的第一个参数，后续参数可作为函数的实例参数

1. 第一个参数为null时，this指向window
2. 第一个参 数指向obj时，this指向obj
3. new newShow()，this指向所创建的对象

如：

```js
var value = 0;
var obj = {
    value: 1
}
function show(name, age){
    console.log(this.value);
    console.log(name, age)
}
var newShow = show.bind(obj， "cst");
newShow(18);//1      23      18
```

### 模拟

核心：改变this指向，返回一个函数，参数列表问题

```js
Function.prototype.newBind = function(target){
    target = target || window;
    //this -> show
    var self = this;
    var args = [].slice.call(arguments, 1)//从第一位开始截取传入所有参数，因为第零位为target，这里截取的args相当于Func.bind(target, arguments)中的arguments
    var temp = function(){};
    var F = function(){
        var _arg = [].slice.call(arguments, 0);//_arg相当于在调用bind生成的函数时所传入的参数，因此args拼接_arg即为最后所有的参数列表
        return self.apply(this instanceof temp ? this : target, args.concat(_arg));
    }
    temp.prototype = this.prototype;
    F.prototype = new temp();
    return F;
}
```

## 纯函数

函数f的概念就是，对于输入x产生一个输出y = f(x)

纯函数的定义是，对于相同的输入，永远会得到相同的输出，而且没有任何可观察的副作用，也不依赖外部环境的状态，如：

```js
let num = 18;
function compare(x, num){
    return x < num;
}
console.log(compare(10, num))
```

在向一个函数中传入参数时，如果参数为引用值，在函数中可使用深度克隆来达到纯函数的效果

### Bug-守恒定律

一旦你网站或应用的代码量达到一定程度，它将不可避免的包含某种bug。这不是JavaScript特有的问题，而是一个几乎所有语言都有的通病——虽然不是不可能，但是想要彻底清除程序中的所有bug还是非常难办到的。但是，这并不意味着我们不可以通过某些代码方式来预防bug的引入。

纯函数非常容易进行单元测试，因为不需要考虑上下文环境，只需要考虑输入和输出

用处：更好的管理状态，使得可预测性增强，降低代码管理的难度，但是前端基本上都是在和副作用打交道，所有函数都是纯函数这种愿望不可强求

## 柯里化

在数学和计算机科学中，柯里化时一种将使用多个参数的一个函数转换成一系列使用一个参数的函数的技术

前端使用柯里化的用途主要就应该是简化代码结构，提高系统的维护性，一个方法，只有一个参数，强制了功能的单一性，很自然就做到了功能内聚，降低耦合。

柯里化的优点就是降低代码的重复，提高代码的适用性

#### 前奏

```js
function add(a, b, c, d){
    return a + b + c + d;
}
function FixedParamsurry(fn){
    var _arg = [].slice.call(arguments, 1);
    return function(){
        var newArg = _arg.concat([].slice.call(arguments, 0));
        return fn.apply(this, newArg);
    }
}
```

#### 柯里函数

思路，判断当前所传入参数列表的长度是否为原函数所需参数的长度，若未达到，则采用递归的形式继续查看后续传入的参数，直到总参数个数达到目标数为止，返回一个函数。

```js
function FixedParamsCurry(fn){
    var _arg = [].slice.call(arguments, 1);
    return function(){
        var newArg = _arg.concat([].slice.call(arguments, 0));
        return fn.apply(this, newArg);
    }
}

function Curry(fn, length){
    var length = length || fn.length;
    return function(){
        if(arguments.length < length){
            var combined = [fn].concat([].slice.call(arguments, 0));
            return Curry(FixedParamsCurry.apply(this, combined), length - arguments.length);
        }else{
            return fn.apply(this, arguments);
        }
    }
}
```

JS中函数的length属性表示函数有多少个必须传入的参数

可用于Ajax请求中逐个传入参数，或保存传入某个参数后返回的函数，利用返回的函数更方便的发出Ajax请求

## 节流

在前端开发中有一部分的用户行为会频繁的触发事件执行，而对于DOM操作、资源加载等耗费性能的处理，很可能导致界面卡顿，甚至浏览器的崩溃。函数节流(throttle)和函数防抖(debounce)就是为了结局类似需求应运而生的

函数节流就是预定一个函数只有大于等于执行周期时才执行，周期内调用不执行。好像水滴攒到一定重量才会落下一样。

场景：窗口调整（resize）、页面滚动（scroll）、抢购疯狂点击（mousedown）

```js
const oBtn = document.getElementsByTagName('button')[0];
const oDiv = document.getElementsByTagName('div')[0];
let timer = null;
oBtn.onclick = throttle(buy, 1000)
function buy(){
    console.log(this)
    oDiv.innerHTML = parseInt(oDiv.innerHTML) + 1;
}
function throttle(handler, time){
    let lastTime = 0;
    return function(e){
        let nowTime = new Date().getTime()
        if (nowTime - lastTime > time){
            handler(this, arguments);
            lastTime = nowTime;
        }
    }
}
```

## 防抖

在前端开发中有一部分的用户行为会频繁的触发事件执行，而对于DOM操作，资源加载等耗费性能的处理，很可能导致界面的卡顿，甚至浏览器的崩溃。函数节流（throttle）和函数防抖（debounce）就是为了解决类似需求应运而生的

函数防抖就是在函数需要频繁触发情况时，只有够空闲的事件，才执行一次。好像公交司机会等人都上车后才出站一样

```js
const ipt = document.getElementsByTagName('input')[0];
let timer = null;
ipt.oninput = debounce(ajax, 1000);
function debounce(fn, time){
    let timer = null;
    return function(){
        let _self = this;
        let _arg = arguments;
        clearTimeout(timer);
        timer = setTimeout(function(){
            fn.apply(_self, _arg)
        }, time);
    }
}
function ajax(e){
    console.log(this.value, e);
}
```

