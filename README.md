# study-ES6
ES6  学习


## let and const

### 作用域的概念
> let 和 var 区别

>{ } 块作用域    在{let} 在块作用域声明的let 只在块中应用


> let 在块级作用域里不可以let 同一个变量
````
 function test(){
     let a = 1;
     console.log(a);
 }
 test(); // 1

function test(){
    var a = 1;
    console.log(a)
} 
test(); //1;

for(var i=0;i<3;i++){
    console.log(i) // 0,1,2
}
console.log(i); // 3

for(let i=0;i<3;i++){
    console.log(i) // 0,1,2
}
console.log(i); // i in not defined;
```` 

### const

> ## const 用于声明常量;
    1.常量不可改变

````
    function test(){
        const a = 1;
        a = 23;
        console.log(a);
    }
    test(); //报错  
    
    function test(){
        const a = 1;
        const b = {
            c:1,
        };
        b.k = 2;
        console.log(a,b); 
    }
    test() // 对象可以改变;
````




## 解构赋值

 > 什么是解构赋值

 >解构赋值的分类
 > 1. 数组解构赋值 
 > 2. 对象解构赋值
 > 3. 字符串解构赋值
 > 4. 布尔值解构赋值
 > 5. 函数参数解构赋值
 > 6. 数组解构赋值


 # 1 数组结构赋值

`````
{
    let a,b;
    [a,b] = [1,2];
    console.log(a,b)
}
{ // c = 3 默认值 防止变量成为undefined;
    let a,b,c ;
    [a,b,c = 3] = [1,2];
    console.log(a,b,c); 
}
`````

# 2 对象解构赋值
````
{
    let {a,b} = {1,2}
    console.log(a,b);
}
{
    let o = {p:2,q:true};
    let {p,q} = o;
    console.log(p,q); 
}
{
    let metaDate = {
        title:'abc',
        test:[{
            title:'test',
            decs:'description'
        }]
    }
    let {title:esTitle,test:[title:cnTitle]} = metaDate;
    log(esTitle,cnTitle);
}

````


## 正则拓展

> 正则新增特性 

    1. 构造函数的变化
    2. 正则方法的扩展
    3. u修饰符
    4. y修饰符
    5. s修饰符

### 构造函数的变化
 
 ````
 
 {
     <!--  es5写法 -->
     let regex = new RegExp('xyz','i');
     let regex2 = new RegExp(/xyz/i)
     console.log(regex.text('xyz123'),regex2.text('xyz123')) //  true true
    <!-- 覆盖 第一个参数的修饰符 -->
     let  regex3 = new RegExp(/xyz/ig,'i');
     //新增属性 flags 获取修饰符  
     cosnole.log(regex.flags);
 }
 ````


 ###  y修饰符
 ````
 {
     let s = 'bbb_bb_b';
     let a1 = /b+/g; // 全局匹配
     let a2 = /b+/y; // 全局匹配 只匹配一次后面参数如果匹配不上返回null
     log(a1.exec(s),a2.exec(s));// bbb bbb 
     log(a1.exec(s),a2.exec(s)); // bb null
     //  sticky 判断正则是否用了y修饰符;
     console.log(a1.sticky,a2.sticky) //false true
 }
 ````

 ### u修饰符

 ````
 {
     <!-- unicode 匹配unicode编码 -->
      //  大于两个字节   . 不可匹配所有字符  只可匹配小于两个字节的字符;
        // u  =  unicode 只匹配 unicode编码
        log('u-1',/^\uD83D/.test('\uD83D\uDC2A'));   // true
        log('u-2',/^\uD83D/u.test('\uD83D\uDC2A'));  // false

        log(`\u{20BB7}`)
        let s = '𠮷';
        // 字符串大于两个字节加 u
        console.log('u',/^.$/.test(s));
        console.log('u',/^.$/u.test(s));
        log('test-1',/𠮷{2}/.test('𠮷𠮷'))
        log('test-2',/𠮷{2}/u.test('𠮷𠮷'))
 }      
 ````
