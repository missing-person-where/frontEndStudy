# JS-day01

## js介绍

### 是什么?

JavaScript是一门运行在客服端（浏览器）的编程（脚本）语言，可以实现人机交互

### 组成

包含ECMAScript(基本语法部分)和WebAPIs，WebAPIs又包含DoM(操作文档)和BoM(操作浏览器)

DoM: 页面元素移动，删除，设置大小等

BoM: 页面弹窗，检测窗口高度，存储数据到浏览器等

### 有什么用呢？

1. 网页特效（监听用户的一些行为，网页有相应的响应）
2. 表单验证（对表单数据的合法性进行验证）
3. 数据交互（获取后端的数据，并把数据渲染到前端）

### 体验js

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .pink {
      background-color: pink;
    }
  </style>
</head>
<body>
  <button class="pink">按钮1</button>
  <button>按钮2</button>
  <button>按钮3</button>
  <button>按钮4</button>

  <script>
    let btns = document.querySelectorAll('button');
    for (let i = 0; i < btns.length; ++i){
      btns[i].addEventListener('click', function(){
        document.querySelector('.pink').className = '';
        this.className = 'pink';
      });
    }
  </script>
</body>
</html>
```



## js书写位置

### 1.内部样式

```html
<!-- 内部js -->
  <script>
    // 页面弹出警示框
    alert("你好，js~ 努力奋斗");
  </script>
```

### 2.外部样式

```html
<script src="./js/my.js">
    // 中间不要写内容，写了也会被忽略
  </script>
```

```js
// ./js/my.js
alert("我是外部的js 文件  努力奋斗");
```

### 3.内联样式

```html
<button onclick="alert('努力奋斗')">按钮</button>
```

## js注释和结束符

### 1.单行注释

vscode快捷键：ctrl+/ 

> `// 这是一段单行注释`

### 2.多行注释

vscode快捷键：shift+alt+a

> `/*这是一段多行注释*/·`

### 3.结束符

结束符是分号==;== ，可以不加也可以加

## 字面量

数字3.14，666，字符串"abcdef"等都是字面量

## 输入和输出

### 1.文档输出内容

特点:**可以解析html内容**

```js
// 1.文档输出内容
    document.write("我是div标签")
    document.write('<h1>我是标题</h1>')
```

### 2.alert弹出警告框

```js
alert('警告，禁止F12调试')
```

### 3.控制台打印输出

```js
console.log('看看有没有内容')
console.log('日志');
```

### 4.输入语句

```js
prompt('请输入你的年龄：')
```

prompt特点：会弹出一个一个框，prompt里面的**传参**是**提示语句**，点击输入框里面可以输入内容，**返回值**以输入内容的**字符串**形式返回

## 变量

### let,var关键字

| 关键字 | 声明变量 | 初始化       | 赋值     |
| ------ | -------- | ------------ | -------- |
| let    | let age  | let age = 18 | age = 20 |
| var    | var age  | var age = 18 | age = 19 |

**声明变量未赋值，变量的默认值是undefined**

**一般都用let声明变量**，var是过去的产物，有很多缺陷，比如变量提升，例子：可以先赋值再使用后声明，并且还能多次声明同一个变量

```js
console.log(num)  // 变量未在前面声明就使用，但是不报错
var num = 10
num = 10
console.log(num)
```

```js
var num
var num = 10
var num = 20  // 变量多次声明不报错
console.log(num)
num = 10
console.log(num)
```

用let的原因是不会出现var那么多不合理的问题

### 变量的命名

1. 可以是**字母，数字，$和下划线**的组合
2. 严格**区分大小写**
3. 不能是关键字（如 let，var，if等）
4. 不能以数字开头（如 1abc）
5. 可以用小驼峰命名

### 变量的交换

```js
let num1 = 10
let num2 = 20
// 交换逻辑
// 方法1：只能用于数字
// num1 = num1 ^ num2
// num2 = num1 ^ num2
// num1 = num1 ^ num2
// 方法2：可以用于所有类型的数据
let tp = num1
num1 = num2
num2 = tp
console.log(num1, num2)    
```



### const定义常量

1.常量 不允许更改值

```js

const PI = 3.14
console.log(PI)
// PI = 3.14159625  // 报错
console.log(PI)
```

2. 常量声明的时候必须赋值

```js
// const PI 报错
```

## 数据类型

数据类型包含 基本数据类型 和 引用数据类型
基本数据类型 number string boolean undefined null
引用数据类型 (function, object, array)

### 数字型(number)

数字可以是小数也可以是大数，当遇到未定义的数学运算时，结果可能是（NaN， not a number）

NaN与任何其它的东西不相等，与任何操作数运算的结果都是NaN

数字运算的**运算符**

加 减 乘 除 （+ - * /）

正号 +    负号 -

取余 （%）

= 赋值运算符  例如：`age = 18`

+= 组合的赋值运算符  例如：`age += 1`

自增自减运算符 ++ --

`++a`:先运算再参与运算

`a--`:先参与运算再自减 

单独的`a++`或`++a`等于`a+=1`

比较运算符（结果是boolean类型的true或者false）

| 运算符 | 解释                 |
| ------ | -------------------- |
| >      | 大于                 |
| <      | 小于                 |
| >=     | 大于等于             |
| <=     | 小于等于             |
| ==     | 值相等，类型可以不同 |
| ===    | 值相等，而且类型相同 |
| !=     | 值不等               |
| !==    | 不全等（值和类型）   |

