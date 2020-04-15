
![](https://user-gold-cdn.xitu.io/2020/4/3/1713eb34d74e7c08?w=2151&h=1745&f=png&s=259533)
### 1. 前言
关于JS原型和原型链我之前刚学js就有写过一篇学习笔记形式的博客，但前两天翻出来一看——什么鬼，这是我写的吗？自己都看不懂了，于是我重新整理思路，今天 **「前端料包」** 带来第四篇。

原型和原型链是js中的难点也是重点，有句话说，没理解透原型和原型链，就算还没有真正入门的前端。并且原型和原型链会是面试中必不可少的话题。看完这篇相信你能对原型和原型链会有个深刻的理解。如果觉得本文对你有所帮助请点亮左边的👍，不甚感激🙆
### 2. 函数对象
JS所有对象分为函数对象和普通对象。凡是通过new Function()创建得来的对象都是函数对象。函数对象拥有prototype属性和__proto__属性。


**prototype**
> JS设计之初为了实现简单继承，引入了prototype属性，也叫原型对象（显式原型）。

```js
//原型对象
function Animal(){};  

console.log(typeof Animal.prototype) //Object  
console.log(typeof Object.prototype) // Object  
```
可以看出，从本质上讲，prototype是一个普通对象，是函数对象的构造函数创建的一个实例。相当于在Animal创建的时候，自动创建了一个它的实例，并且把这个实例赋值给了prototype。

但是存在一个特例Function， Function.prototype是原型对象，本质却是函数对象。作为一个函数对象，又没有prototype属性。

```js
console.log(typeof Function.prototype) // 特殊 Function  
console.log(typeof Function.prototype.prototype) //undefined 函数对象却没有prototype属性
```

**__proto__属性**

> **所有的对象**obj(null和undefined除外)都具有__proto__属性（隐式原型），__proto__属性在本质上为一个指针，指向函数对象的prototype属性。

```js
//创建构造函数
        function Animal(name,age){
            this.name = name;
            this.age= age;
        }
        Animal.prototype = {
            alertName(){
                alert(this.name);
            }
        }
        //创建实例
        var dog = new Animal("大黄");
        dog .print = function(){
             alert(this.name);
        }
        dog.print();  //大黄
        dog.alertName();  //大黄

```
 print()方法是dog实例本身具有的方法，所以`dog.print()`输出“大黄”；`alertName()`不属于dog实例的方法，属于构造函数的方法，`dog.alertName()`也会输出“大黄”，是因为dog实例继承了构造函数的方法。
 
 事实上，实例dog的隐式原型（`__proto__`）指向它构造函数的显式原型（`prototype`），指向的意思是等价于，即

```js
dog.__proto__ === Animal.prototype// true
```
我们可以通过一张图来帮助理解记忆
![](https://user-gold-cdn.xitu.io/2020/4/3/1713bda7c6545f5b?w=707&h=348&f=png&s=22132)

### 3. 构造器constructor
顾名思义，构造器constructor就是用来构造函数对象的，constructor 属性返回对创建此对象的函数对象的引用。通俗了讲就是指向当前对象的爸爸
```js
function Animal(){};
console.log(Animal.constructor===Function); //true
console.log(Animal.prototype.constructor===Animal); //true
```
函数Animal是由Function创造出来的,那么它的constructor指向的Function,`Animal.prototype`是由new Animal()的方式创造出来,那么`Animal.prototype.constructor`理应指向Animal。
同样，我们用图来帮助记忆
    
![](https://user-gold-cdn.xitu.io/2020/4/3/1713beca0f760ab6?w=725&h=438&f=png&s=29077)

> 留个思考题：`Animal.prototype._proto_.constructor`指向谁? ，欢迎在评论区留下你的答案

### 4. 原型链
原型链是JS中实现继承的主要方法。其基本思想就是让一个引用类型继承另一个引用类型的属性和方法。下面就是我们最常见的组合继承
```js
function Animal(){  
    this.animalType = "animal";  
}  
Animal.prototype.getAnimalType = function(){  
    return this.animalType ;  
}  

function Dog(){  
    this.Dogtype = "dog";  
}  
Dog.prototype = new Animal();  

Dog.prototype.getDogType = function(){  
    return this.Dogtype ;  
}  

var dahuang = new Dog();

alert(dahuang.getAnimalType ());// animal
```
`dahuang.getAnimalType ()`打印结果为`animal，dahuang`自身没有`getAnimalType ()`方法，那么就会去它的`_proto_`(即它的构造函数的`prototype`)中寻找，发现Dog中也没有，于是顺着`_proto_`再往上找，在`Animal.prototype.getAnimalType`找到，返回结果 。

`如果Animal中还是没有，就接着往上找，一直到Object.prototype原型对象终止`

![](https://user-gold-cdn.xitu.io/2020/4/3/1713bfa7dcac5d49?w=1067&h=557&f=png&s=45845)

总结得出**原型链**就是：由原型对象组成，每个对象都有 `__proto__` 属性，指向了创建该对象的构造函数的原型，`__proto__` 将对象连接起来组成了原型链。是一个用来实现继承和共享属性的有限的对象链。


*    **属性查找机制:** 当查找对象的属性时，如果实例对象自身不存在该属性，则沿着原型链往上一级查找，找到时则输出，不存在时，则继续沿着原型链往上一级查找，直至最顶级的原型对象Object.prototype，如还是没找到，则输出undefined；
* **属性修改机制:** 只会修改实例对象本身的属性，如果不存在，则进行添加该属性，如果需要修改原型的属性时，则可以用: `b.prototype.x = 2`；但是这样会造成所有继承于该对象的实例的属性发生改变。

### 5. 原型对象和原型链他们之间到底起什么作用？
如果构造函数中有很多属性和方法，那么构造函数所有的实例化对象都是公用这些属性和方法的，当有多个实例想用共用这些东西的时候，每个实例都拷贝一份，就造成极大的资源浪费，那是不是可以考虑存把这些需要共用的属性和方法放到一个共同的东西上。这个共同的东西就是原型对象（`prototype`）。

`当然原型链实现继承也会存在一些问题，最主要的问题来自包含引用类型的原型以及在继承父类函数的时候调用了父类构造函数，导致子类的原型上多了不需要的父类属性，存在内存上的浪费。其次就是在创建子类型的实例时，不能向超类型的构造函数中传递参数。解决办法将在后续的文章中揭晓~`
### 后话
JavaScript的原型和原型链的确晦涩难懂，据说搞了几年前端的老司机都不一定绕的通，但通过实例和画图来帮助理解还是可以的。最后，小生乃前端小白一枚，写文章的最初衷是为了让自己对该知识点有更深刻的印象和理解，写的东西也很小白，文中如有不对，欢迎指正~  然后就是希望看完的朋友可以点个喜欢，也可以关注一波~  我会持续输出！

[个人博客链接](http://jkvin.cn) 

[CSDN个人主页](https://blog.csdn.net/qq_39735040) 

[掘金个人主页](https://juejin.im/user/5d999147f265da5b9764b996)

[简书个人主页](https://www.jianshu.com/u/370e80e9707d)