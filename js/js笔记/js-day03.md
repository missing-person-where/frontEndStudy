# js-day03

## 函数

### 有参函数

```javascript
// 求start到end的值
function getSum(start, end){
    let s = 0
    for (let i = start; i <= end; ++i){
        s += i
    }
    console.log(s)
}
getSum(50, 100)
```



### 默认参数

```javascript
function getSum(x = 0, y = 0){
    // 函数不设置默认参数。那么默认值是undefined
    // console.log(x, y)
    document.write(x + y)
}
getSum(1, 2)
getSum()

// 求start到end的值
function getSum(start=0, end=0){
    let s = 0
    for (let i = start; i <= end; ++i){
        s += i
    }
    console.log(s)
}
getSum(50, 100)
```



### 有返回值的函数

```javascript
// 函数的返回值
function fn(){
    return 20
}
console.log(fn())  // fn() =20

// 求和
function add(x = 0, y = 0){
    return x + y
    // return后面的语句永远也不会执行
    console.log('hello World')
}
let sum = add(3, 4)
console.log(sum)
console.log(add(100, 300))

// 最大公约数 gcd
function gcd(a = 0, b = 0){
    return !b ? a : gcd(b, a % b)
}
let a = +prompt('输入一个数')
let b = +prompt('输入一个数')
console.log(`${a}和${b}的最大公约数为${gcd(a, b)}`)
```

### 返回多个值

```javascript
function getArrMax(arr = []){
    let max = arr[0]
    let min = arr[0]
    for (let i = 1; i < arr.length; ++i){
        if (arr[i] > max){
            max = arr[i]
        }
        arr[i] < min ? min = arr[i] : 0
    }
    // return max, min  // 逗号表达式，最后返回的是min
    return [max, min]
}
```

### 函数的细节

1. 如果有**同名函数**，后面的会覆盖前面的，**注意**，并**不是就近原则**

```javascript
function fn(){
    return 10
}
function fn(){
    return 20
}
let result = fn()
console.log(result) // 20
```



2. 函数不支持重载，虽然**参数不同**，但是**函数名相同**，**后面的函数**就一定会**覆盖前面的函数**

```javascript
function add(a, b){
    return a + b
}

function add(a, b, c){
    return a + b + c
}

let s = add(2, 3) // NaN
console.log(s)
s = add(2, 3, 4)
console.log(s) // 9
```



3. 函数**传参不一定需要匹配**

```javascript
function add(a, b){
      console.log(a + b)
    }
    // 1. 实参多于形参
    add(1, 2, 3)  // a = 1, b = 2, 3传不过去
    // 2. 实参等于形参
    add(1, 9)  // a = 1, b = 9  10
    // 3. 实参小于形参  
    add(1) // a = 1, b = undefined NaN
```

## 变量的作用域

### 全局作用域

在script里面第一层的变量都是**全局变量**

```javascript
let num = 10

console.log(num)

function fn() {
    console.log(num)
}
fn()
// 输出 10 10
```

### 局部作用域

在函数里面的变量叫做**局部变量**

```javascript
function fun(){
    let str = 'red'
    }
// console.log(str) 
// 报错，不能访问函数内的局部变量
```

### 变量的特殊情况

* 函数中的变量没**用let声明**而是直接赋值，那么只要这个函数被执行，这个变量在函数外部是可以被访问的，可以看作全局变量，但是及其不推荐这么用 

```javascript
let num = 20
    function fn(){
      num = 10  // 代码执行后，全局变量来看 强烈不推荐 
    }
    fn()
    console.log(num) // 10
```

* 函数的**形参**被看作是函数的**局部变量**

```javascript
function fun(x, y){
    // 形参看作是函数的局部变量
    console.log(x)
}
fun(1, 2)
console.log(x)  // 报错
```

* 变量的访问遵循**就近原则**

```javascript
let num = 10
function fn(){
    let num = 20
    function fun() {
        let num = 30
        console.log(num) // 就近原则
    }
    fun()
}
fn()
```

## 匿名函数

### 函数表达式

将一个匿名函数赋值给一个变量，相当于这个变量应用这个函数

