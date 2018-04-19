# 函数式编程
今天早上米清醒，误删了昨天整理的东西┭┮﹏┭┮
从头再来吧哎

 - 什么是函数式编程
 
 ~~纪委~~ 维基百科中缩函数式编程是一种编程范型，~~它将电脑运算视为数学上的函数计算，并且避免使用程序状态以及易变对象

 
 这种教科书式语言我是完全没搞懂，我理解就是:只有表达式木有语句过程（**不带return的句子**）。
 还是举个栗子，代码走一走
 ``` JS
function factorial(n) {   //上学总是考滴阶乘
  if (n == 1) {  
    return 1;  
  } else {  
    return factorial(n - 1) * n;  
  }  
}  
  
  
// 函数式编程风
function mul(a, b){  return a*b; } 
function dec(x){  return x - 1; }   
function equal(a, b){  return a==b; }   
  
function factorial(n) {  
  if (equal(n, 1)) {  
    return 1;  
  } else {  
    return mul(n, factorial(dec(n)));  
  }  
} 
 
 ```
由此可见，函数式优点：
1. 代码简洁，开发快速
2. 接近自然语言，易于理解
3. 更方便的代码管理
4. 易于"并发编程"
5. 代码的热升级

上面的栗子偶们应该或多或少都用过,就是可能有的人并不知道他叫函数式编程，下面偶们搞一下令人头晕的
## 纯~~真~~函数
直接举栗子瞅瞅
```JS
// 不纯，结果除了由age决定还有minimum决定
var minimum = 18;
var checkAge = function(age) {
  return age >= minimum;
};

//纯的
var checkAge = function(age) {
  return age >= 18;
};

```
由此可以看出**纯函数的输出，仅仅取决于输入值。**无论何时何地被调用，只要输入值相同，返回值也就一样。但是checkAge的扩展性显然很不友好。
所以偶们需要介绍几个知识点

- 高阶函数

声明一个将另一个函数作为参数或返回值的函数，额，有点儿绕口
举栗子
```JS
var add = function ( x, y ) {
  return x + y;
};

add(2,3)//=>5

//进化后
var add = function(x){
    this.addy = function (y){
        return x+y;
    };
    return addy;
};
//调用
add(2)(3)//=>5

```
通过调用控制台我们可以查出

这里add(2)是一个函数返回的是addy函数，而（3）是addy的参数
执行的过程是从右往左

- 6-箭头函数
这个问题可大可小还是戳→→[大叔讲箭头](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001438565969057627e5435793645b7acaee3b6869d1374000)

- es6-解构
方便滴从对象或数组中提取数据
```JS
//Array
let [x,y]=[1,2];//x=1,y=2

//ES5
var arr=[1,2];
var x=arr[0];
var y=arr[1];

//Object
function obj() {
  return { foo: 'bar' };
}
var {foo: fu} = obj();//foo:'bar'
```
看着挺好消化哈，再看个栗子
```JS
var a = {};
var b = a;
[a.x, a.y, a.x] = [1, (a = {}), 3];
```
四不四想说a.x=3,a.y={},b = { x: 3, y: Object }
现实的结果
a = { x: 3, y: Object } // 其中的y就是a的引用
b = {}
偶们慢慢撸一下es6解构的步骤

1. 先分析左侧，得到一个赋值模式（AssignmentPattern）

2. 计算右侧，得到一个值

3. 按照左侧的模式，将右侧的值当中一部分赋值到左侧
上面慢慢分步就是
```JS
var a = {};
var b = a;
var _tmp = [1, (a = {}), 3];
a.x = _tmp[0];
a.y = _tmp[1];
a.x = _tmp[2];
```
由此可见，
```
var _tmp = [1, (a = {}), 3];
```
中a赋了个{},与b指导不再是一个对象

同时新的a.x = 1,a.y=a这里行程了一个循环引用之后新的a.x又被赋了新值，而b还是那个{}
分析之后我们就可以得出一个小李子
```JS
const [first, ...rest] = [1, 2, 3, 4, 5] //=>first = 1,rest=[2,3,4,5]
```
- 柯里化
我理解的柯里化就是**将一个低阶函数转换为高阶函数的过程**

网上有个图片表述的还蛮有道理滴

![吃豆豆](../img/curry.gif)

上面的add栗子curry后
```JS
//es5
var add = function(x) {
  return function(y) {
    return x + y;
  };
};

//es6
var add = x => (y => x + y);

```
编一个公式大概是这样
``` JS
var curry = fn => (...left) => (...right) => fn(...left, ...right)
```
实际运用将上面的checkage颗粒一下
```JS
//纯的
var checkAge = function(age) {
  return age >= 18;
};

//curry
var checkAge = minnum => age => age >= minnum;

//调用
checkAge(18)(20);//=>true验证20是否小于18
```
作用：
 1. 参数复用；
 
 2. 提前返回；
 
 3. 延迟计算/运行。
 介绍完这些知识点我们开始弄下好玩滴东西
 
 ## 循环改写
 普通的循环大家都知道
 ```JS
 for (var i = 0; i < arr.length; i++) {
    // dosomething..
}
 ```
要用纯函数，首先我们要限制几个条件
- 用const定义，禁止含有变量。
- 不让“顺序执行”，解除过程式编程范式。这次不要忘记不加分号哦
- 用条件表达式替代if/else
- 不用for/while/do-while。
- 不用prototype和this来解除JS中的面向对象编程范式。
- 不用function和return关键字，只用箭头函数。
- 取消多参数函数，相当于强行~~颗粒~~curry化

根据条件我们可以简单用递归进化一下
```JS
var loop_on_array = (arr, body, i)=> {
    i < arr.length ? body(arr[i])loop_on_array(arr, body, i + 1) : null
}

```
还有两点需要优化，1.没柯里化 2. 顺序实行问题
```JS
//针对1.cuury
const loop_on_array = arr=> body => i => i< arr.length ? body(arr[i])loop_on_array(arr, body, i + 1) : null//先表达那么个意思

//针对2

const two_steps = step1 => step2 => param => step2(step1(param))//step1是body ，step2是_on_array
```
注意param它是一个函数，而并不是直接loop(arr, body, i + 1)，它所接收的是body(arr[i])的结果，但是它并不需要这个结果。用_来表示忽略不用的参数。

进化后
```JS
const two_steps = step1 => step2 => param => step2(step1(param))
const loop_on_array = arr => body => i => i < arr.length ? two_steps (body) (_ => loop_on_array(arr)(body)(i + 1)) (arr[i]) :null
//调用
loop_on_array ([1, 2, 3, 4, 5]) (item => console.log(item)) (0)
```
调用的不太好看再进化下
```JS
const two_steps = step1 => step2 => param => step2(step1(param))
const _loop_array = arr => body => i => i < arr.length ? two_steps (body) (_ => _loop_array(arr)(body)(i + 1)) (arr[i]) :null
const loop_on_array = arr => body =>_loop_array(arr)(body)(0)

//调用
loop_on_array([1,2,3,4])(item=>console.log(item))

```
饿了，有空再续~~

