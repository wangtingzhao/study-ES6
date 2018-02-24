## ECMAScript 2015  ES6

>支持
>> ECMAScript 6  的兼容
  //  http://kangax.github.io/es5-compat-table/es6
>> 兼容包
https://github.com/paulmilr/es6-shim

>支持环境
```
 Traceur 转码器
 <!-- 加载Traceur编译器 -->
 <script src='http://google.github.io/traceur-compiler/bin/traceur.js' type='text/javascript'></script>
<!-- 将Traceur编译器用于网页 -->
<script src='http://google.github.io/traceur-compiler/src/bootstrap.js' type='text/javascript' ></script>
 <!-- 打开实验选项,否则有些特性可能编译不成功 -->
 <script>
    traceur.options.experimental = true;
 </script>
 <!-- ES6转ES5 -->
 <!-- script 标签的type 属性的值是module(或者是Traceur),而不是text/javascript。
 这是Traceur编译器识别ES6代码的标识，编译器会自动将所有type=module的代码转化为ES5,然后交给浏览器执行。 -->
 源代码
 <script type='module'>
    class Calc{
        constructor() {
            cosnole.log('Calc constructor');
        }
        add(a,b){
            return a+b;
        }
    }
    var c = new Calc();
    console.log(c.add(4,5));
 </script>
```

# ES6 学习

> 1、let 命令
>>(1)基本用法
```
<script>
var console.log = log;
</script>
// ES6 新增了let 命令,用来声明变量。它的用法类似于var,但是所声明的变量,只在let命令所在的代码块内有。
<script>
    var console.log = log;
    var a = 100;
    let b = 200;
    log(a) // 100
    log(b) // 200
    {
        var a = 300;
        let b = 400;
    }
    log(a) // 300;
    log(b) // is not defined -- Error
</script>
```
>>(2) 不存在变量提升
```
// let 不像var那样，会发生“变量提升”现象。
    var log = console.log;
    <!-- ES5 -->
    var a = [];
    for(var i=0;i<10;i++){
        var c = i;
        a[i] = function(){
            log(c);
        }
    }
    a[5]() // 9
    <!-- ES6 -->
    var d = [];
    for(var i=0;i<10;i++){
        let b = i;
        d[i] = function(){
            log(b)
        }
    }
    d[5]() // 5
```
>>(3)暂时性死区
```
// 只要块级作用域内存在的let命令,它所声明的变量就“绑定”这个区域,不在受外部的影响.
{
    console.log(a); //  is not defined 暂时性死区
    let a = 100;
    console.log(a) // 100
}
<!-- 不受外界影响 -->
var a = 100;
{
    console.log(a); // undefined 
    let a = 100;
    console.log(a);// 100
}
```
>>(4)不允许重复声明
```
// let 不允许在相同作用域内,重复声明一个变量.

<!-- 模块之间不影响，可以声明相同变量 -->
{
    var a = 200;
    var a = 300;
    console.log(a); // 300 
}
{
    let a = 400;
    console.log(a) // 400;
}

<!-- 模块内部不可重复声明 let命令 -->
{
    var a = 1;
    let a = 2;
}
```

## 块级作用域


    为什么需要块级作用域? 
        ES5只有全局作用域和函数作用域,没有块级作用域,这带来很多不合理的场景.
        第一种场景, 内层变量可能会覆盖外层变量.
        第二种场景, 用来计数的循环变量泄露为全局变量
    ES6的块级作用域 
        let实际上为javascript新增了块级作用域.   


```
场景一
<script>
    var time = new Date();
    function fun(){
        console.log(time);
        if(false){
            var time = 'Hello World!';
        }
    }
    // 内层变量可能会覆盖外层变量
    fun() // undefined
</script>
场景二
<script>
    var s = 'Hello World!';
    for(var i=0;i<s.length;i++){
        console.log(s[i]);
    }
    console.log('循环结束!');
    console.log(i)// 12 计数循环变量泄露为全局变量
</script>
块级作用域
<script>
cosnole.log('ES6:')
fun () {
    let num = 100;
    if(true){
        let num = 200;
    }
    console.log(num);
};
fun();

 var foo = 'aaa';
 function bur ( fun = () => foo ){
     var foo = 'bbb';
     cosnole.log(foo());
 }
 bur() // aaa;

  var foo = 'aaa';
 function bur ( fun = () => foo ){
     foo = 'bbb';
     cosnole.log(foo());
 }
 bur() // bbb;
console.log('ES5:')
    function fun (){
        var nums = 200;
        if(true){
            var nums = 300;
        }
        console.log(nums);
    }



</script>
立即执行函数
<script>
    console.log('ES5:')
    function fun5 () {
        console.log('I am outside!');
    };
    (function () {
        if(false){
            function fun5 () {
                console.log('I am inside!');
            };
        };
        fun(); //  I am outside;
    } ());
    console.log('ES6:')
    function fun6() {
        console.log('I am outside!');
    }
    (function () {
        if(false){
            function fun6() {
                console.log('I am inside!')
            }
        }
        fun(); // I am outside;
    }())
</script>
```

>2 const 命令
```
const 也用来声明变量,但是声明的是常量.
一旦声明,常量的值无法改变.
```
   