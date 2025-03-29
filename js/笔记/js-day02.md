# js-day02

## 运算符

### 赋值运算符

```javascript
let num = 1
num = num + 1
console.log(num)
num += 3
console.log(num)
```

### 一元运算符（++， --）

```javascript
let num = 1
num += 1
console.log(num)
// 先自增再赋值
let m = ++num
console.log(num + ' m: ' + m)
// 先赋值再自增
m = num++
console.log(num + ' m : ' + m)
// 先自减再赋值
m = --num
console.log(num + ' m : ' + m)
// 先自减再赋值
m = num--
console.log(num + ' m : ' + m)

let i = 1
// 加了 输出 再自增
console.log(i++ + 1) // 2
// 自增 加了 再输出
console.log(++i + 1) // 4

i = 1
console.log(i++ + ++i + i) // 7
```

### 比较运算符

```javascript
console.log(3 > 5)
console.log(3 >= 3)
console.log(2 == 2)
// == 左右值是否相等
// 有隐式转换 把 '2' 转换成 2
console.log(2 == '2')
// === 左右两边类型和值都相等 一般用这个判断是否相等
console.log(2 === '2')
console.log(NaN == NaN) // NaN与所有东西都不相等 包含她自己

// !== 左右两边不全等
console.log(2 != '2')  // false 对应 ==
console.log(2 !== '2') // true  对应 ===

// 字符串比较按照ASSCII
console.log('-----------')
console.log('a' < 'b')
console.log('aa' < 'ab')
console.log('aa' < 'aac')
console.log('----------')

// 浮点数有精度问题，一般不直接比较浮点数
console.log(0.1 + 0.2)
console.log((0.1*10 + 0.2*10) / 10)
```



### 逻辑运算符

与 或 非 && || ! 

```javascript
// && 与  一假就假
console.log(true && false)
console.log(false && true)
console.log(3 < 5 && 3 > 2)
console.log(3 < 5 && 3 < 2)
console.log('------------------')

// || 或  一真就真
console.log(true || true)
console.log(true || false)
console.log(false || true)
console.log('------------------')
// !  逻辑非  取反
console.log(!true)
console.log(!false)
console.log(!1)

console.log('------------------')
let num = 2
console.log(num > 5 && num < 10)
console.log('------------------')
```

### 三元运算符

 condition ? condition为true执行的代码:condition为false执行的代码

```javascript
 // condition ? condition为true执行的代码:condition为false执行的代码
console.log( 3 > 5 ? 3 : 5)
// if (3 > 5){
//   alert('真的')
// } else {
//   alert('假的')
// }
// alert(3 > 5 ? '真的' : '假的')

let sum = 3 < 5 ? 3 : 5
console.log(sum)
```

### 求最大值

```javascript
let num1 = +prompt('输入第一个数')
let num2 = +prompt('输入第二个数')
alert(num1 > num2 ? num1 : num2)
let num3 = +prompt('输入第三个数')
// alert((num1 > num2 ? num1 : num2) > num3 ? (num1 > num2 ? num1 : num2): num3)
let a
alert(a = num1 > num2 ? num1 : num2, a > num3 ? a: num3)					
```



### 运算符优先级

![image-20250329140123512](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250329140123512.png)

## 流程控制语句

### if分支语句

#### 单分支

**if**

```javascript
if (false){
    console.log('看我在不在1')
}
if (3 > 5){
    console.log('看我在不在2')
}
if (2 == '2'){
    console.log('看我在不在3')
}
if (2){
    console.log('看我在不在4')
}
if (0){
    console.log('看我在不在5')
}
if ('你好啊'){
    console.log('看我在不在6')
}
if (''){
    console.log('看我在不在7')
}

// 案例
let score = +prompt('输入成绩')
if (score >= 700){
    alert('恭喜考入北京大学')
}
console.log('-----------')
```

#### 双分支

**if else**

```javascript
let uname = prompt('输入用户名')
let pwd = prompt('输入密码')
// 注意 ===
if (uname === 'admin' && pwd === '1234'){
    alert('登录成功')
}else{
    alert('用户名或密码错误')
}
```

#### 多分支

**if else if ····· else**

