# WebAPIS-day06

## 正则表达式

### 介绍

**什么是正则表达式**

* 正则表达式是用来**匹配字符串中字符组合的模式**。在JavaScritp中，正则表达式也是对象
* 通常用来**查找**，**替换**那些符合正则表达式的文本，很多语言都支持正则表达式
* **使用场景：**
* 例如**验证表单：**用户名表单只能输入英文字母，数字或者下划线，昵称输入框中可以输入中文（**匹配**）
  * 比如用户名：`/^[a-z0-9_-][3,16]$/`
* **过滤**掉页面内容的**敏感词**（**替换**），或重字符串中**提取**我们想要的**特定部分**（**提取**）等

### 语法

1. **定义正则表达式语法：**

* ```javascript
  const 变量名 = /表达式/
  ```

2. **判断是否有符合规则的字符串：**

   **test()** 方法，用来查看正则表达式与指定字符串是否匹配

* **语法：** 

  ```javascript
  regObj.test(被检测的字符串)
  ```

* **返回值：**能匹配上返回true，不能匹配上返回false

    ```javascript
    const str = '我在学习前端，希望对网页有更深刻的理解'
    // 正则表达式使用：
    // 1. 定义规则
    const reg = /前端/
    // 2. 是否匹配
    console.log(reg.test(str))  // true	
    ```

3. **检索（查找）符合规则的字符串**

   **exec()** 方法在一个指定字符串中执行一个搜索匹配

* **语法**

    ```javascript
    regObj.exec(被检测的字符串)
    ```

* **例子：**

  ```javascript
  const str = '我在学习前端，希望对网页有更深刻的理解，并且能够用学习的前端知识构建一个美丽的网页'
  // 正则表达式使用：
  // 1. 定义规则
  const reg = /前端/
  // 2. 是否匹配
  // console.log(reg.test(str))  // true
  // 3. exec()
  console.log(reg.exec(str))

* **输出：**['前端', index: 4, input: '我在学习前端，希望对网页有更深刻的理解，并且能够用学习的前端知识构建一个美丽的网页', groups: undefined]

* **返回值：**匹配成功，返回所有匹配上的值（数组形式），没匹配上返回null

### 元字符

* **普通字符：**

  大多数的字符仅能够描述它们本身，这些字符称作普通字符，例如所有的字母和数字。
  也就是说普通字符只能够匹配字符串中与它们相同的字符。

* **元字符（特殊字符）：**

  是一些具有特殊含义的字符，可以极大提高灵活性和强大的匹配功能

  * 比如，规定用户只能输入26个英文字母，用普通字符abcdefghijk.....
  * 换成元字符：[a-z]

* 参考文档

  >MDN:  https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions
  >正则测试工具: http://tool.oschina.net/regex

**元字符分类：**

* **边界符（表示位置，开头和结尾，必须用什么开头，用什么结尾）**
* **量词**（用来表示**重复**次数）
* **字符类**（比如 \d 表示0-9）

**1.边界符**

* **提示字符所处位置**

  | 边界符 | 说明                           |
  | ------ | ------------------------------ |
  | ^      | 表示匹配行首的文本（以谁开始） |
  | $      | 表示匹配行尾的文本（以谁结束） |

* ^和$一起用，表示精确匹配

```javascript
// 元字符
console.log(/哈/.test('哈'))  // true
console.log(/哈/.test('哈哈哈'))  // true
console.log(/哈/.test('二哈'))  // true
// 1. 边界符
console.log(/^哈/.test('哈'))  // true
console.log(/^哈/.test('哈哈'))  // true
console.log(/^哈/.test('二哈'))  // false
console.log(/^哈$/.test('哈'))  // true
// 精确匹配 只能是 哈 才为true
console.log(/^哈$/.test('哈哈'))  // false
console.log(/^哈$/.test('哈不哈'))  // false
console.log(/^哈$/.test('二哈'))  // false

console.log('-------------')
```

**2.量词**

| 量词   | 说明            |
| ------ | --------------- |
| *      | 重复0次或多次   |
| +      | 重复1次或多次   |
| ?      | 重复0次或一次   |
| {n}    | 重复n次         |
| {n, }  | 重复n次或更多次 |
| {n, m} | 重复n到m次      |

**注意：逗号两侧不能出现空格**

```javascript
// // 量词 * 类似 >=0 次
// console.log('-------------')
// console.log(/^哈$/.test('哈'))  // true
// console.log(/^哈*$/.test(''))  // true
// console.log(/^哈*$/.test('哈'))  // true
// console.log(/^哈*$/.test('哈哈'))  // true
// console.log(/^哈*$/.test('二哈太傻啦'))  // false
// console.log(/^哈*$/.test('哈不哈'))  // false
// console.log('----------------')

