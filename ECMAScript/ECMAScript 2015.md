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
<script>
const pi = 3.1415926535;
console.log(pi);
pi = 3; // 报错 PI is read-only -- Error;
console.log(pi) 
<!-- 块级作用域 -->
if(false){
    cosnt s = '123';
}
console.log(s); // s is not defined;

<!-- 暂时性死区 -->
if(true){
    console.log(s);
    const s = '123';
}
<!-- 不可重复声明 -->
{   
    const s = 1;
    const s = 2; 
    console.log(s);  //  Identifier 'a' has already been declared
}

//const 对象
const obj = {};
obj.name = '张三';
obj.age = 29;
console.log(obj,obj.name,obj.age); Object { name:'张三',age: 29 }  张三   29

// const 对象结冻；

const obj1 = Object.freeze({
    name:'张三'，
    age :29
});
console.log(obj1,obj1.name,obj1.age);

// 对象结冻函数
function freeze(obj){
    Object.freeze(obj);
    Object.keys(obj).forEach((key, value) => {
        if(typeof obj[key] === 'object'){
            freeze(obj[key])
        }
    })
} 
const freeze = (obj) =>{
    Object.freeze(obj);
    Object.keys(obj).forEach((key, value) =>{
        if(typeof obj[key] === 'object' ){
            freeze(obj[key]);
        };
    });
};
</script>
 // 跨模块常量
 <cript>
    // module.js
    export const intVariantName = 100;
    export const FloatVariantName = 3.1415926;
    export const chatVariantName = 'variantValue';

    // use.js
    import * as variant from './module';
    console.log(variant.intVariantName);
    console.log(variant.FloatVariantName);
    console.log(variant.chatVariantName);

    // othuse.js
    import { FloatVariantName, chatVariantName } as variant from './module'
    console.log(variant.FloatVariantName);
    console.log(variant.chatVariantName);

    // onlyuse.js
    import intVariantName as variant from './module.js'
    console.log(variant.intVariantName);
 </script>
 // 全局对象的属性
<script>
    /*
        全局对象是最顶层的对象，在浏览器环境是指Window对象，在Nodejs指的是global对象。在javascript语言中，所有全局变量都是全局对象的属性。（Node的情况比较特殊，这一条对ERPL环境适用，模块环境必须显式声明成global的属性。）

        ES6规定，var命令，function命令声明的全局变量，属于全局对象的属性；
        let命令，const命令,class命令声明的全局变量，不属于全局对象的属性。
    */
    
    var varName = 'varValue';
    // 浏览器环境下
    console.log(window.varName);
    // Node环境下
    console.log(global.varName);
    // 通用环境下
    console.log(this.varName);

    /*
        use strict
        let 声明的变量在严格模式下会使用window.letName 会报错 undefined  
    */ 
    let letName = 'letValue';
    console.log(window.letName); 
    console.log(this.letName);
