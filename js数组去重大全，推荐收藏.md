**情境：**

`将数组var arr  = [1,1,‘true’,‘true’,true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,‘NaN’,` `0, 0, ‘a’, ‘a’,{},{}];中重复的值过滤掉`

 
## 1、 ES6-set

使用ES6中的set是最简单的去重方法


```js
var arr  = [1,1,'true','true',true,true,15,15,false,false, undefined,
undefined, null,null, NaN,NaN,'NaN', 0, 0, 'a', 'a',{},{}];
 
function arr_unique1(arr){
return  [...new Set(arr)];
//或者
//return  Array.from(new Set(arr));
}
arr_unique1(arr); // (13)[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]
```

该方法可以说是最完美的方法，就是需要环境支持ES6

 



## 2、利用Map数据结构去重

创建一个空Map数据结构，遍历需要去重的数组，把数组的每一个元素作为key存到Map中。由于Map中不会出现相同的key值，所以最终得到的就是去重后的结果。


```js
function arr_unique2(arr) {
  let map = new Map();
  let array = new Array();  // 数组用于返回结果
  for (let i = 0; i < arr.length; i++) {
    if(map .has(arr[i])) {  // 如果有该key值
      map .set(arr[i], true);
    } else {
      map .set(arr[i], false);   // 如果没有该key值
      array .push(arr[i]);
    }
  }
  return array ;
}

 console.log(arr_unique2(arr)); //(13) [1, "a", "true", true, 15, false, 1, {…}, null, NaN, NaN, "NaN", 0, "a", {…}, undefined]
```



## 3、 利用递归去重


```js
function arr_unique3(arr) {
     var array= arr;
     var len = array.length;
     array.sort(function(a,b){   //排序后更加方便去重
     return a - b;
    })
    
 function loop(index){
        if(index >= 1){
            if(array[index] === array[index-1]){
                array.splice(index,1);
            }
            loop(index - 1);    //递归loop，然后数组去重
        }
    }
    loop(len-1);
    return array;
}
 
console.log(arr_unique3(arr)); //(14) [1, "a", "true", true, 15, false, 1, {…}, null, NaN, NaN, "NaN", 0, "a", {…}, undefined]
```


 

 

## 4、  forEach + indexOf


```js
function arr_unique4(arr){
var res = [];
arr.forEach((val,index)=>{
if( res.indexOf(val) === -1 ){
res.push(val);
}
});
return res;
}

console.log(arr_unique4(arr)); // (14) [1, "true", true, 15, false, undefined, null, NaN, NaN, "NaN", 0, "a", {…}, {…}]
```


该方法的不足之处在于无法对NaN进行过滤，原因是var a = [1, NaN , 2]; a.indexOf(NaN) === -1;，改善的方法是使用includes方法

 



## 5、 filter+indexOf


```js
function arr_unique5(arr){
return  arr.filter((val,index,item)=>{
return item.indexOf(val) === index;
});
}

arr_unique5(arr); // (12) [1, "true", true, 15, false, undefined, null, "NaN", 0, "a", {…}, {…}]
```

美中不足的地方在于漏掉了NaN,原因同方法四

 



## 6、 forEach + includes


```js
function arr_unique6(arr){
var res = [];
arr.forEach((val)=>{
if( ! res.includes(val) ){
res.push(val);
}
});
return res;
}
arr_unique6(arr); // (13) [1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]
```

该方法也算是比较完美，没有什么遗漏的地方





## 7、  reduce + includes


```js
function  arr_unique7(arr){
return arr.reduce( (prev, cur )=>{
if(  ! prev.includes(cur) ){
prev.push(cur);
}
return prev;
} ,[]);
}
arr_unique7(arr); // (13)[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]
```

 

## 8、 嵌套循环+splice


```js
function arr_unique8(arr){
for(var i = 0 ; i < arr.length; i++){
for( var j = i + 1; j < arr.length; j++){
if( arr[i] === arr[j] ){
arr.splice(j,1);
}
}
}
return arr;
}
arr_unique8(arr); // (14) [1, "true", true, 15, false, undefined, null, NaN, NaN, "NaN", 0, "a", {…}, {…}]
```

这是最麻烦的方法，效率十分低下，每一个循环都会去动态获取数组的length。且该方法无法过滤掉NaN，因为NaN === NaN的结果为false。





## 9、 hash+hasOwnProperty+JSON.stringify（终级版）


```js
function arr_unique9(arr){
var hash = {};
return arr.filter( (val)=>{
return hash.hasOwnProperty( typeof  val + JSON.stringify(val) ) ? false : hash[typeof val + JSON.stringify(val)] = true ;
});
}
arr_unique9(arr); // (12) [1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}]
```

这种方法是终极版的，因为它可以进行数组中的对象元素的去重！前面的几种方法是不可以进行对象去重的。不过话说回来，JS中的对象是本身就是一个地址的引用，比如 {} == {} ;//false，两者是两个不同的对象，这里我将其进行JSON.stringify进行简化。



经亲测，使用ES6的Set和Map效率最高，reduce()和sort()效率还可以，双层循环效率最低。

数组去重大全，你，get到了吗~

## 后话

小生乃前端小白一枚，写文章的最初衷是作学习笔记，为了让自己对该知识点有更深刻的印象和理解，写的东西也很小白，文中如有不对，欢迎指正~ 然后就是希望看完的童鞋可以点个喜欢，也可以关注一波~ 我会持续输出！

[`本人博客网站链接`](http://jkvin.cn)

[`CSDN个人主页`](https://blog.csdn.net/qq_39735040)

[`掘金个人主页`](https://juejin.im/user/5d999147f265da5b9764b996)

[`简书个人主页`](https://www.jianshu.com/u/370e80e9707d)
