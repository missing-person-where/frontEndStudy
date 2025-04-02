# WebAPIs-day01

## **变量声明**

* 建议：const优先，尽量用const，原因是：
  * const语义化更好
  * 当变量不会被改变了，用const很好
  * 实际开发中也是，比如react框架，基本用const
* 有了变量先用const，如果发现它后面是要被修改的，再改成let

存储**数组**和**对象**等引用类型的变量是地址，地址指向堆内存某片区域，如果用const修饰这个变量，可以修改指向的内容，但是不能修改这个变量它存储的地址，**建议数组和对象使用const来声明**

```javascript
const arr = ['green', 'red']
// arr.push('pink')
// console.log(arr)  // 不报错
// arr = [1, 2, 3]  // 报错
// console.log(arr) 
```

## WebAPIs基本认知

### 作用和分类

* 作用：就是使用JS去操作html和浏览器
* 分类：DOM(文档对象模型)、BOM(浏览器对象模型)

其实alert和prompt属于BOM

### 什么是DOM

* DOM (Document Object Model - **文档对象模型**) 是用来呈现以及与任意HTML 或 XML文档交互的API
* 白话：DOM是浏览器提供的一套专门用来**操作网页内容**的功能
* DOM的作用
  * 开发网页特效和实现网页交互，比如：随机点名

### DOM树

* 是什么
  * 将HTML文档以树状结构直观的表现出来，我们称之为文档树或DOM树
  * 描述网页内容关系的名词
  * 作用：**文档树直观的体现了标签与标签的关系**

**图例**

![image-20250330220632053](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250330220632053.png)

### DOM对象(重要)

* DOM对象：浏览器根据html标签生成的**JS对象** 

  ![image-20250330221351320](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250330221351320.png)

  * 所有的标签属性都可以在在这个对象上面找到
  * 修改这个对象的属性会自动映射到标签身上

* DOM的核心思想

  * 把网页内容当作**对象**处理

* document对象
  * 是DOM里提供的一个**对象**
  * 所以它提供的属性和方法都是**用来访问和操作网页内容的**
    * 例如：`document.write() `	
  * 网页所有内容都在document里面

```javascript
<div>123</div>
  <script>
    const div = document.querySelector('div')
    // 打印对象
    console.dir(div) // DOM对象
    
  </script>
```



## 获取DOM对象

#### 通过CSS选择器来获取元素

1. 选中匹配的**第一个**元素

   **语法：**

   ```javascript
   document.querySelector('css选择器')
   ```

   **参数：**

   包含一个或多个有效的CSS选择器 **字符串**

   **返回值：**

   CSS选择器匹配的第一个元素，一个HTMLElement对象。如果没有匹配到，返回null，**可以直接操作修改**

2. 选择匹配的**多个**元素

   **语法：**

   ```javascript
   document.querySelectorAll('css选择器')
   ```

   **参数：**

   包含一个或多个有效的CSS选择器 **字符串**

   **返回值：**

   CSS选择器匹配的**NodeList 对象集合**，**不可以直接操作修改**

   得到的是一个**伪数组**：

   * 有长度有索引号的数组
   * 但是没有pop() push() 等数组方法

   想要得到里面的每一个对象，需要用for遍历获得

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .box {
      width: 200px;
      height: 200px;
    }
  </style>
</head>
<body>
  <div class="box">123</div>
  <div class="box">abc</div>
  <p id="nav">导航栏</p>
  <ul>
    <li>测试1</li>
    <li>测试2</li>
    <li>测试3</li>
  </ul>
  <script>
    // 1. 获取匹配的第一个元素
    // const box = document.querySelector('div')
    // const box = document.querySelector('.box')
    // console.log(box)
    const nav = document.querySelector('#nav')
    // console.log(nav)
    nav.style.color = 'red'
    // 1. 获取第一个li
    // const li = document.querySelector('ul li:first-child')
    // console.log(li)
    // 2. 选择所有的li
    const lis = document.querySelectorAll('ul li')
    console.log(lis)
    
  </script>
</body>
</html>
```

**遍历伪数组**

```html
<ul class="nav">
    <li>我的首页</li>
    <li>产品介绍</li>
    <li>联系方式</li>