// // 量词 + 类似 >=1 次
// console.log('-------------')
// console.log(/^哈+$/.test('哈'))  // true
// console.log(/^哈+$/.test(''))  // false
// console.log(/^哈+$/.test('哈'))  // true
// console.log(/^哈+$/.test('哈哈'))  // true
// console.log(/^哈+$/.test('二哈太傻啦'))  // false
// console.log(/^哈+$/.test('哈不哈'))  // false
// console.log('----------------')

// // 量词 ？ 类似 0 || 1 次
// console.log(/^哈?$/.test(''))  // true
// console.log(/^哈?$/.test('哈'))  // true
// console.log(/^哈?$/.test('哈哈'))  // false
// console.log(/^哈?$/.test('二哈太傻啦'))  // false
// console.log(/^哈?$/.test('哈不哈'))  // false

// 量词 {n} 写几就出现几次
console.log('----------------')
console.log(/^哈{4}$/.test(''))
console.log(/^哈{4}$/.test('哈'))
console.log(/^哈{4}$/.test('哈哈'))
console.log(/^哈{4}$/.test('哈哈哈'))
console.log(/^哈{4}$/.test('哈哈哈哈'))  // true
console.log(/^哈{4}$/.test('哈哈哈哈哈'))
console.log(/^哈{4}$/.test('哈哈哈哈哈哈'))
console.log('----------------')

// 量词 {n, } >= n 次
console.log('----------------')
console.log(/^哈{4,}$/.test(''))
console.log(/^哈{4,}$/.test('哈'))
console.log(/^哈{4,}$/.test('哈哈'))
console.log(/^哈{4,}$/.test('哈哈哈'))
console.log(/^哈{4,}$/.test('哈哈哈哈'))  // true
console.log(/^哈{4,}$/.test('哈哈哈哈哈')) // true
console.log(/^哈{4,}$/.test('哈哈哈哈哈哈')) // true
console.log('----------------')

// 量词 {n,m} [n,m] 次 逗号左右两侧不能有空格
console.log('----------------')
console.log(/^哈{4,5}$/.test(''))
console.log(/^哈{4,5}$/.test('哈'))
console.log(/^哈{4,5}$/.test('哈哈'))
console.log(/^哈{4,5}$/.test('哈哈哈'))
console.log(/^哈{4,5}$/.test('哈哈哈哈'))  // true
console.log(/^哈{4,5}$/.test('哈哈哈哈哈')) // true
// 易错：逗号左右两侧不能有空格 否则视为普通字符
console.log(/^哈{4, 5}$/.test('哈哈哈哈哈')) // true
console.log(/^哈{4,5}$/.test('哈哈哈哈哈哈')) 
console.log('----------------')

```

**3.字符类**

  [] 匹配字符集合

* 后面的字符串只要包含任意一个字符，返回**true**

* 比如 `\[abc]\`只会匹配a，b，c中任意**一个**字符

* 使用连字符 - 表示一个范围

  ```javascript
  console.log(/^[a-z]$/.test('c')) // true
  ```

* 比如：

* [a-z] 表示 **a到z** 26个英文字母都可以

* [a-zA-Z] 表示大小写字母都可以

* [0-9] 表示0到9的数字都可以

* 腾讯qq号正则匹配

  ```javascript
  腾讯qq号：/^[1-9][0-9]{4,}$/ (腾讯qq号从10000开始)
  ```

* [] 里面加上 ^ 取反

* 比如：

* [^a-z]匹配除了小写字母为以外的字符

* 注意写道括号内

* .(点) 匹配**除了换行符**之外的任何单个字符

```javascript
// 字符类 [a-z] 选择a到z其中一个
console.log(/^[a-z]$/.test('p'))  // true
console.log(/^[A-Z]$/.test('p'))  // false
console.log(/^[0-9]$/.test('6'))  // true
console.log(/^[a-zA-Z0-9]$/.test('3'))  // true
console.log(/^[a-zA-Z0-9]$/.test('p'))  // true
console.log(/^[a-zA-Z0-9]$/.test('P'))  // true
console.log(/^[^a-zA-Z0-9]$/.test('P'))  // false
console.log('----------------')