```javascript
let score = +prompt('输入成绩')
if (score > 90){
    alert('优秀')
}else if (score >= 70){
    alert('良好')
}else if (score >= 60){
    alert('及格')
}else{
    alert('不及格')
}
```

### switch语句

不break和c语言一样有case穿透，相比与if，else，switch的效率较高，运用了二叉搜索树的优良性质，可以在log n 的复杂度找到目标值

```javascript
// switch (val){
//   case val1:
//     code1
//     break
//   case val1:
//     code1
//     break
//   case val1:
//     code1
//     break
//   default:
//     coden
// }
switch (2){
    case 1:
        console.log('1');
        // break
    case 2:
        console.log('2');
        // break
    case 3:
        console.log('3');
        // break
    case 4:
        console.log('4');
        // break
    default:
        console.log('other')
}
```

### while循环

```javascript
// while (condition){
//   要重复执行的代码(循环体)
// }
// let i = 1
// while (i <= 3){
//   document.write('我会循环三次<br>')
//   i++
// }
let end = +prompt('输入次数')
let i = 1
while (i <= end){
    document.write('我会循环三次<br>')
    i++
}
// do while 同c语言 基本不用
```

### do while循环

**略** 用的少

### for 循环

```javascript
for (let i = 0; i < 3; ++i){
    document.write(`学习js第二天<br>`)
}
```

continue：执行到continue后，结束本次循环，进行下一次循环，可以在**循环**中使用

break：执行到break后，结束循环，可以在**循环**和**switch**选择语句执行

### 循环嵌套

**循环里面还有其它循环**就叫循环嵌套

一些demo：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    span {
      display: inline-block;
      width: 100px;
      text-align: center;
      padding: 5px 10px;
      border: 1px solid pink;
      margin: 2px;
      border-radius: 5px;
      box-shadow: 2px 2px 2px rgba(255, 192, 203, .4);
      background-color: rgba(255, 192, 203, .1);
      color: pink;
    }
  </style>
</head>
<body>
  <script>
    // 1 打印小星星
    /* let row = +prompt('输入行数')
    let col = +prompt('输入列数')
    for (let i = 1; i <= row; ++i){
      for (let j = 1; j <= col; ++j){
        document.write(`⭐`)
      }
      document.writeln('<br>')
    }
    document.writeln('----------------<br>')*/
    // 2 打印三角形星星
    for (let i = 1; i <= 5; ++i){
      for (let j = 1; j <= i; ++j){
        document.write(`⭐`)
      }
      document.writeln('<br>')
    }
    document.writeln('----------------<br>')
    // 3. 九九乘法表
    for (let i = 1; i <= 9; ++i){
      for (let j = 1; j <= i; ++j){
          document.writeln(`<span>${j} × ${i} = ${j * i}</span>`)
      }
      document.writeln('<br>')
    }
  </script>
</body>
</html>
```

### 数组

#### 数组的初始化以及访问

可以通过字面量或者构造函数初始化数组

可以通过下标访问数组中的元素

数组名.length可以获取数组长度

```javascript
// 1. 字面量声明数组
// let arr = [1, 'blue', 2, true]
// 2. 使用 new Array() 构造函数声明
// let arr = new Array(1, 2, 3, 4)
// console.log(arr)

// 数组求和
let arr = [2, 6, 1, 7, 4]
let s = 0
for (let i = 0; i < arr.length; ++i){
    s += arr[i]
}
console.log(`总数：${s}`);

console.log(`平均数: ${s / arr.length}`);
```

#### 数组修改元素

直接通过下标访问元素并赋值

```javascript
let arr = [0, 1, 2, 3]
// 查询
console.log(arr[0])
// 添加元素
console.log(arr[arr.length] = 4)
console.log(arr[arr.length] = 5)
console.log(arr)
// 修改元素
arr[0] = 99
console.log(arr)
```

#### 数组添加元素

假设数组名是arr

在尾部追加元素：

1. arr[arr.length] = 插入的新元素
2. arr.push(element) ==> 返回数组新长度

在头部添加元素：

1. arr.unshift(element) ==> 返回数组新长度

```javascript
let arr = []
// 插入方式1：不常用
console.log(arr[0])
console.log(arr[1])
arr[1] = 2
console.log(arr)
console.log(arr.length)
console.log(arr[0] = 1)
console.log(arr)

