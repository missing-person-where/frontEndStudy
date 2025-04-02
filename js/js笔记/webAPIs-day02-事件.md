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

  * 也是一个对象，这个对象中有事件触发时 的相关信息

* 使用场景

  * 可以判断鼠标点击了哪个元素，比如按下enter可以发布新闻
  * 可以判断鼠标点击了哪个元素，从而执行相应的操作


  





