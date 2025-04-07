# webAPIs-day03

### 全选反选案例

全选设置为类名为allCheck的复选框，一些子选项设置为类名为ck的复选框

**注意：对于复选框的属性checked，如果选中为true，未选中为false**

#### 全选实现

监听全选的复选框的点击事件，将所有子选项的checked设置为全选复选框的checked

#### 反选实现

监听每一个子选项复选框的点击事件，如果发现所有子选项复选框都被选中（可以通过for遍历的方式），就设置全选复选框被选中；否则设置为未选中

**伪类选择器：**`:checked` 可以选中所有被选中的复选框

通过伪类选择器`:checked`，选中所有子选项中被选中的复选框，如果和可选的子选项复选框数量一致，就设置全选复选框被选中；否则设置为未选中

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

### 事件流

![image-20250405170605498](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250405170605498.png)

 #### 事件捕获

大 => 小  父=>子

* 概念：

  从DOM的根元素开始去执行对应的同名事件（从外到里）

* 事件捕获需要写对应代码才能看到效果

* 代码：

  ```javascript
  DOM.addEventListener(事件类型, 事件处理函数, 是否使用捕获机制)
  ```

* 说明：

  * addEventListener第三个参数传入true代表是捕获阶段触发（很少使用）
  * 若传入false代表冒泡阶段触发，默认就是false
  * 若是用L0事件监听则只有冒泡阶段，没有捕获

#### 事件冒泡

**概念：**

当一个元素的事件被触发时，中依次被触发。这一过程被称为事件冒泡

* 简单理解：当一个元素触发事件后，会依次向上调用父级元素的**同名事件**
* 事件冒泡是默认存在的
* L2事件监听第三个参数是false，或者默认都是冒泡

#### 阻止冒泡

* **问题：**因为默认就有冒泡模式的存在，所以容易导致事件影响到父级元素

* **需求：**若想把事件就限制在当前元素内，就需要阻止事件冒泡

* **前提：**阻止事件冒泡需要拿到事件对象

* **语法：**

  `事件对象.stopPropation()`

  `e.preventDefault()`: 链接的跳转，表单域跳转

* **注意：**此方法可以阻断事件流动传播，不光在冒泡阶段有效，捕获阶段也有效

#### 解绑事件

on事件方式，直接用null覆盖就行

addEventListener方式，必须使用：

removeEventListener(事件类型, 事件处理函数, [获取捕获或者冒泡阶段])

eg:

![image-20250405173621759](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250405173621759.png)

**注意：匿名函数无法被解绑**

#### 鼠标经过事件的区别

* 鼠标经过事件：
  * mouseover 和 mouseout 
  * mouseenter和mouseout

#### 两种注册事件的区别

![image-20250405175015003](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250405175015003.png)

### 事件委托

事件委托是利用事件流的特征解决一些开发需求的知识技巧

* 优点：减少注册次数，可以提高程序性能

* 原理：事件委托其实是利用事件冒泡的特点

  * 给**父元素注册事件**，当我们触发子元素的时候，会冒泡到父元素身上，从而触发父元素的事件

  ![image-20250405175954693](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250405175954693.png)

**事件对象.target**可以获得触发事件的DOM元素

**DOM元素.tagName**可以获取这个DOM元素的标签名

## 其它事件

### 页面加载事件

* 加载外部资源（如图片、外联CSS和JavaScript等）加载完毕时触发的事件

* 为什么学？

  * 有些时候需要等页面资源全部处理完了做一些事情
  * 老代码喜欢把script写在head中，这时候直接找dom元素找不到

* 事件名：**load**

* 监听页面所有资源加载完毕：

  * 给**window**添加load事件

  ![image-20250406094547527](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250406094547527.png)

  * 注意：不光可以监听整个页面资源加载完毕，也可以针对某个资源绑定load事件

* 当初始的HTML 文档被完全加载和解析完成之后，DOMContentLoaded事件被触发，而**无需等待样式表、图像等完全加载**

* 事件名：**DOMContentLoaded**

* 监听页面DOM加载完毕：

  * 给**document**添加DOMContentLoaded事件

  ![image-20250406095413230](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250406095413230.png)

  

### 元素滚动事件

* 滚动条在滚动的时候持续触发的事件

* 为什么学？

  * 很多网页需要检测用户把页面滚动到某个区域后做一些处理，比如固定导航栏，比如返回顶部

* 事件名：scroll

* 监听整个页面滚动

  ![image-20250406095926523](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250406095926523.png)

  * 给window或者document 添加scroll事件

* 监听某个元素内部滚动直接给某个元素添加即可

* 使用场景：

* 我们想要页面滚动一段距离，比如100px，就让某些元素

* 显示隐藏，那我们怎么知道，页面滚动了100像素呢?

* 就可以使用scroll 来检测滚动的距离~

* scrollLeft和scrollTop（属性）

  * 获取被卷去的大小
  * 获取元素内容往左，往上滚除去看不到的距离
  * 这两个值是可**读写**的

* 开发中，我们经常检测页面滚动的距离，比如页面滚动100像素，就可以显示一个元素，或者固定一个元素

* scrollTo() 方法可以把内容滚动到指定的坐标

  ![image-20250406110831352](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250406110831352.png)

### 页面尺寸事件

* 会在窗口尺寸改变的时候触发事件：

  * resize：

  ![image-20250406111118082](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250406111118082.png)

* 检测屏幕宽度：

  ![image-20250406111453335](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250406111453335.png)

* 获取宽高：

  * 获取元素的可见部分宽高（**不包含边框，margin，滚动条等**）

  * clientWidth和clientHeight

    ![image-20250406111631663](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250406111631663.png)

### 元素尺寸与位置

* 使用场景：
* 通过**js计算元素在页面中的位置**
* 当元素移动到某个位置，可以做某些操作

#### 尺寸

* 获取宽高：
  * 获取元素的自身宽高，包含元素自身的**宽高，padding，border，滚动条**
  * offsetWidth和offsetHeight
  * 获取的结果是数值，方便计算
  
* 获取位置：
  * 获取元素距离自己定位的**父级元素**的左，上距离
  
  * 以哪个父级为准？
    * 带有定位的父级
    * 如果都没有以 文档左上角为准
    
  * **offsetLeft和offsetTop 注意是只读属性**
  
  * 2.element.getBoundingClientRect()
  
    方法返回元素的大小及其相对于**视口**的位置
  
  ![image-20250406155333520](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250406155333520.png)