```js

    // 运算符 + - * / % ()
    // js 弱数据类型的语言
    // let num = 'pink'
    // let num = 11.1
    // console.log(num)
    console.log(1 + 1)
    console.log(1 - 1)
    console.log(1 * 1)
    console.log(1 / 1)
    console.log(4 % 3)
    // java 强数据类型的语言 int num = 10;

    // NaN  not a number 不正确的或者未定义的数学操作

    console.log('周晓七' - 2)
    // NaN是有粘性的
    console.log(NaN + 2)
    console.log(NaN - 2)
    console.log(NaN * 2)
    console.log(NaN / 2)
    console.log(NaN % 2)
    console.log(NaN = NaN)
    console.log(NaN == NaN)
    console.log(NaN === NaN)
```

### 字符串(string)

有三种定义字符串的方式

| 分类               | 示例           |
| ------------------ | -------------- |
| 单引号             | 'hello world'  |
| 双引号             | "hello world"  |
| 反引号(模板字符串) | \`helloworld\` |

使用示例

```js
// 单引号('')和双引号("")都可以 推荐单引号
// let str = 'pink'
// let str1 = "pink"
// let str2 = `中文`
// console.log(str2)
// console.log(11)
// console.log(`11`);
// console.log('str');
// let strt = ''  // 这种情况叫空字符串 默认情况
// console.log(strt);
// console.log('他一直都很"积极"');
// console.log("他一直都很'积极'");
// console.log('他一直都很\'积极\'')
```

+号运算符可以拼接字符串

```js
// 字符串拼接
console.log(1 + 1);
console.log('你好' + 'javascript')
```

```js
let age = 24

// document.write('我今年'+19)
// document.write('我今年'+age)
// document.write('我今年'+age+'岁了') // 含隐式转换 age为'24'然后拼接字符串
document.write('我今年'+ age +'岁了')
```

模板字符串 

${变量名} 的使用 ``内的这个东西会被解析成变量的值

```js
// 模板字符串 反引号包含 `` 里面 ${变量名}
// let age = 18
// document.write(`我今年${age}岁了`)

// 案例
let uname = prompt('输入姓名')
let age = prompt('输入年龄')
document.write(`大家好，我叫${uname},今年${age}岁了`)
```

### 其它类型

| 类型      | 解释                                         |
| --------- | -------------------------------------------- |
| boolean   | 布尔类型，只有true和false                    |
| undefined | 未定义，变量声明了但是没赋值的默认值         |
| null      | 当知道要传的是对象的时候，可以设置的默认参数 |

```js
// 1. true false 是布尔类型的字面量
// console.log(3 > 4)

// let isCool = true
// console.log(isCool)

// 2. undefined 表示没有赋值 未定义类型 弱数据类型
// 声明一个变量未赋值就是 undefined
// let va
// console.log(va)

// 3. null（空类型）存对象
// 表示赋值了但是内容为空
// let obj = null
// console.log(obj)
// 计算区别
console.log(undefined + 1)  // NaN
console.log(null + 1)  // 1
```



### typeof检测数据类型

返回的是number, string, boolean等类型。注意是字符串

| 用法          | 示例          | 结果      |
| ------------- | ------------- | --------- |
| typeof {data} | typeof "abc"  | "string"  |
| typeof(data)  | typeof(false) | "boolean" |

 ```js
 let num = 10
 console.log(typeof num) // number
 let str = 'pink'
 console.log(typeof str) // string
 let flag = false
 console.log(typeof flag) // boolean
 let un
 console.log(typeof un)  // undefined
 let obj = null
 console.log(typeof obj) // object
 console.log(typeof(typeof obj)) // string
 
 let num1 = prompt('请输入第一个数：')
 
 console.log(typeof num1)
 
 ```

### 数据类型转换

#### 隐式转换

1. +号左右两边只要有一个操作数是字符串，返回的就是字符串
2. 其它的- * /等都会尝试把两边的操作数隐式转换成数字再进行运算

```js
// 规则一 +号左右两边有一个字符串另外一个会被隐式转换成字符串
console.log(1 + 1)
console.log('pink' + 1) // 隐式转换1为'1'
console.log(2 + 2)
console.log(2 + '2') // 22
// 规则二 出来+号比如 - * /等都会把数据转成数字类型
console.log(2 - 2)
console.log(2 - '2')
console.log(2 * '2')
console.log(2 / '2')
console.log(2 % '2')
```



#### 技巧

+作为正号可以将字符串转换成数字

+'123' ==> 123

```js
console.log(+12)
// 技巧：
// +号作为正号解析可以转换成数字型
// 任何数据和字符串相加结果都是字符串
console.log(+'123')  // 转换为数字型
    
```

#### 显示转换

Number(str)可以将str转换成数字

parseInt(str)可以取str的前几位构成的整数

parseFloat(str)可以取str的前几位构成的小数

```js
// Number(数据) 转换成数字
let str = '123'
console.log(Number(str))
console.log(Number('pink'))
// let num = Number(prompt('输入年薪'))
// let num = +prompt('输入年薪')
// console.log(Number(num))
// console.log(num)
// parseInt 取前面的整数部分
console.log(parseInt('12px'))
console.log(parseInt('12.34px'))
console.log(parseInt('12.94px'))
console.log(parseInt('abc12.94px'))

// ---------------
// parseFloat 去前面的小数部分
console.log(parseFloat('12px'))
console.log(parseFloat('12.34px'))
console.log(parseFloat('12.94px'))
console.log(parseFloat('abc12.94px'))
```

一个demo：

```js
let num1 = +prompt('输入第一个数字')
// let num2 = Number(prompt('输入第二个数字'))
let num2 = parseInt(prompt('输入第二个数字'))
// let num2 = parseFloat(prompt('输入第二个数字'))
alert(`${num1} + ${num2} = ${num1 + num2}`)
```

