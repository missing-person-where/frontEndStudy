# `webAPIs-day02`-事件

## 事件监听

目标：可以给DOM元素添加事件监听

* 什么是事件？
  事件是在编程时系统内发生的动作或者发生的事情
  比如用户在网页上单击一个按钮
* **什么是事件监听**？
  就是让程序检测是否有事件产生，一旦有事件触发，就立即调用一个函数做出响应，也称为绑定事件或者注册事件
  比如鼠标经过显示下拉菜单，比如点击可以播放轮播图等等

* 语法：

  ```javascript
  元素对象.addEventListener('事件类型', 要执行的函数)
  ```

* 事件监听三要素：

  * 事件源：那个DOM元素被事件触发了，要获取DOM元素
  * 事件类型：用什么方式触发，比如鼠标单击`click`、鼠标经过`mouseover` 等
  * 事件调用的函数：要做什么事

```html
<button>点击</button>
<script>
    // 需求：点击了按钮，弹出一个对话框
    // 1. 事件源  按钮
    const btn = document.querySelector('button')
    // 2. 事件类型 点击
    // 3. 事件处理程序 弹出对话框
    btn.addEventListener('click', function() {
        alert('按钮被点击了')
    })
</script>
```

#### 事件监听版本

* DOM L0

  事件源.on事件 = function() {}

* DOM L2

  事件源.addEventListener(事件， 事件处理函数)

* 区别：

  on方式会被覆盖，addEventListener方式可以绑定多次，，拥有事件更多特性，推荐使用

```html
<button>点击</button>
  <script>
    const btn = document.querySelector('button')
    // btn.onclick = function() {
    //   alert('按钮被点击11')
    // }
    // btn.onclick = function() {
    //   alert('按钮被点击22')
    // }

    // let num = 10
    // num = 20

    btn.addEventListener('click', ()=>alert(11))
    btn.addEventListener('click', ()=>alert(22))
  </script>
```



#### 事件类型

![image-20250401131645966](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250401131645966.png)

**鼠标事件**

```html
<div>23324324</div>
  <script>
    const div = document.querySelector('div')
    // 鼠标经过
    div.addEventListener('mouseenter', function() {
      console.log('轻轻的我来了')
    })
    // 鼠标离开
    div.addEventListener('mouseleave', function() {
      console.log('轻轻的我走了')
    })
  </script>
```

**焦点事件和文本输入**

```html
<input type="text">
  <script>
    const input = document.querySelector("input")
    // 1. 键盘事件
    // input.addEventListener('keydown', function() {
    //   console.log("键盘按下了")
    // })

    // input.addEventListener('keyup', function() {
    //   console.log("键盘弹起了")
    // })
    // 2. 用户输入文本事件  input
    input.addEventListener('input', function() {
      console.log(input.value)
    })
  </script>
```

### 事件对象

#### 获取事件对象

* 是什么？

  * 也是一个对象，这个对象中有事件触发时的相关信息

* 使用场景

  * 可以判断鼠标点击了哪个元素，比如按下enter可以发布新闻
  * 可以判断鼠标点击了哪个元素，从而执行相应的操作

* 语法：如何获取

  * 在事件绑定的回调函数的第一个参数就是事件对象

  * 一般命名为event，ev，e

    ```javascript
    元素.addEventListener('click', function(e) {
        
    })
    ```

* 部分常用属性

  * type：获取事件类型
  * clientX/clientY：点击的坐标位置
  * offsetX/offsetY：获取光标相对于当前DOM元素左上角的位置
  * key：用户按下的键盘键的值，现在不用keyCode了

```html
<!-- <button>点击</button> -->
  <input type="text">
  <script>
    // const btn = document.querySelector('button')
    // btn.addEventListener('click', (e) => {
    //   console.log(e)
    // })

    const input = document.querySelector('input')
    input.addEventListener('keyup', function(e) {
      // console.log(111111)
      // console.log(e)
      // console.log(e.key)
      // console.log(e.type)
      if (e.key === 'Enter') {
        console.log("按下了回车")
      }
    })
  </script>
```

