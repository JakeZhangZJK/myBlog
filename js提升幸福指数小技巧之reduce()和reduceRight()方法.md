昨天在看一篇提升js性能的文章中看到数组中reduce()方法的应用，由于我在之前的es6学习中没有了解过这个方法，在我所看的文章中感觉这个方法挺有意思的，然后我查了点资料，前后看了几篇文章，自己找了几个例子敲了敲，瞬间感觉内心美滋滋的，所以今天我总结了下，在这里小结一下reduce()方法的概念和几个小例子。

**1、reduce()方法**
reduce()方法接收一个函数callbackfn作为累加器（accumulator），数组中的每个值（从左到右）开始合并，最终为一个值。

**语法**

```js
array.reduce(callbackfn,[initialValue])
```

reduce()方法接收callbackfn函数，而这个函数包含四个参数：

```js
function callbackfn(preValue,curValue,index,array){}
```

preValue: 上一次调用回调返回的值，或者是提供的初始值（initialValue）
curValue: 数组中当前被处理的数组项
index: 当前数组项在数组中的索引值
array: 调用 reduce()方法的数组
而initialValue作为第一次调用 callbackfn函数的第一个参数。

reduce()方法为数组中的每一个元素依次执行回调函数callbackfn，不包括数组中被删除或从未被赋值的元素，接受四个参数：初始值（或者上一次回调函数的返回值），当前元素值，当前索引，调用 reduce() 的数组。

回调函数第一次执行时，preValue 和 curValue 可以是一个值，如果 initialValue 在调用 reduce() 时被提供，那么第一个 preValue 等于 initialValue ，并且curValue 等于数组中的第一个值；如果initialValue 未被提供，那么preValue 等于数组中的第一个值，`curValue等于数组中的第二个值。

来看一个示例：


```js
var arr = [0,1,2,3,4];
arr.reduce( (preValue,curValue,index,array) =>(preValue + curValue)) // 10
```
示例中的回调函数被执行四次，每次参数和返回的值如下：

<table>
    <thead>
        <tr>
            <th></th>
            <th>preValue</th>
            <th>curValue</th>
            <th>index</th>
            <th>arr</th>
             <th>返回值</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>第一次回调</td>
            <td>0</td>
            <td>1</td>
                <td>1</td>
            <td>[0,1,2,3,4]</td>
                <td>1</td>
        </tr>
       <tr>
            <td>第二次回调</td>
            <td>1</td>
            <td>2</td>
                <td>2</td>
            <td>[0,1,2,3,4]</td>
                <td>3</td>
        </tr>
         <tr>
            <td>第三次回调</td>
            <td>3</td>
            <td>3</td>
                <td>3</td>
            <td>[0,1,2,3,4]</td>
                <td>6</td>
        </tr>
         <tr>
            <td>第四次回调</td>
            <td>6</td>
            <td>4</td>
                <td>4</td>
            <td>[0,1,2,3,4]</td>
                <td>10</td>
        </tr>
    </tbody>
</table>

上面的示例reduce()方法没有提供initialValue初始值，接下来再上面的示例中，稍作修改，提供一个初始值，这个值为5。这个时候reduce()方法会执行五次回调，每次参数和返回的值如下：


```js
var arr = [0,1,2,3,4];

arr.reduce( (preValue,curValue,index,array) =>(preValue + curValue), 5); //15
```

<table>
    <thead>
        <tr>
            <th></th>
            <th>preValue</th>
            <th>curValue</th>
            <th>index</th>
            <th>arr</th>
             <th>返回值</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>第一次回调</td>
            <td>5</td>
            <td>0</td>
                <td>0</td>
            <td>[0,1,2,3,4]</td>
                <td>5</td>
        </tr>
       <tr>
            <td>第二次回调</td>
            <td>4</td>
            <td>1</td>
                <td>1</td>
            <td>[0,1,2,3,4]</td>
                <td>6</td>
        </tr>
         <tr>
            <td>第三次回调</td>
            <td>6</td>
            <td>2</td>
                <td>2</td>
            <td>[0,1,2,3,4]</td>
                <td>8</td>
        </tr>
         <tr>
            <td>第四次回调</td>
            <td>8</td>
            <td>3</td>
                <td>3</td>
            <td>[0,1,2,3,4]</td>
                <td>11</td>
        </tr>
          <tr>
            <td>第五次回调</td>
            <td>11</td>
            <td>4</td>
                <td>4</td>
            <td>[0,1,2,3,4]</td>
                <td>15</td>
        </tr>
    </tbody>
</table>
上面代码通过reduce()方法一行代码就实现了一个简单的求和运算，是不是感觉hin简洁哟~ 而且性能杠杠滴。
没有对比就没有伤害，我们来对比一下我们平时用的for循环和while循环的求和方法以及它们消耗的时间：

**for循环**

```js
console.time("forLoop");
var arr = [0,1,2,3,4];
Array.prototype.sum = function (){
    var sumResult = 0;
    for (var i = 0; i < this.length; i++) {
        sumResult += parseInt(this[i]);
    }
    return sumResult;
}