</ul>

<p id="nav">导航栏</p>

<script>
    const lis = document.querySelectorAll('.nav li')
    for (let i = 0; i < lis.length; ++i){
        console.log(lis[i])  // 每一个li对象
    }

    const p = document.querySelector('#nav')
    console.log(p)
    p.style.color = 'blue'
</script>
```



#### 其它方式获取DOM元素方法(了解)

| 方法                                 | 作用                                 |
| ------------------------------------ | ------------------------------------ |
| document.getElementById('id值')      | 根据id值获取一个元素                 |
| document.getElementsByTagName('div') | 根据 标签获取页面中的 所有div        |
| document.getElementByClassName('w')  | 根据类名获取 页面中所有类名为w的元素 |

### 操作元素内容

```html
<div class="box">我是文字的内容</div>
```

* 对象.innerText属性

  * 将文本内容添加/更新到任意标签位置

  * 表示纯文本，**不解析标签**

    

* 对象.innnerHTML属性

  * 将文本内容添加/更新到任意标签位置
  * **会解析标签**，**多标签**建议使用**模板字符串**

```javascript
// 1. 获取元素
const box = document.querySelector('.box')
// 2. 修改文字内容 对象.innerText 属性
// console.log(box.innerText)  // 获取文字内容

// // box.innerText = '我是一个盒子'  // 修改
// box.innerText = '<strong>我是一个盒子</strong>'  // 不解析标签

// 3. innerHTML 解析标签
console.log(box.innerHTML)
// box.innerHTML = '我要更换的内容'
box.innerHTML = '<strong>我要更换的内容</strong>'
```

### 操作元素属性

#### **操作元素常用属性**

* 可以通过js设置/修改标签元素属性，比如通过src切换图片

* 最常见的属性比如：href，title，src等

* 语法

  ```javascript
  对象.属性 = 值
  ```

  ```html
  <img src="./images/1.gif" alt="">
  <script>
      // 1. 获取图片元素
      const img = document.querySelector('img')
      // 2. 修改图片对象的属性  对象.属性 = 值
      img.src = './images/1_.jpg'
      img.title = 'vue创始人'
  </script>
  ```

#### 操作元素**样式**属性

* **通过style属性操作CSS**

  ```javascript
  对象.style.属性 = '值'
  // 生成的是行内样式表 权重较高
  ```

  ```html
  <div class="box"></div>
  <script>
      const box = document.querySelector('.box')
      // 对象.style.样式属性 = '值' 别忘了加单位
      box.style.width = '300px'
      // 多组单词的采取小驼峰命名法
      box.style.backgroundColor = 'hotpink'
      box.style.border = '2px solid blue'
      box.style.borderTop = '2px solid red'
  </script>
  ```

  **随机更换背景图案例**

  ```html
  <style>
      body {
          background: url(./images/1_.jpg) no-repeat top center/cover;
      }
  </style>
  
  <script>
      function getRandInt(N, M){
          return parseInt(Math.random() * (M - N + 1) + N)
      }
      const arr = ['1.jpg', '1_.jpg', '1.gif', '18.jpg']
      const body = document.body
      const random = getRandInt(0, arr.length)
      body.style.backgroundImage = `url(./images/${arr[random]})`
  
  </script>
  ```

  

* 通过类名（className）操作CSS

  * 如果修改的样式比较多，直接通过style修改比较繁琐，我们可以通过借助css类名的形式

  * 语法：

    ```javascript
    // active是一个类名
    元素.className = 'active'
    ```

  * 注意：

    > 1. 由于class是关键字，所以使用className去代替
    > 2. className是使用**新值换旧值**，如果需要添加一个类，需要保留之前的类名

  ```html
  <style>
      div {
          width: 200px;
          height: 200px;
          background-color: pink;
      }
      .nav {
          color: red;
      }
      .box {
          width: 300px;
          height: 300px;
          background-color: skyblue;
          margin: 100px auto;
          padding: 10px;
          border: 1px solid #000;
      }
  </style>
  
  <body>
    <div class="nav">123</div>
    <script>
      // 1. 获取元素
      const div = document.querySelector('div')
      // 2. 添加类名 class 是个关键字 所以用className
      console.log(div.className);
      // div.className = div.className + ' box'
      div.className = 'nav box'
    </script>
  </body>
  ```



* 通过classList操作类控制CSS

  * 为了解决className容易覆盖以前的类名，我们可以通过classList方式**追加**和**删除**类名

  * 语法

    ```javascript
    // 追加一个类
    元素.classList.add('类名')
    // 删除一个类
    元素.classList.remove('类名')
    // 切换一个类
    元素.classList.toggle('类名')
    ```



#### 操作**表单元素**属性

* 表单很多情况，也需要修改属性，比如可以看到密码，本质是把表单类型转换成文本框

* 正常的有属性有取值的 跟其他的标签属性没有任何区别

* 获取：DOM对象.属性值

* 设置：DOM对象.属性值 = 新值

  ```javascript
  表单.value = '用户名'
  表单.type = 'password'
  ```

* 表单属性中添加就有效果，移除就没有效果，一律用布尔值表示 如果为true就代表添加了该属性 如果是false代表移除了该属性

* 比如：disabled，checked，selected

```html
<body>
  <!-- <input type="text" value="电脑"> -->
  <input type="checkbox">
  <button>点击</button>
  <script>
    // 1 获取元素
    // const uname = document.querySelector('input')
    // 2. 获取值  获取表单里面的值 用的是 表单.value
    // console.log(uname.value) // 电脑
    // console.log(uname.innerHTML)  // innerHTML 得不到表单的输入内容
    // 3. 设置表单的值
    // uname.value = '我要买电脑'
    // console.log(uname.type)
    // uname.type = 'password'

    // 1. 获取
    const ipt = document.querySelector('input')
    // console.log(ipt.checked) // false   只接受boolean值
    ipt.checked = true
    // ipt.checked = 'true'  // 会选中，不提倡 有隐式转换

    // 1. 获取button
    const btn = document.querySelector('button')
    console.log(btn.disabled)  // 默认false
    btn.disabled = true // 禁用按钮
    
  </script>
