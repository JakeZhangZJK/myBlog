
## 前言

接触写博客有一段时间了，都是边学边学着写，但总感觉写的凌乱，想起啥写啥。这几天在刷红宝书，收获还是蛮多的，决定结合自己的学习，写一个系列，我叫它「前端料包」，旨在巩固前端基础，努力提升自己，同时也乐于做一个分享者。这个系列包括但不限于下面脑图中的内容，目录和发文顺序暂且如下：

![](https://user-gold-cdn.xitu.io/2019/11/7/16e4576ee993d1d9?w=1008&h=1620&f=jpeg&s=137855)
## 概述
在讲数据类型之前，想先讲讲**变量**。
`JavaScript`的变量是松散型的，即可以保存任何类型的数据，在JavaScript中我们使用**var**关键字来声明一个变量（es6中新增了let 、 const 来声明变量）

```js
var message1 = 10;
var message2 = true;
var message3 = "hello world";
```
当然，因为松散型，也可以改变同一变量的值的类型，虽然不推荐这么做，但是在JavaScript中完全有效。

```js 
var message1 = 10;
    message1 = true;// 重新给变量message赋不同类型的值，数据类型也会随之改变，不推荐这样做
```
而我们所有数据类型的值都是保存在自定义的某一个变量中。
`JavaScript`从诞生到现在最新的正式版本，一共有7种数据类型，其中有6种基本数据类型：`Undefined、Null、Boolean、Number、String和Symbol（es6新增）`；1种引用数据类型——`Object`（`Object`本质上是由一组无序的名值对组成的）。`JavaScript`不支持任何创建自定义类型的机制，而所有值最终都将是上述 7 种数据类型之一。这里要特别说明一下，最新的基本数据类型的第7个兄弟——`BigInt`已经诞生，将在下一版本（es10）中作为新特性出现（V8引擎v6.7 默认启用对 `BigInt` 的支持）。

> `BigInt`是什么? `BigInt`是`JavaScript`中一种可以用来表示任意精度整数的基本数据类型
> 
> `BigInt`可以用来表示任意精度整数的特性为`JavaScript`解锁了更多的骚操作，使用`BigInt`可以告别过去因为整数运算导致溢出的痛苦。特别是金融方面因为涉及大量的数据运算，比如高精度时间戳，或者数值过大的ID，这些是无法安全的用Number类型去存储的，所以退而求其次使用String类型去存储，有了BigInt类型后就可以安全的将其存储为数值类型。
> 
> 另外`BigInt`的实现也为实现`BigDecimal`打下坚实基础，那将对于以十进制精度表示货币金额并对其进行精确运算（也就是0.10 + 0.20 !== 0.30问题）非常有帮助

关于`BigInt`可以看看这篇文章：[JavaScript基本类型之--BigInt](https://segmentfault.com/a/1190000020920960?utm_source=tag-newest)


## 1、 Undefined类型
`Undefined`类型只有一个值，即特殊的`undefined`，一个变量在声明后未初始化时，这个变量的值就是`undefined`。

```js
var message;
alert(message);// undefined
```
需要注意的是声明了但未初始化的变量与未声明的变量是不一样的

```js
var msg;
alert(msg);// 声明了未初始化（即赋值），默认值为"undefined"
alert(a);// 报错
```
但使用`typeof`操作符来检测上面两个变量时，都会返回`undefined`

```js
var msg;
// var a
alert(typeof msg);// "undefined"
alert( typeof a);// "undefined"
```
这个结果有其逻辑上的合理性。因为虽然这两种变量从技术角度看有本质区别，但实际上无论对哪种变量也不可能执行真正的操作。

> 未初始化的变量会自动被赋予 undefined 值，没有必要将变量显式的设置为`undefined`，但显式地初始化变量依然是明智的选择。如果能够做到这一点，那么当 `typeof` 操作符返回"undefined"值时，我们就知道被检测的变量还没有被声明，而不是尚未初始化。

## 2、Null类型
`Null` 类型是第二个只有一个值的数据类型，这个特殊的值是 `null`。`null` 值表示一个空对象指针，使用 `typeof` 操作符检测 null 值时会返回`"object"`。

```js
var jake = null; 
alert(typeof jake ); // "object"
```

虽然 `typeof null` 会输出 `object`，但这只是 `JS` 存在的一个悠久 `Bug`。在 `JS` 的最初版本中使用的是 32 位系统，为了性能考虑使用低位存储变量的类型信息，000 开头代表是对象然而 `null` 表示为全零，所以将它错误的判断为 `object` 。
`null`值的主要作用是如果定义的变量在将来用于保存对象，那么最好将该变量初始化为`null`值。

## 3、Boolean类型
`Boolean`类型是`JavaScript`中使用最多的一种基本数据类型，只有两个值`true`和`false`（全为小写）。

```js
var  a = true;
var  b = false;
```
虽然`Boolean`类型只有两个值，但`JavaScript`中所有类型的值都有与这两个`Boolean`值等价的值，可以调用转型函数`Boolean()`将其他类型的值转化为`Boolean`值。

```js
var  msg = "hello world";
var  magBoolean = Boolean(msg);
```
根据转换值的数据类型及其实际值，返回一个`Boolean`值。各种数据类型及其对应的转换规则如下表：
|数据类型|转为true|转为false|
|-|--|-|--|
| Boolean| true|false|
| String| 任何非空字符串|" "（空字符串）|
| Number| 任何非零数字（包括无穷大）|0和NaN|
| Object| 任何对象|null|
| Undefined|not applicable（不适用）|undefined|

## 4、Number类型
`Number`类型算是`JavaScript`中最复杂也最令人关注的基本数据类型了，`Number`可以同时表示整数和浮点数值，同时也支持各种进制和科学计数法。具体如下

```js
var intNum = 55; // 整数

// 浮点数
var floatNum1 = 1.1; 
var floatNum2 = 0.1; 
var floatNum3 = .1; // 有效，但不推荐

// 科学计数法
var floatNum = 3.125e7; // 等于 31250000 相当于 3.125*10的7次方

// 八进制（以O开头),数字序列(0~7)
var octalNum1 = 070; // 八进制的 56
var octalNum2 = 079; // 无效的八进制数值——解析为 79
var octalNum3 = 08; // 无效的八进制数值——解析为 8

// 十六进制(以Ox开头),数字序列(0~9及A~F)，字母大小写同等
var hexNum1 = 0xA; // 十六进制的 10
var hexNum2 = 0x1f; // 十六进制的 31

```
`JavaScript`能够表示的最小数值保存在 `Number.MIN_VALUE` 中——在大多数浏览器中，这个值是 `5e-324`；能够表示的最大数值保存在`Number.MAX_VALUE` 中——在大多数浏览器中，这个值是 `1.7976931348623157e+308`。如果某次计算的结果得到了一个超出 `JavaScript` 数值范围的值，那么这个数值将被自动转换成特殊的 `Infinity` 值(有正负)。
这里要特别说明一下，浮点数值的最高精度是 17 位小数，但在进行算术计算时其精确度远远不如整数。例如，0.1 + 0.2的结果不是 0.3，而是 0.30000000000000004。这是因为0.1和0.2在转换成二进制后会无限循环，由于标准位数的限制后面多余的位数会被截掉，此时就已经出现了精度的损失，相加后因浮点数小数位的限制而截断的二进制数字在转换为十进制就会变成0000000000000004。所以上面提到的BigInt就应运而生。

> 关于浮点数值计算会产生舍入误差的问题，有一点需要明确：这是使用基于IEEE754 数值的浮点计算的通病，ECMAScript 并非独此一家；其他使用相同数值格式的语言也存在这个问题。

#### NaN
`NaN（Not a Number)`，即非数值，用于表示一个本来要返回数值的操作数未返回数值的情况。
`NaN`有两个非同寻常的特点：

 1. 任何涉及`NaN`的操作(例如`NaN/10`)都会返回`NaN`；
 2. `NaN`与任何值都不相等，包括`NaN`本身。

针对这两个特点，`JavaScript`定义了`isNaN()`函数。这个函数接受一个参数，该参数可以是任何类型。`isNaN()`在接收到一个值后，会尝试将这个值转换为数值，而任何不能被转换为数值的值都会导致函数返回`true`。

```js
alert(isNaN(NaN)); //true 
alert(isNaN(10)); //false（10 是一个数值）
alert(isNaN("10")); //false（可以被转换成数值 10）
alert(isNaN("blue")); //true（不能转换成数值）
alert(isNaN(true)); //false（可以被转换成数值 1）
```
#### 数值转换
`JavaScript`提供3个函数可以把非数值转换为数值：

 - `Number()`可以用于任何数据类型
 - `parseInt()`和`parseFloat()`专门用于把字符串转换为数值
 
 **Number()**
 `Number()`函数的转换规则很多，这里直接引用红宝书里的描述：

>  
> 如果是 Boolean 值，true 和 false 将分别被转换为 1 和 0。
> 
>如果是数字值，只是简单的传入和返回。 
>
>如果是 null 值，返回 0。  如果是 undefined，返回 NaN。 
>
>如果是字符串，遵循下列规则：
>    
>   a、中只包含数字（包括前面带正号或负号的情况），则将其转换为十进制数值，即"1" 会变成 1，"123"会变成 123，而"011"会变成 11（注意：前导的零被忽略了）；
>
> b、串中包含有效的浮点格式，如"1.1"，则将其转换为对应的浮点数值（同样，也会忽 略前导零）； 
>
> c、字符串中包含有效的十六进制格式，例如"0xf"，则将其转换为相同大小的十进制整 数值；
>
> d、字符串是空的（不包含任何字符），则将其转换为 0；  如果字符串中包含除上述格式之外的字符，则将其转换为 NaN。
>
> 
> 如果是对象，则调用对象的 valueOf()方法，然后依照前面的规则转换返回的值。如果转换 的结果是 NaN，则调用对象的
> toString()方法，然后再次依照前面的规则转换返回的字符 串值。

```js
var num1 = Number("Hello world!"); //NaN 
var num2 = Number(""); //0 
var num3 = Number("000011"); //11 
var num4 = Number(true); //1 
NumberExample04.htm
```
**parseInt()**
`parseInt()`函数在转换字符串时，更多的是看其是否符合数值模式。它会忽略字
符串前面的空格，直至找到第一个非空格字符。如果第一个字符不是数字字符或者负号，`parseInt()`
就会返回 `NaN`；也就是说，用 `parseInt()`转换空字符串会返回 `NaN`（`Number()`对空字符返回 `0`）。如
果第一个字符是数字字符，`parseInt()`会继续解析第二个字符，直到解析完所有后续字符或者遇到了
一个非数字字符。

```js
var num1 = parseInt("1234blue"); // 1234 
var num2 = parseInt(""); // NaN 
var num3 = parseInt("0xA"); // 10（十六进制数）
var num4 = parseInt(22.5); // 22 
var num5 = parseInt("070"); // 56（八进制数）
var num6 = parseInt("70"); // 70（十进制数）
var num7 = parseInt("0xf"); // 15（十六进制数）
```
在使用 `parseInt()`解析像八进制字面量的字符串时，`ECMAScript 3` 和 `5` 存在分歧。

```js
//ECMAScript 3 认为是 56（八进制），ECMAScript 5 认为是 70（十进制）
var num = parseInt("070");
```
因此`parseInt()`引入第二个参数：转换时使用的基数，以解决上述困惑。

```js
var num1 = parseInt("10", 2); //2 （按二进制解析）
var num2 = parseInt("10", 8); //8 （按八进制解析）
var num3 = parseInt("10", 10); //10 （按十进制解析）
var num4 = parseInt("10", 16); //16 （按十六进制解析）
```

**parseFloat()**
与 `parseInt()`函数类似，`parseFloat()`也是从第一个字符（位置 0）开始解析每个字符。而且也是一直解析到字符串末尾，或者解析到遇见一个无效的浮点数字字符为止。

```js
var num1 = parseFloat("1234blue"); //1234 （整数）
var num2 = parseFloat("0xA"); //0 
var num3 = parseFloat("22.5"); //22.5 
var num4 = parseFloat("22.34.5"); //22.34 第二个小数点无效
var num5 = parseFloat("0908.5"); //908.5 
var num6 = parseFloat("3.125e7"); //31250000
```

## 5、String类型
`String`，即字符串，由一对双引号或单引号表示(单双引号没有区别)

```js

var firstName = "Jake";
var lastName = 'JakeZhang';
```
`JavaScript`中的字符串是不可变的，也就是说，字符串一旦创建，它们的值就不能改变。要改变某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量。

```js
var   name  = "Jake";
name = name + "Zhang";
```
实现这个操作的过程如下：首先创建一个能容纳 10 个字符的新字符串，然后在这个字符串中填充"Jake"和"Zhang"，最后一步是销毁原来的字符串"Jake"和字符串"Zhang"，因为这两个字符串已经没用了(这个过程是在后台发生的)。

实际开发中经常为方便存储，经常需要将值转换为字符串。要把一个值转换为一个字符串有两种方式：


**1、toString()**
除了`null`和`undefined`值没有`tostring()`方法，其他值都有这个方法，该方法返回字符串的一个副本。

```js
var age = 11; 
var ageAsString = age.toString(); // 字符串"11" 
var found = true; 
var foundAsString = found.toString(); // 字符串"true"
```
`toString()`可以传入一个参数：输出数值的基数。可以输出以二进制、八进制、十六进制，乃至其他任意有效进制格式表示的字符串值。下面给出几个例子：

```js
var num = 10; 
alert(num.toString()); // "10" 
alert(num.toString(2)); // "1010" 
alert(num.toString(8)); // "12" 
alert(num.toString(10)); // "10" 
alert(num.toString(16)); // "a"
```
**2、使用+"  "**
即可以通过**要转换的值 + 空字符串("  ")**，也可以实现转换。

```js
var num = 10;
var numAsString = num + " ";// "10"
var boolean = true;
var booleanAsString = boolean + " ";// "true"

var a ;
var b = a + " ";// "undefined "

var c = null;
var d = c + " ";// "null "

var o = {
   valueOf: function() {
           return -1;
                }
} 
 var m = o + " ";// "-1 "

```
## 6、Symbol
`Symbol`是`es6`新增的一种原始数据类型，表示独一无二的值，是一种唯一标识符。`Symbol`值通过`Symbol()`函数生成。

```js
let id = Symbol("jake");
```
`Symbol`的主要特点是如上所说的唯一性，可用作对象的唯一属性名，即使是用同一个变量生成的值也不相等。

```js
 let id1 = Symbol('jake');
 let id2 = Symbol('jake');
 console.log(id1 == id2);  //false
```
但我们不排除希望能够多次使用同一个`symbol`值的情况。官方提供的`Symbol.for()`方法可以做到这一点。它接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 `Symbol` 值。如果有，就返回这个 `Symbol` 值，否则就新建一个以该字符串为名称的 `Symbol` 值，并将其注册到全局。

```js
 let name1 = Symbol.for('name'); //检测到未创建后新建
 let name2 = Symbol.for('name'); //检测到已创建后返回
 console.log(name1 === name2); // true

```
`Symbol`的另一特点是隐藏性，`Symbol` 作为属性名，遍历对象的时候，该属性不会出现在`for...in、for...of`循环中，也不会被`Object.keys()`、`Object.getOwnPropertyNames()`、`JSON.stringify()`返回。

```js
 let id = Symbol("id");
 let obj = {
  [id]:'symbol'
 };
 for(let option in obj){
     console.log(obj[option]); //空
 }

```

但是也有能够访问的方法：`Object.getOwnPropertySymbols`
该方法会返回一个数组，成员是当前对象的所有用作属性名的`Symbol`值。

```js
 let id = Symbol("id");
 let obj = {
  [id]:'symbol'
 };
let array = Object.getOwnPropertySymbols(obj);
 console.log(array); //[Symbol(id)]
 console.log(obj[array[0]]);  //'symbol'

```
关于`Symbol`这个新的数据类型更多的知识点可以参考阮神的文章—>[传送门](http://es6.ruanyifeng.com/#docs/symbol)


## 7、引用数据类型——Object类型
`Object`类型是`JavaScript`中最庞大而复杂的引用数据类型，本文只做简单介绍，后续的文章会做`Object`的详细介绍。
`Object`，即对象，是一组数据和功能的集合。对象可以通过执行`new`操作符后跟要创建
的对象类型的名称来创建。而创建 `Object` 类型的实例并为其添加属性和（或）方法，就可以创建自定
义对象。

```js
var person1 = new Object(); 
person.name = "Jake"; 
person.age = 23;

var person2 = {}; //与 new Object()相同
person2.name = "Jake"; 
perso2.age = 23;

//字面量的创建方式
var person3 = {
	name:"jakezhang",//name如果是保留字、有连接符/空格，则要'name-p'，即使用字符串
	age:23,
	action:function(){
			console.log(this.name);
			}
					
};
```
以上任意一种方式都可以创建一个对象实例，当然更多的写法和设计思想以后的文章再做体现。在实际的开发中我们用的最多的是字面量的方式，因为这种语法要求的代码量少，而且能够给人封装数据的感觉。这里引用一个红宝书的例子：

```js
function displayInfo(args) { 
 var output = ""; 
 if (typeof args.name == "string"){
 output += "Name: " + args.name + "\n"; 
 } 
 if (typeof args.age == "number") { 
 output += "Age: " + args.age + "\n"; 
 } 
 alert(output); 
} 
displayInfo({ 
 name: "Nicholas", 
 age: 29 
}); 
displayInfo({ 
 name: "Greg" 
});
```
代码相信都看得懂~

**Object 的每个实例都具有下列属性和方法：**

 - `constructor`：保存着用于创建当前对象的函数。对于前面的例子而言，构造函数（`constructor`）就是 `Object()`。
 - `hasOwnProperty(propertyName)`：用于检查给定的属性在当前对象实例中（而不是在实例的原型中）是否存在。其中，作为参数的属性名（`propertyName`）必须以字符串形式指定（例如：`o.hasOwnProperty("name")`）。
   
 - `isPrototypeOf(object)`：用于检查传入的对象是否是传入对象的原型。      
  
 - `propertyIsEnumerable(propertyName`)：用于检查给定的属性是否能够使用 for-in 语句来枚举。与`hasOwnProperty()`方法一样，作为参数的属性名必须以字符串形式指定。  
  
 - `toLocaleString()`：返回对象的字符串表示，该字符串与执行环境的地区对应。

 - `toString()`：返回对象的字符串表示。

 - `valueOf()`：返回对象的字符串、数值或布尔值表示。通常与 toString()方法的返回值相同。

由于在 ECMAScript 中 Object 是所有对象的基础，因此所有对象都具有这些基本的属性和方法。
## 8、基本数据类型和引用类型值的比较
#### **基本数据类型**

 1. **值是不可变的**
 

```js
var name = "Jake Zhang";
name.toUpperCase();//输出 JAKE ZHANG
console.log(name);// 输出 Jake Zhang
```
由以上代码可看出基本数据类型的值是不可变的。

 2. **存放在栈区**
  基本类型值指的是简单的数据段，按值访问，可操作保存在变量中的实际的值，其占据空间小、大小固定，属于被频繁使用的数据，所以放入栈（`stack`）中存储。
 2. **值的比较**
 

```js
var n = 1;
var m = true;
console.log(n == m);//true
console.log(n === m);// false
```
"=="：只进行值的比较，会进行数据类型转换；

"==="：不会转换数据类型。


 
 
#### 引用数据类型
 
 1. **值是可变的**
  ```js
var person = {
		  name:'jake',
          age:22,
          action:function () {
           console.log("do something!")
          }
        }
person.age = 23;
console.log(person.age)// 23
```
有上面的代码可看出引用数据 类型可以拥有一个或多个属性和方法，而且是可以动态修改的。
 


 2. **同时存放在栈内存和堆内存**
  引用数据类型是存放在堆（heap）中的对象，占据空间大、大小不固定，如果存放在栈中，会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址，当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。（有关js中栈和堆的详解可参考文章[(https://www.cnblogs.com/heioray/p/9487093.html)）
 3. **值的比较**
当从一个变量向另一个变量赋引用类型的值时，同样也会将存储在变量中的对象的值复制一份到位新变量分配的空间中。

```js
	var person1 = {
			  age:20
      }
      var person2 = person1;
			person2.age = 23;
			console.log(person1.age = person2.age)// true
```
前面讲到基本数据类型和引用数据类型存储于内存中的位置不一样，引用数据类型存储在堆中的对象，与此同时，在栈中存储了指针，而这个指针的指向正是堆中实体的起始位置。变量person1初始化时，person1指针指向该对象{age：20}的地址，将person1赋给person2后，person2又指向该对象{age：20}的地址，这两个变量指向了同一个对象。因此改变其中任何一个变量，都会相互影响。
![ZJK](https://user-gold-cdn.xitu.io/2019/11/9/16e50425886c10a9?w=648&h=247&f=png&s=31806)此时，如果取消某一个变量对于原对象的引用，不会影响到另一个变量。

```js
	var a = {age:22}
			var b = a;
			a = 1;
			console.log(b);//{age:22}
```
上面代码中，a和b指向同一个对象，然后a的值变为1，这时不会对b产生影响，b还是指向原来的那个对象。 

## 后话
小生乃前端小白一枚，写文章的最初衷是作学习笔记，为了让自己对该知识点有更深刻的印象和理解，写的东西也很小白，文中如有不对，欢迎指正~ 然后就是希望看完的朋友点个喜欢，也可以关注一波~ 我会持续输出！

[个人博客链接](http://jkvin.cn)

[CSDN个人主页](https://blog.csdn.net/qq_39735040)

[掘金个人主页](https://juejin.im/user/5d999147f265da5b9764b996)

[简书个人主页](https://www.jianshu.com/u/370e80e9707d)