console.log(/^.$/.test(''))  // false
console.log(/^.$/.test('\n'))  // false
console.log(/^.$/.test('a'))
console.log(/^.$/.test('0'))
console.log(/^.$/.test('A'))
console.log(/^.$/.test(' '))
```

**用户名验证案例**

用户名要求，英文字母，数字，下划线或者短横线组成，用户名长度为6到16

**6和16之间不能有空格**

`/^[a-zA-Z0-9-_]{6,16}$/`

* 预定义：指的是 **某些常见模式的简写方式。**

  | 预定类 | 说明                                                         |
  | ------ | ------------------------------------------------------------ |
  | \d     | 匹配0-9之间的任一数字，相当于`[0-9]`                         |
  | \D     | 匹配所有0-9以外的字符，相当于`[^0-9]`                        |
  | \w     | 匹配任意的字母，数字和下划线，相当于`[A-Za-z0-9_]`           |
  | \W     | 除所有字母，数字和下划线以外的字符，相当于 `[^A-Za-z-0-9_]`  |
  | \s     | 匹配空格（包括换行符，制表符，空格符等），相当于 `[\t\r\n\v\f]` |
  | \S     | 匹配非空格的字符，相当于`[^\t\n\r\v\f]`                      |

  日期格式: `/^\d{4}-\d{1, 2}-\d{1,2}/`

### 修饰符

* 修饰符约束正则执行的某些细节行为，如是否区分大小写，是否支持多行匹配

* 语法：

  ```javascript
  /表达式/修饰符
  ```

* **i**是ignore的缩写，正则匹配时字母**不区分大小写**

* **g**是单词global的缩写，**匹配所有满足正则表达式的结果**

  ```javascript
  console.log(/a/i.test('a')) // true
  console.log(/a/i.test('A')) // true
  ```

* **replace替换**

* **语法：**

  ```javascript
  字符串.replace(/正则表达式/, '替换的文本')
  ```

* **返回值：**

  返回被替换后的字符串，原来要被替换的字符串变量是不会被改变的 

  ```javascript
  console.log(/^java$/.test('java'))  // true
  console.log(/^java$/.test('JAVA'))  // false
  // i（ignore）不区分大小写
  console.log(/^java$/i.test('JAVA'))  // true
  console.log(/^java$/i.test('Java'))  // true
  
  const str = 'java是一门编程语言，学完JAVA工资很高'
  // 只能查找并替换一个
  const re = str.replace(/java/i, '前端')
  console.log(str)
  console.log(re)
  // const re2 = str.replace(/java/ig, '前端')
  // const re2 = str.replace(/java/gi, '前端')
  const re2 = str.replace(/java|JAVA/g, '前端')
  console.log(str)
  console.log(re2)
  ```

**过滤敏感词**

要求用户输入不能有敏感词

比如，老师上课很有**

1. 用户输入
2. 对敏感内容进行正则查找替换为**
3. 需要全局替换g

## 综合案例

**小兔鲜页面注册**

1.发送验证码模块

```javascript
// 1. 发送短信验证码
const code = document.querySelector('.code')
let flag = true // 通过变量控制 - 节流阀
// 1.1 点击事件
code.addEventListener('click', function() {
    if (flag) {
        // 取反了，不允许第二次点击
        flag = false
        let i = 5
        // 点击之后立马触发
        code.innerHTML = `0${i}秒后重新获取`
        let timerId = setInterval(function(){
            i--
            code.innerHTML = `0${i}秒后重新获取`
            if (i == 0) {
                clearInterval(timerId)
                // 重新获取
                code.innerHTML = '重新获取'
                // 到时间了，下一次可以点击了
                flag = true
            }
        }, 1000)
        }
})
```



2.各个表单验证模块

**change事件**：表单中的值改变会触发的事件

```html
<input type="text">
  <script>
    // change是内容发生了变化
    const input = document.querySelector('input')
    input.addEventListener('change', function() {
      console.log(11)
    })
  </script>
```



3.勾选已经阅读同意模块

4.下一步验证全部模块

​	只要有一个input验证不通过就不同意提交

   classList.contains(类名)可以用来检测有没有这个类

## 阶段案例

**小兔鲜登录页面**

1.tab栏切换，事件委托，自定义属性记录每一个导航对应的盒子

2.点击登录可以跳转页面

```html
<form action="" autocomplete="off"></form>
autocomplete="off"表示不记录之前的输入，更安全
```

* 先阻止默认行为

* 如果没有勾选同意协议，则提示要勾选

* required属性不能为空

* 假设登录成功

  把用户名记录到本地存储

  同时跳转到首页

**小兔鲜首页页面**

步骤：最好写一个**渲染函数**

1.如果本地有用户名，读取本地数据，把用户名写入到里面取

​	需要把用户名写到第一个li里面
​	格式: `<a href="javascript:;"><i class="iconfont icon-user"> pink老师</i></a>`
​	因为登录了，所以第二个里面的文字变为，退出登录
​	格式：`<a href="javascript:;">退出登录</a>`

2.没有数据，还原为默认结构
