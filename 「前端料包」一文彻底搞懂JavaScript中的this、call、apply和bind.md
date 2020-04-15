


## 前言
关于JavaScript中this又是一个老生常谈的话题，也是面试绕不开的经典考题。**「前端料包」** 系列第三篇就聊聊关于this的话题。写的很小白，如有不对，欢迎各路大佬指正~
![](https://user-gold-cdn.xitu.io/2019/12/2/16ec681ff474766c?w=1022&h=1620&f=png&s=49393)
 
 在讲this之前，先得说说`环境` 这个概念。一门语言在运行的时候，需要一个环境，叫做`宿主环境`。对于JavaScript，宿主环境最常见的是web浏览器，另一个最为常见的就是 Node 了，同样作为宿主环境，node 也有自己的 JavaScript 引擎：`V8`（目前最快JavaScript引擎、Google生产）。关于node中的this本文不做展开。日后更新~
 ## this的初衷
  this设计的初衷是**在函数内部使用，用来指代当前的运行环境**。为什么这么说呢？
  
  JavaScript中的对象的赋值行为是将地址赋给一个变量，引擎在读取变量的时候其实就是要了个地址然后再从原始地址中读取对象。而JavaScript 允许函数体内部引用当前环境的其他变量，而这个变量是由运行环境提供的。由于函数又可以在不同的运行环境执行（如全局作用域内执行，对象内执行...），所以需要一个机制来表明代码到底在哪里执行！于是this出现了，它的设计目的就是在函数体内部，指代函数当前的运行环境。

 ## global this
在浏览器里，在全局范围内：

 1. this等价于window对象；
 2. 用var声明一个变量和给this或者window添加属性是等价的；
 3. 如果你在声明一个变量的时候没有使用var或者let、const(es6),你就是在给全局的this添加或者改变属性值。

```js
// 1
console.log(this === window); //true
//2
var name = "Jake";
console.log(this.name ); // "Jake"
console.log(window.name ); // "Jake"

//3
 age = 23;
 function testThis() {
   age = 18;
 }
 console.log(this.age ); // 23
 testThis();
 console.log(this.age ); // 18

```
总结起来就是：在全局范围内this是大哥大，它等价于window对象（即指向window），如果你声明一些全局变量(不管在任何地方)，这些变量都会作为this的属性。



 ## function  this
 对于函数中的this的指向问题，有一句话很好用：`运行时`**this永远指向最后调用它的那个对象**。
 
 举一个栗子
```js
var name = "windowsName";
function sayName() {
var name = "Jake";
console.log(this.name);   // windowsName
console.log(this);    // Window
}
sayName();
console.log(this) // Window
```
我们看最后调用 sayName的地方 `sayName()`;，前面没有调用的对象那么就是全局对象 window，这就相当于是 `window.sayName()`。

> 需要注意的是，对于严格模式来说，默认绑定全局对象是不合法的，this被置为undefined。会报错 `Uncaught TypeError: Cannot read property 'name' of undefined。`

再看下面这个栗子

```js
function foo() {
    console.log( this.age );
}

var obj1 = {
    age : 23,
    foo: foo
};

var obj2 = {
    age : 18,
    obj1: obj1
};

obj2.obj1.foo(); // 23

```
还是开头的那句话，最后调用`foo()`的是`obj1`，所以this指向`obj1`，输出23。
## 构造函数中的this
所谓构造函数，就是通过这个函数生成一个新对象（object）。当一个函数作为构造器使用时(通过 new 关键字), 它的 this 值绑定到新创建的那个对象。如果没使用 new 关键字, 那么他就只是一个普通的函数, this 将指向 window 对象。

这又是另一个经典话题：**new 的过程**

```js
var a = new Foo("zhang","jake");

new Foo{
    var obj = {};
    obj.__proto__ = Foo.prototype;
    var result = Foo.call(obj,"zhang","jake");
    return typeof result === 'obj'? result : obj;
}
```
若执行 new Foo()，过程如下：
1）创建新对象 obj；

2）给新对象的内部属性赋值，构造原型链（将新对象的隐式原型指向其构造函数的显示原型）；

3）执行函数 Foo，执行过程中内部 this 指向新创建的对象 obj（这里使用了call改变this指向）；

4）如果 Foo 内部显式返回对象类型数据，则返回该数据，执行结束；否则返回新创建的对象 obj。

```js
var name = "Jake";jiuzhixiang
function testThis(){
  this.name = 'jakezhang';
  this.sayName = function () {
		return this.name;
	}
}
console.log(this.name ); // Jake

new testThis(); 
console.log(this.name ); // Jake

var result = new testThis();
console.log(result.name ); // jakezhang
console.log(result.sayName()); // jakezhang

testThis();  
console.log(this.name ); // jakezhang
```
很显然，谁被new了，this就指向谁。
## class中的this
> 本小节摘自阿里大神Nealyang的文章---->[【THE LAST TIME】this：call、apply、bind](https://juejin.im/post/5da7cdff6fb9a04de7735742#heading-10)

在es6中，类，是 JavaScript 应用程序中非常重要的一个部分。类通常包含一个 constructor ， this可以指向任何新创建的对象。
不过在作为方法时，如果该方法作为普通函数被调用， this也可以指向任何其他值。与方法一样，类也可能失去对接收器的跟踪。

```js
class Hero {
  constructor(heroName) {
    this.heroName = heroName;
  }
  dialogue() {
    console.log(`I am ${this.heroName}`)
  }
}
const batman = new Hero("Batman");
batman.dialogue();

```
构造函数里的 this指向新创建的 类实例。当我们调用 batman.dialogue()时， dialogue()作为方法被调用， batman是它的接收器。
但是如果我们将 dialogue()方法的引用存储起来，并稍后将其作为函数调用，我们会丢失该方法的接收器，此时 this参数指向 undefined 。

```js
const say = batman.dialogue;
say();

```
出现错误的原因是JavaScript 类是隐式的运行在严格模式下的。我们是在没有任何自动绑定的情况下调用 say()函数的。要解决这个问题，我们需要手动使用 bind()将 dialogue()函数与 batman绑定在一起。

```js
const say = batman.dialogue.bind(batman);
say();

```

## call、apply和bind中的this
call、apply、bind 被称之为 this 的强绑定，用来改变函数执行时的this指向，目前所有关于它们的运用，都是基于这一点来进行的。

```js
var name = 'zjk';
  function fun() {
  console.log (this.name);
}

var obj= {
  name: 'jake'
};
fun(); // zjk
fun.call(obj); //Jake

```
上面的`fun.ccall(obj)`等价于`fun.capply(obj)`和`fun.cbind(obj)()`

## 箭头函数中的this

es5中的this要看函数在什么地方调用（即要看运行时），通过谁是最后调用它该函数的对象来判断this指向。但es6的**箭头函数中没有 this 绑定，必须通过查找作用域链来决定其值**，如果箭头函数被非箭头函数包含，则 this 绑定的是最近一层非箭头函数的 this，否则，this 为 undefined。箭头函数的 this 始终指向函数定义时的 this，而非执行时。

```js
    let name = "zjk";

    let o = {
        name : "Jake",

        sayName: function () {
            console.log(this.name)     
        },

        func: function () {
            setTimeout( () => {
                this.sayName()
            },100);
        }

    };

    o.func()     // Jake
```
使用 call 、 apply或 bind等方法给 this传值，箭头函数会忽略。箭头函数引用的是箭头函数在创建时设置的 this值。

```js
let obj = {
  name: "Jake",
  func: (a,b) => {
      console.log(this.name,a,b);
  }
};
func.call(obj,1,2);// 1 2
func.apply(obj,[1,2]);//  1 2

```
`最后放一道常见的this面试题`
```js
var number = 1;

var obj = {

	number:2,

	showNumber:function(){

	this.number = 3;

	(function(){

	console.log(this.number);

})();

	console.log(this.number);

}

};

obj.showNumber();// 答案就欢迎留在评论区囖~

```

## call & apply
每个函数都包含两个非继承而来的方法：apply()和 call()。这两个方法的用途都是在特定的作用域中调用函数，实际上等于设置函数体内 this 对象的值。

#### apply()
apply()方法接收两个参数：一个是在其中运行函数的作用域，另一个是参数数组。其中，第二个参数可以是 Array 的实例，也可以是arguments 对象。


```js
function sum(num1, num2){ 
 return num1 + num2; 
} 
function callSum1(num1, num2){ 
 return sum.apply(this, arguments); // 传入 arguments 对象
} 
function callSum2(num1, num2){ 
 return sum.apply(this, [num1, num2]); // 传入数组
} 
console.log(callSum1(10,10)); //20
console.log(callSum2(10,10)); //20
```

> 在严格模式下，未指定环境对象而调用函数，则 this 值不会转型为 window。除非明确把函数添加到某个对象或者调用 apply()或 call()，否则 this 值将是undefined。


#### call()
call()方法与 apply()方法的作用相同，它们的唯一区别在于接收参数的方式不同。在使用call()方法时，传递给函数的参数必须逐个列举出来。

```js
function sum(num1, num2){ 
 return num1 + num2; 
}
function callSum(num1, num2){ 
 return sum.call(this, num1, num2); 
} 
console.log(callSum(10,10)); //20
```
call()方法与 apply()方法返回的结果是完全相同的，至于是使用 apply()还是 call()，完全取决于你采取哪种给函数传递参数的方式最方便。

 - 参数数量/顺序确定就用call，参数数量/顺序不确定的话就用apply。
 - 考虑可读性：参数数量不多就用call，参数数量比较多的话，把参数整合成数组，使用apply。

 
 #### bind()
 bind()方法会创建一个函数的实例，其 this 值会被绑定到传给 bind()函数的值。意思就是 bind() 会返回一个新函数。例如：

```js
window.color = "red"; 
var o = { color: "blue" }; 
function sayColor(){ 
 alert(this.color); 
} 
var objectSayColor = sayColor.bind(o); 
objectSayColor(); //blue
```
#### call/apply与bind的区别

  ***执行：***
 - call/apply改变了函数的this上下文后马上执行该函数
 - bind则是返回改变了上下文后的函数,不执行该函数
```js
function add (a, b) {
    return a + b;
}

function sub (a, b) {
    return a - b;
}

add.bind(sub, 5, 3); // 这时，并不会返回 8
add.bind(sub, 5, 3)(); // 调用后，返回 8

```
***返回值:***
 - call/apply 返回fun的执行结果
 - bind返回fun的拷贝，并指定了fun的this指向，保存了fun的参数。



## call/apply/bind的核心理念
从上面几个简单的例子可以看出`call/apply/bind`是在向其他对象借用方法，这也符合我们的正常思维，举个简单的栗子。
我和我高中一个同学玩的超级好，衣服鞋子都是共穿的，去买衣服的时候，他买衣服，我买鞋子；回来后某天我想穿他买的衣服了，但是我没有，于是我就借用他的穿。这样我就既达到了穿新衣服的目的，又节省了money~
A对象有个方法，B对象因为某种原因也需要用到同样的方法，这时候就可以让B借用 A 对象的方法啦，既达到了目的，又节省了内存。

**这就是call/apply/bind的核心理念：借。**

## call/apply/bind的应用场景
关于call/apply/bind的用法因篇幅有限就不做展开了，可以看看下面这篇，个人觉得写得超级棒！

> [「干货」细说 call、apply 以及 bind 的区别和用法](https://juejin.im/post/5c493086f265da6115111ce4)

## 手写实现apply、call、bind


#### apply
1、先给Function原型上扩展个方法并接收2个参数,

```js
Function.prototype.myApply = function (context, args) {}
```

2、因为不传context的话,this会指向window,所以这里将context和args做一下容错处理。

```js
Function.prototype.myApply = function (context, args) { 
    // 处理容错
    context = (typeof context === 'object' ? context : window)
    args = args ? args : []
}
```

3、使用隐式绑定去实现显式绑定

```js
Function.prototype.myApply = function (context, args) {
    // 处理容错
   context = (typeof context === 'object' ? context : window)
   args = args ? args : []
    //给context新增一个独一无二的属性以免覆盖原有属性
    const key = Symbol()
    context[key] = this
    //通过隐式绑定的方式调用函数
    context[key](...args)
}
```


4、最后一步要返回函数调用的返回值,并且把context上的属性删了才不会造成影响

```js
Function.prototype.myApply = function (context, args) {
   // 处理容错
    context = (typeof context === 'object' ? context : window)
    args = args ? args : []
    //给context新增一个独一无二的属性以免覆盖原有属性
    const key = Symbol();
    context[key] = this;
    //通过隐式绑定的方式调用函数
    const result = context[key](...args);
    //删除添加的属性
    delete context[key]
    //返回函数调用的返回值
    return result
}
```

这样一个乞丐版的apply就实现了,至于优化，网上有很多大牛写的很好，可以去找找，这里就不做继续优化了。

验证走一波~

```js

function fun(...args) {
  console.log(this.name,...args)
}
const result = { 
name: 'Jake' 
}
// 参数为数组;方法立即执行
fun.myApply (result, [1, 2])

```
结果如下，说明已经实现了apply方法。
![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/12/2/16ec6809f5c29087?w=305&h=100&f=png&s=4903)
#### call
call的实现几乎和apply一模一样，就直接上代码了。
```js
//传递参数从一个数组变成逐个传参了,不用...扩展运算符的也可以用arguments代替
Function.prototype.NealCall = function (context, ...args) {
    //这里默认不传就是给window,也可以用es6给参数设置默认参数
     context = (typeof context === 'object' ? context : window)
    args = args ? args : []
    //给context新增一个独一无二的属性以免覆盖原有属性
    const key = Symbol();
    context[key] = this;
    //通过隐式绑定的方式调用函数
    const result = context[key](...args);
    //删除添加的属性
    delete context[key];
    //返回函数调用的返回值
    return result;
}

```


####  bind
bind的实现要稍微麻烦一点，因为bind是返回一个绑定好的函数,apply是直接调用.但其实简单来说就是返回一个函数,里面执行了apply上述的操作而已.不过有一个需要判断的点,因为返回新的函数,要考虑到使用new去调用,并且new的优先级比较高,所以需要判断new的调用,还有一个特点就是bind调用的时候可以传参,调用之后生成的新的函数也可以传参,效果是一样的,所以这一块也要做处理。
```js
Function.prototype.myBind = function (objThis, ...params) {
    const thisFn = this; // 存储源函数以及上方的params(函数参数)
    // 对返回的函数 secondParams 二次传参
    let fToBind = function (...secondParams) {
        const isNew = this instanceof fToBind // this是否是fToBind的实例 也就是返回的fToBind是否通过new调用
        const context = isNew ? this : Object(objThis) // new调用就绑定到this上,否则就绑定到传入的objThis上
        return thisFn.call(context, ...params, ...secondParams); // 用call调用源函数绑定this的指向并传递参数,返回执行结果
    };
    if (thisFn.prototype) {
        // 复制源函数的prototype给fToBind 一些情况下函数没有prototype，比如箭头函数
        fToBind.prototype = Object.create(thisFn.prototype);
    }
    return fToBind; // 返回拷贝的函数
};

```

## 总结

 1. 在浏览器里，在全局范围内this 指向window对象；
 2. 在函数中，this永远指向最后调用他的那个对象；
 3. 构造函数中，this指向new出来的那个新的对象；
 4. call、apply、bind中的this被强绑定在指定的那个对象上；
 5. 箭头函数中this比较特殊,箭头函数this为父作用域的this，不是调用时的this.要知道前四种方式,都是调用时确定,也就是动态的,而箭头函数的this指向是静态的,声明的时候就确定了下来；
 6. apply、call、bind都是js给函数内置的一些API，调用他们可以为函数指定this的执行,同时也可以传参。
 
 **最后放一张图来帮助记忆**


![](https://user-gold-cdn.xitu.io/2020/4/12/1716ceb4a34ad963?w=2753&h=1181&f=png&s=205720)
## 后话
说来惭愧啊，刚学JS的时候我写过一篇关于this的学习笔记，好像2个小时就写完了，本以为一天就能写完这篇，结果前前后后写了好几天，写之前也看了几篇各路大佬写的，我下面都贴了链接，我只能感叹写的是真的好啊！不过这几天下来还是对this这个知识点有了新的的认识。另外，小生乃前端小白一枚，写文章的最初衷是为了让自己对该知识点有更深刻的印象和理解，写的东西也很小白，文中如有不对，欢迎指正~  然后就是希望看完的朋友可以点个喜欢，也可以关注一波~  我会持续输出！

[个人博客链接](http://jkvin.cn) 

[CSDN个人主页](https://blog.csdn.net/qq_39735040) 

[掘金个人主页](https://juejin.im/user/5d999147f265da5b9764b996)

[简书个人主页](https://www.jianshu.com/u/370e80e9707d)

## 参考文章
 `红宝书第五章`

 [`JavaScript 的 this 原理`](http://www.ruanyifeng.com/blog/2018/06/javascript-this.html)
 
[`【THE LAST TIME】this：call、apply、bind`](https://juejin.im/post/5da7cdff6fb9a04de7735742#heading-10)

 [`JavaScript中的this陷阱的最全收集--没有之一`](https://segmentfault.com/a/1190000002640298#item-1-1)
 
[`详解 JS 中 new 调用函数原理`](https://juejin.im/post/5b397b526fb9a00e5d7999a4)

[`js基础-面试官想知道你有多理解call,apply,bind？[不看后悔系列]`](https://juejin.im/post/5d469e0851882544b85c32ef)
 
 
 