### 环境对象

目标：能够分析判断函数运行在不同环境中this所指代的对象
**环境对象**：指的是函数内部特殊的变量this，它代表着当前函数运行时所处的环境

**作用**：可以让我们的代码更简洁

* 函数的调用方式不同，this指代的对象也不同
* 【**谁调用，this就是谁**】是判断this指向的粗略规则
* 直接调用函数，其实相当于是window.函数，所以 this 指代window

### 回调函数

目标：能够说出什么是**回调函数**
如果将函数A做为参数传递给函数B时，我们称函数A 为**回调函数**

* 常见使用场景：

  定时器

  事件监听

### tab栏切换案例

**tab栏实现布局**

![image-20250404191424794](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250404191424794.png)



```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>tab栏切换</title>
  <style>
    * {
      margin: 0;
      padding: 0;
    }

    .tab {
      width: 590px;
      height: 340px;
      margin: 20px;
      border: 1px solid #e4e4e4;
    }

    .tab-nav {
      width: 100%;
      height: 60px;
      line-height: 60px;
      display: flex;
      justify-content: space-between;
    }

    .tab-nav h3 {
      font-size: 24px;
      font-weight: normal;
      margin-left: 20px;
    }

    .tab-nav ul {
      list-style: none;
      display: flex;
      justify-content: flex-end;
    }

    .tab-nav ul li {
      margin: 0 20px;
      font-size: 14px;
    }

    .tab-nav ul li a {
      text-decoration: none;
      border-bottom: 2px solid transparent;
      color: #333;
    }

    .tab-nav ul li a.active {
      border-color: #e1251b;
      color: #e1251b;
    }

    .tab-content {
      padding: 0 16px;
    }

    .tab-content .item {
      display: none;
    }

    .tab-content .item.active {
      display: block;
    }
  </style>
</head>

<body>
  <div class="tab">
    <div class="tab-nav">
      <h3>每日特价</h3>
      <ul>
        <li><a class="active" href="javascript:;">精选</a></li>
        <li><a href="javascript:;">美食</a></li>
        <li><a href="javascript:;">百货</a></li>
        <li><a href="javascript:;">个护</a></li>
        <li><a href="javascript:;">预告</a></li>
      </ul>
    </div>
    <div class="tab-content">
      <div class="item active"><img src="./images/tab00.png" alt="" /></div>
      <div class="item"><img src="./images/tab01.png" alt="" /></div>
      <div class="item"><img src="./images/tab02.png" alt="" /></div>
      <div class="item"><img src="./images/tab03.png" alt="" /></div>
      <div class="item"><img src="./images/tab04.png" alt="" /></div>
    </div>
  </div>
  <script>
    // 1.实现经过导航栏高亮效果
    // 1.1 获取所有a标签
    const as = document.querySelectorAll('.tab-nav a')
    // const items = document.querySelectorAll('.tab-content div.item')
    // console.log(as)
    console.log(items)
    // 1.2 循环遍历每一个a标签，添加鼠标进入这个标签的监听
    for (let i = 0; i < as.length; ++i) {
      // console.log(as[i])
      as[i].addEventListener('mouseenter', function() {
        // console.log("鼠标经过了")
        // 去除原来高亮的带有active类的a的active类
        document.querySelector('.tab-nav .active').classList.remove('active')
        // 给当前的a标签(this)添加active类
        this.classList.add('active')

        // 根据导航栏的切换，切换下面的商品内容等
        // 让之前的内容不显示，将之前有active的item去除active类
        document.querySelector('.tab-content .item.active').classList.remove('active')
        // 显示当前对应的商品，将选中导航标签对应的内容添加active类
        // as[i] 与 items[i] 一一对应
        // items[i].classList.add('active')
        document.querySelector(`.tab-content .item:nth-child(${i+1})`).classList.add('active')
      })
    }
  </script>
</body>

</html>
```