console.log(arr.sum()); // 10
console.timeEnd("forLoop");// forLoop: 0.370849609375ms
```

**while循环**


```js
console.time("whileLoop");
var arr = [0,1,2,3,4];

Array.prototype.sum = function () {
    var sumResult = 0;
    var i = this.length;
    while (i--) {
        sumResult += parseInt(this[i]);
    }
    return sumResult;
}

console.log(arr.sum()); // 10
console.timeEnd("whileLoop");//whileLoop: 0.97509765625ms
```


回头再看看刚刚写的reduce()方法所用时间：

```js
console.time("reduceLoop");
var arr = [0,1,2,3,4];
arr.reduce( (preValue,curValue,index,array) =>(preValue + curValue)) // 10
console.timeEnd("reduceLoop");//reduceLoop: 0.08984375ms
```
从下表可以明显的看出reduce()相较于for循环和while循环性能高了一个数量级，而且代码十分简洁，是不是瞬间有种感觉：原来还可以这样！！
<table>
    <thead>
        <tr>
            <th></th>
            <th>for循环</th>
            <th>while循环</th>
            <th>reduce()方法</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>消耗时间</td>
            <td>0.370849609375ms</td>
            <td>0.97509765625ms</td>
                <td>0.08984375ms</td>
        </tr>
    </tbody>
</table>

**2、几个小案例**

1）同时实现map和filter

```js
   //1.同时实现map和filter
      var numbers = [10,20,30,40];
      var doubleAndOver50 = numbers.reduce((resultList,num)=> {
        num = num*2;
        if (num > 50){
          resultList.push(num);
        }
        return resultList;
      },[]);
      doubleAndOver50;//[60, 80]
    
```
2）统计数组中相同项的个数

```js
  //2.统计数组中相同项的个数
      var cars = ["BMW","benz","benz","BMW","大众汽车","大众汽车","兰博基尼"];
      var carsObj = cars.reduce(function (obj,name) {
        obj[name] = obj[name]? ++obj[name]:1;
        return obj;
      },{});
      carsObj;  //{BMW: 2, benz: 2, 大众汽车: 2, 兰博基尼: 1}
```
3）数组去重

```js
var arr = [1,2,1,2,3,5,4,5,3,4,4,4,4];
var str = arr.sort().reduce((prev, cur)=>{
    if(prev.length===0 || prev[prev.length-1]!==cur){
        prev.push(cur);
    }
    return prev;
}, []);
str // (5) [1, 2, 3, 4, 5]

```
4）求阶乘

```js
var a = [1,2,3,4,5,6,7,8,9,10]
 
var str = a.reduce(function(prev,cur,index,arr){
	return prev * cur ;
})
 
str //3628800

```
5）将二维数组转为一位数组

```js
var a= [[0,1],[2,3],[4,5],[6,7],[8,9]];
 
var str = a.reduce(function(prev,cur){
	return prev.concat(cur)
})
 
str    //(10) [0,1, 2, 3, 4, 5, 6, 7, 8, 9]

```
**3、reduceRight()方法**
reduceRight()方法的功能和reduce()功能是一样的，不同的是reduceRight()从数组的末尾向前将数组中的数组项做累加，参数和上面是一样的。结果都是一样的我就不写代码了。




**小生乃牛犊一枚，如有不对，欢迎各路大佬下方评论指教~~**