</body>
```



#### 自定义属性

* **标准属性**：标签天生自带的属性 比如class，title等 比如：disabled，checked，selected
* **自定义属性**：
  * 在html5中推出了专门的data-自定义属性
  * 在标签一律以data-开头
  * 在DOM对象上一律用dataset对象方式获取

```html
<div data-id="1" data-spm="不知道">1</div>
  <div data-id="2">2</div>
  <div data-id="3">3</div>
  <div data-id="4">4</div>
  <div data-id="5">5</div>
  <script>
    const one = document.querySelector('div')
    console.log(one.dataset)
    console.log(one.dataset.id)   // 1
    console.log(one.dataset.spm)  // 不知道

  </script>
```





### 定时器-间歇函数

#### 介绍与应用场景

* 网页中经常需要一种功能：每隔一段时间需要自动执行一段代码，不需要我们手动触发
* 例如：网页中的倒计时
* 要实现这个需求，需要用到计时器函数
* 计时器函数有两种

定时器函数可以开启和关闭定时器

1. **开启定时器**

   ```javascript
   setInterval(函数, 间隔时间)
   ```

   * 作用：每隔一段时间调用这个函数
   * 注意：
     1. 函数名**不需要加括号**
     2. **定时器返回的是一个id数字**

2. **关闭定时器**

   ```javascript
   let 变量名 = setInterval(函数, 间隔时间)
   clearInterval(变量名)
   ```

   一般不会刚创建就停止，而是满足一定条件再停止

```javascript
// setInterval(函数, 间隔时间)
// setInterval(()=>{
//   console.log('一秒执行一次')
// }, 1000)

function fn() {
    console.log('一秒钟执行一次')
}
// setInterval(函数名, 间隔时间) 函数名不要加小括号
let n = setInterval(fn, 1000)
// setInterval('fn()', 1000)
console.log(n)
// 关闭定时器
// clearInterval(n)

// console.log('你好啊');

// let m = setInterval(function() {
//   console.log(12);
// }, 1000)
// console.log(m)

// const num = 10
// num = 10
// console.log(num)
```

综合案例

1. 用户协议倒计时
2. 轮播图计时版