</script>
```

## 变量的解构赋值
1、数组的解构赋值
```
    /*
        Destructuring
         ES6允许按照一定的模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）;

        不完全解构
         等号左边的模式，只匹配一部分的等号右边的数组。

        指定默认值
         * 注意 ：ES6内部使用严格模式相等运算符（===）判断一个位置是否有值。所以，如果一个数组成员不严格等于undefined，默认值是不会生效的。
        
        let 和 const 命令
            只要某种数据结构具有lterator接口，都可以采用数组形式的解构赋值
    */
    <script>
        // 数组解构赋值使用
        let [ a, b, c ] = [ 1, 2, 3 ];
        console.log(a);
        console.log(b);
        console.log(c);
        
        // 对应位置
        let [A, [ [B], C ] ] = [ 1,[ [ 2 ], 3 ] ];
        console.log(A);
        console.log(B);
        console.log(C);

        let [ , , three ] = [1,2,3];
        console.log(three);

        let [ One, , Three ] = [1,2,3];
        console.log(One);
        console.log(Three);

        let [ head, ...tial ] = [0,1,2,3,4,5,6,7,8,9];
        console.log(head);
        cosnole.log(tial);
        
        // 数组解构不成功

        let [temp] = [];
        console.log(temp);

        let [ temp, temp1 ] = [100];
        console.log(temp);
        console.log(temp1);
        
        // 不完全解构
        
        let [x, y] = [1, 2, 3];
        console.log(x); // 1
        console.log(y); // 2

        let [a, [b], c] = [1,[2, 3], 4];
        console.log(a);
        console.log(b);
        console.log(c);
        
        // 指定默认值
        let [temp = 'string'] = [];
        console.log(temp); // string;
        let [temp = 'string'] = [strings];
        console.log(temp); // strings 
        let [a = 'aaa', b] = ['bbb'];
        console.log(a); // bbb
        console.log(b); // undefined
        let [a, b = 'aaa'] = ['bbb];
        console.log(a); // bbb
        console.log(b); // aaa
        let [a, b = 'bbb'] = ['aaa',undefined];
        console.log(a); // aaa
        console.log(b); // bbb
        
        // Iterator 接口
        let [a, b, c] = new Set(['a','b','c']);
        console.log(a);
        console.log(b);
        console.log(c);

        function* fibs(){
            let a = 0;
            let b = 1;
            while (true) {
                yield a;
                [a, b] = [b, a + b]; s      
            }
        }
        var [first, second, third, fourth, fifth, sixth] = fibs();
        console.log(sixth);

    </script>
```
2、对象的解构赋值
```
/*
    解构不仅可以用于数组，还可以用于对象
        对象的属性没有次序，变量必须与属性同名，才能取到正确的值。
    
    指定的默认值
        默认值生效的条件是，对象的属性严格等于undefined。
    
    现有对象的方法
        对象解构赋值，可以很方便地将现有对象的方法，赋值到某个变量。
*/
<script>
    let {name, age} = {name:'张三', age: 18};
    console.log(name);
    console.log(age);

    let { person_id, person_name, person_age } = {name:'张三', age: 18, id: '007'};
    console.log(person_id);     // undefined
    console.log(person_name);   // undefined
    console.log(preson_age);    // undefined

    let { id: person_id, name: person_name, age: person_age } = {name:'张三', age: 18, id: '008'};

    console.log(person_name);       // 张三
    console.log(person_age);        // 18
    console.log(person_id);         // 008

    let object = { frist: 'Hello', last: 'World'};
    let { frist: fristName, last: lastName } = object;
    console.log(fristName);
    console.log(lastName);
    
    // 默认值
    let { x = 3 } = {};
    console.log(x);

    let {x, y = 3} = {x:1};
    console.log(x);
    console.log(y);

    let {message: msg = 'You Are Person'} = {};
    console.log(msg);
    
    // 对象解构严格条件等于undefined 
    let {x = 1;} = {x:undefined};
    console.log(x); // 1

    let {y = 3} = {y = null};
    console.log(y) // unll 
    
    // 已声明变量的解构

    /*
        报错写法：
        var s ;
        {s} = {s:1};
        console.log(s);
    */
    // 正确写法
    var s ;
    ({s} = {s:1});
    console.log(s);

    // 现有对象的方法
        console.log(Math.sin(Math.PI/6));   //0.49999999999999994
        
        let {sin, cos, tan, log} = Math;
        console.log(sin(Math.PI/6));     
</script>

```
3、字符串的j解构赋值
```
/*
    字符串也可以解构赋值
        字符串被转换成为一个类似数组的对象  

    属性解构赋值
        类似数组的对象都有一个leng属性,因此还可以对这个属性解构赋值。
*/
<script>
    const = [a, b, c, d, e] = 'hello';
    console.log(a) // h
    console.log(b) // e
    console.log(c) // l
    console.log(d) // l
    console.log(e) // o

    // 字符串的length 属性
    const {length: len} = 'hello';
    console.log(len); // 5

    const {length} = 'hello world!';
    console.log(length); // 12

</script>
```
4、函数的解构赋值
````
/*
    函数的参数也可以解构赋值

    函数参数的解构也可以使用默认值
*/
<script> 
    // 函数参数解构赋值
    fun sum ({x,y}){
        return x + y;
    }
    console.log(sum({x:1, y: 2})) // 3

    // 函数参数解构赋值的默认值
    function fun({x: 0, y: 0} = {}){
        return [x, y];
    }
    console.log(fun({x: 100, y: 200}));   // [100,200]
    console.log(fun({x: 100}));           // [100,0]
    console.log(fun({}));                 // [0,0]
    console.log(fun());                   // [0,0]

    function funs({x , y} = {x: 0, y: 0}) {
        return [x, y];
    }
    console.log(fun({x: 100, y: 200}));   // [100,200]
    console.log(fun({x: 100}));           // [100,undefined]
    console.log(fun({}));                 // [undefined,undefined]
    console.log(fun());                   // [0,0]
</script>
````
5、解构赋值的用途
````
/*
    交换变量的值
    从函数返回多个值
    函数参数的定义
    提取json数据
    函数参数的默认值
    遍历Map的结构
    输入模板的指定方法
*/
<script>
    /*
        交换变量
    */
    <!-- ES5 -->
    var a  = 100,
        b  = 200,
        temp ;
    <!-- 交换前 -->
    console.log('a',a);   // 100
    console.log('b',b);   // 200
    temp = a;
    a = b;
    b = temp;
    <!-- 交换后 -->
    console.log('a',a);   // 200
    console.log('b',b);   // 100

    <!-- ES6 -->
    let [x, y] = [100,200];
    <!-- 交换前 -->
    console.log('x',x);   // 100
    console.log('y',y);   // 200
    [x, y] = [y, x];
    <!-- 交换后 -->
    console.log('x',x);   // 200
    console.log('y',y);   // 100
</script>
````