```javascript
// 1. 函数表达式
fn(1, 2) // 错误，先声明后使用
let fn = function(a, b){
    // console.log('我是函数表达式')
    console.log(a + b)
}
console.log(fn)
fn(2, 3)  // 不报错
```

与具名函数的区别：

1. 具名函数的调用可以写到任何位置

2. 函数表达式，必须先写表达式（let修饰），后调用

```javascript
fun()  // 不报错
function fun(){
    console.log(1)
}
fun()
```

### 立即自动执行的匿名函数

* 写法1

  `(function(){})()`

* 写法2

  `(function(){}())`

* 简写

  `((形参列表) => 函数体)(实参列表)`

立即执行函数的优势：防止同名变量污染

**注意**：有**多个匿名函数**，必须要**用结束符 ; 分隔开**

```javascript
// let num = 10
// let num = 20
// (function(){
//   console.log(11)
// })()

// 立即执行函数的优势：防止同名变量污染
// (function(){
//   let num = 10
// })(); // 加分号结束

// (function(){
//   let num = 10
// })();

// 1. 第一种写法
(function(a, b){
    console.log(a + b)
})(1, 2);

// 简写
((a, b) => console.log(a * b))(1, 2);
// ((a, b) => console.log(a * b)(3, 2)); // 没有输出
// 存在多个匿名函数，必须 ; 分开 否则会报错

// 2. 第二种写法
(function (x, y) {
    console.log(x + y)
}(1, 3));

// (function(){})()
// (function(){}())
// (function fun(){}())
```

**转换时间案例**

将秒数，转换成**xx时yy分zz秒**的格式

```javascript
(function(){
    let second = +prompt('输入秒数')
    let h = parseInt(second / 60 / 60 % 24)
    let m = parseInt(second / 60 % 60)
    let s = parseInt(second % 60)
    h < 10 ? h = '0' + h : 0
    m = m < 10 ? '0' + m : m
    s = s < 10 ? '0' + s : s
    document.write(`<h2>转换成时分秒为${h}时${m}分${s}秒</h2>`)
})();
console.log(h)
```

## 逻辑中断

一个例子

```javascript
function fn(x, y){
    x = x || 0
    y = y || 0
    console.log(x + y)
}
fn(1, 2) 
// 对于这个传参，由于1和2都为真，所以x = 1, y = 2, 输出 3
fn()
// 对于没有传参的函数，由于x和y都为undefined，相当于假，因此x和y都会变成0，最后输出 0
// 总结：对于或运算，只要有一个为真，就不会去判断另外一个是不是真，最后结果就是这个真的，如果说没有为真的，最后结果是最后一个假的，事实上，或运算和与运算的结果未必是boolean类型的
```

```javascript
// console.log(false && 2) // false
// console.log(false && 3 + 5) // false
let age = 18
console.log(false && ++age) // false
// ++age不会执行 false直接中断
console.log(age)  // 18

console.log(true || age++) // true
console.log(age)  // 18

console.log(11 && 22)  // 都是真，返回最后一个真值(22)
console.log(11 || 22)  // 都是真，返回第一个值(11)
    
```

## 转换为boolean类型

`Boolean(字面量或变量)`可以将字面量或者变量转换为布尔类型

为false的情况：数字(0)， 数字(0.0)，空字符串('')，空对象(null)，未定义(undefined)，不是一个数字(NaN)

转换成数字参与运算的情况：

''和null会被转成0，undefined会被转换成NaN，但是NaN和所有值不相等包含它自己

```javascript
console.log(Boolean('red')); // true
console.log(Boolean('')) // false
console.log(Boolean(0)) // false
console.log(Boolean(90)) // true
console.log(Boolean(-90)) // true
console.log(Boolean(undefined)) // false
console.log(Boolean(null)) // false
console.log(Boolean(NaN))  // false

console.log('--------------');

if (true) {
    console.log(11)
}

console.log('' + 2) // 2
console.log(null + 2) // 2
// '' null 数字转换会变成0
console.log(undefined + 2) // NaN
// undefined 经过数字转换会变成NaN
console.log('转换', Number(undefined))

console.log(NaN + 2) // NaN
```