// push() 在尾部添加内容 返回添加后数组长度
console.log(arr.push(999))

console.log(arr)

// unshift() 在首部添加内容 返回添加后数组的长度
console.log(arr.unshift(9999))
console.log(arr)
```

#### 数组删除元素

| API            | 作用                           | 返回值                  |
| -------------- | ------------------------------ | ----------------------- |
| pop()          | 删除末尾元素                   | 删除的末尾元素          |
| shift()        | 删除头部元素                   | 删除的头部元素          |
| splice(idx, n) | 从**idx**开始，删除**n**个元素 | 删除的n个元素构成的数组 |

```javascript
let arr = [1, 9, 10, 3, 2]
// pop() 删除数组末尾数据 并返回删除的数据
console.log(arr.pop())
console.log(arr)

// shift() 删除数组开头元素 并返回删除数据
console.log(arr.shift())
console.log(arr)

// splice(idx, num)
// 从idx开始，删除num个元素
// num默认为数组长度
// arr.splice(1)
console.log(arr.splice(1, 1))
console.log(arr)

// slice(start, end) 切片 和 python中的[a: b] 类似 左闭右开
```

#### **柱状图案例**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .box {
      display: flex;
      justify-content: space-around;
      align-items: flex-end;
      margin: 10% auto;
      width: 600px;
      height: 300px;
      /* background-color: skyblue; */
      border-left: 1px solid #000;
      border-bottom: 1px solid #000;
    }

    .box>div {
      display: flex;
      width: 50px;
      background-color: yellowgreen;
      flex-direction: column;
      justify-content: space-between;
    }

    .box div span {
      margin-top: -20px;
    }

    .box div h4 {
      margin-bottom: -35px;
      width: 80px;
      margin-left: -5px;
    }
  </style>
</head>
<body>

  <script>
    let arr = []
    for (let i = 0; i < 4; ++i){
      let income = prompt(`请输入第${i + 1}季度的收益`)
      arr.push(income)
    }
    // 渲染内容
    document.write(`<div class="box">`)
    for (let i = 0; i < arr.length; ++i){
      document.write(`
      <div style="height: ${arr[i]}px;">
        <span>${arr[i]}</span>
        <h4>第${i + 1}季度</h4>
      </div>
    `)
    }
    document.write(`</div>`)
  </script>
</body>
</html>
```

#### **冒泡排序案例以及sort**

```javascript
// let arr = [1, 90, 20, 34, 100]
// 冒泡排序
// let swap
// for (let i = 0; i < arr.length - 1; ++i){
//   swap = false
//   for (let j = 0; j < arr.length - i - 1; ++j){
//     if (arr[j] > arr[j + 1]){
//       swap =  true
//       let temp = arr[j]
//       arr[j] = arr[j + 1]
//       arr[j + 1] = temp
//     }
//   }
//   if (!swap){
//     break
//   }
// }
// console.log(arr)

// 调用数组排序方法
let arr = [90, 100, 1, 300, 234]
console.log(arr)
// 不知道这个sort是不是默认升序
arr.sort()
console.log('-----------')
for (let i = 0; i < arr.length; ++i){
    console.log(arr[i])
} 
// 1 100 234 300 90
// 默认排序行为是将数组元素转换成字符串，再根据字符串的Unicode码值进行比较排序
console.log('-----------')
// 升序
// arr.sort(function(a, b){
//   return a - b
// })
arr.sort((a, b) => a - b)
console.log(arr)
// 降序
// arr.sort(function(a, b){
//   return b - a
// })
// 简写
// arr.sort((a, b) => b - a)
// console.log(arr)
```

### 函数

将一些功能封装到函数内，当有相应的功能需求时，就调用这个函数，提高了代码的**灵活性**以及**复用性**

#### 函数的使用（无参）

````javascript
let num = 10
console.log(num)
// 1. 函数的声明
function sayHi(){
    console.log('hi~~~~')
}
// 函数调用
sayHi()
sayHi()
sayHi()
````

