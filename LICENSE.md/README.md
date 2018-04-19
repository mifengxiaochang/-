# 函数式编程
今天早上米清醒，误删了昨天整理的东西┭┮﹏┭┮
从头再来吧哎

 - 什么是函数式编程
 
 ~~纪委~~ 维基百科中缩函数式编程是一种编程范型，~~它将电脑运算视为数学上的函数计算，并且避免使用程序状态以及易变对象
 。~~ 
 
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

- 柯里化
我理解的柯里化就是**将一个低阶函数转换为高阶函数的过程**






