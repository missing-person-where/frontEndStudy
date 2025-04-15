# WebAPIs-Day05

## Window对象

### BOM

* 浏览器对象模型

  ![image-20250408211324854](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250408211324854.png)

* window对象是一个全局对象，也可以说是JavaScript中的顶级对象
* 像document、alert()、console.log()这些都是window的属性，基本BOM的属性和方法都是window的。
* 所有通过var定义在全局作用域中的变量、函数都会变成window对象的属性和方法
* window对象下的属性和方法调用的时候可以省略window

```javascript
// document.querySelector()
// window.document.querySelector()
console.log(document === window.document)
function fn() {
    console.log(111)
}
window.fn()
var num = 10
console.log(window.num)
```

### 定时器-延时函数

#### 介绍

* JS内置的一个可以用来让代码延迟执行的函数，叫setTImeout

* 语法：

  ```javascript
  setTimeout(回调函数, 等待的毫秒数)
  ```

* setTimeout只会执行移除，可以理解为把一段代码延迟执行，平时会省略window

* **清除延时函数：**

  ```javascript
  let timer = setTimeout(回调函数, 等待的毫秒数)
  clearTimeout(timer)
  ```

* **注意点：**

  * 延时器需要等待，所以后面的代码先执行
  * 每一次调用延时器就会产生一个新的延时器

* **两种定时器对比**：执行的次数

* 延时函数：执行一次

* 间歇函数：反复执行

**5秒后消失的广告案例**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    img {
      position: fixed;
      left: 0;
      bottom: 0;
    }
  </style>
</head>
<body>
  <img src="./images/ad.png" alt="">
  <script>
    // 1. 获取元素
    const img = document.querySelector('img')
    setTimeout(function() {
      img.style.display = 'none'
    }, 3000)
  </script>
</body>
</html>
```



### JS执行机制

JavaScript 语言的一大特点就是**单线程**，也就是说，**同一时间只能做一件事**

这是因为Javascript这门脚本语言诞生的使命所致------JavaScript是为处理页面中用户的交互，以及操作DOM而诞生的。比如我们对某个DOM元素进行添加和删除操作，不能同时进行。应该先进行添加，之后再删除。

单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。这样所导致的问题是：如果JS执行的时间过长，这样就会造成页面的渲染不连贯，导致页面渲染加载阻塞的感觉。



* 为了解决这个问题，利用多核CPU的计算能力，HTML5 提出WebWorker标准，允许JavaScript脚本创建多个线程。于是，JS 中出现了**同步**和**异步**。

#### 同步任务

同步任务都在主线程上执行，形成一个执行栈

![image-20250408214327420](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250408214327420.png)

#### 异步任务

JS一步通过回调函数实现

一般而言，异步任务分为以下三种类型：

1. 普通事件，如click，resize等
2. 资源加载，如load，error等
3. 定时器，包含setInterval，setTimeout  等

异步任务相关添加到**任务队列**中（任务队列也称为消息队列）

![image-20250408214538680](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250408214538680.png)

#### 执行机制

1. 先执行**执行栈中的同步任务**
2. 异步任务放在任务队列中
3. 一旦执行栈中的所有同步任务执行完毕，系统就会按次序读取任务队列中的异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行。

![image-20250408215038040](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250408215038040.png)



![image-20250408215506366](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250408215506366.png)

由于主线程不断的重复获得任务、执行任务、再获取任务、再执行，所以这种机制被称为**事件循环（eventloop**）。

### location对象

* 是对象，它拆分并保存了url的各个组成部分
* **常用属性和方法：**
* href属性获取完整的URL地址，对其**赋值**时用于**地址的跳转**
* ![image-20250408220410489](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250408220410489.png)

##### 5s后自动跳转

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    span {
      color: red;
    }
  </style>
</head>
<body>
  <a href="https://baidu.com">支付成功<span>5</span>秒钟之后跳转到首页</a>
  <script>
    const a = document.querySelector('a')
    let num = 5
    // 2. 开启定时器
    let timer = setInterval(function() {
      num--
      a.innerHTML = `
        <a href="https://baidu.com">支付成功<span>${num}</span>秒钟之后跳转到首页</a>
      `
      // 停止定时器，并跳转
      if (num === 0) {
        clearInterval(timer)
        location.href = a.href  // 跳转后不能回去了
        // a.click()  // 跳转后可以回去
      }
    }, 1000)
  </script>
</body>
</html>
```

* search属性获取地址中携带的参数，符号？后面部分

  ![image-20250408221846919](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250408221846919.png)

* hash属性获取地址中的哈希值，#号后面（网易云音乐是个例子）

* 后期vue的铺垫，经常用于不刷新页面，显示不同页面，比如网易云

* reload方法用来刷新当前页面，传入参数true代表强制刷新

  ```javascript
  location.reload()
  ```

  

### navigator对象

* 数据类型：对象，该对象记录了浏览器自身的相关信息

* **常用属性和方法：**

* 通过userAgent 检测浏览器的版本及平台

  ```javascript
  // 检测 userAgent（浏览器信息）
  !(function () {
      const userAgent = navigator.userAgent
      // 验证是否为Android或iPhone
      const android = userAgent.match(/(Android);?[\s\/]+([\d.]+)?/)
      const iphone = userAgent.match(/(iPhone\sOS)\s([\d_]+)/)
      // 如果是Android或iPhone，则跳转至移动站点
      if (android || iphone) {
          location.href = 'http://m.itcast.cn'
      }
  })();
  ```



### history对象

* history的数据类型是对象，主要管理历史记录，该对象与浏览器地址栏的操作相对应，如前进、后退、历史记录等

* **常用属性和方法：**

  | history对象方法 | 作用                                                        |
  | --------------- | ----------------------------------------------------------- |
  | back()          | 可以后退功能                                                |
  | forward()       | 前进功能                                                    |
  | go(参数)        | 前进后退功能 参数如果是1 前进一个页面 如果是-1 后退一个页面 |

* history对象一般在开发中比较少用，但是会在一些OA办公系统见到

* ![image-20250409192139409](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250409192139409.png)



## 本地存储

### 介绍

* 1.数据存储到浏览器中
* 2.设置，读取方便，甚至页面都不会丢失数据
* 3.容量较大，sessionStorage和localStorage约5M左右：
* 常见使用场景：
* https://todomvc.com/examples/vanilla-es6/页面刷新数据不丢失

### 分类-localStorage

* **作用：**可以将数据永久存储到本地（用户的电脑），除非手动删除，否则关闭页面也会存在
* **特性：**
* 可以多窗口（页面）共享（同一浏览器可以共享）
* 以键值对的形式存储使用
* **语法：**
  * **存储数据：**`localStorage.setItem(key, value)`
  * **获取数据：**`localStorage.getItem(key)`
  * **删除数据：**`localStorage.removeItem(key)`

### 分类-sessionStorage

* **特性：**
* 生命周期为关闭浏览器窗口
* 在同一个窗口（页面）下可以共享
* 以键值对的形式存储使用
* 用法和localStorage基本相同



### 存储复杂数据类型

* **解决：**将复杂数据类型转换成JSON类型的字符串，再存储到本地
* **语法：JSON.stringify(复杂数据类型)**
* **获取数据：**`JSON.parse(JSON字符串)`

```javascript
const obj = {
      uname: '张三', 
      age: 18,
      gender: '男'
    }
    // // 存储复杂数据类型   无法直接使用
    // localStorage.setItem('obj', obj)
    // // 取
    // console.log(localStorage.getItem('obj'))  [object object]
    
    // 1. 复杂数据类型存储必须转换成 JSON字符串存储
    console.log(JSON.stringify(obj))
    localStorage.setItem('obj', JSON.stringify(obj))
    // 取
    // JSON 对象  属性和值有引号。而且引号统一是双引号
    // {"uname":"张三","age":18,"gender":"男"}
    // console.log(localStorage.getItem('obj'))
    // console.log(typeof localStorage.getItem('obj'))
    // 2. 把JSON字符串转换为  对象
    console.log(JSON.parse(localStorage.getItem('obj')))
```

### 综合案例-学生就业统计表

**通过本地存储实现**

**字符串拼接新思路：**（**效率高**，开发常用的写法）

* 利用**map()** 和**join()** 数组方法实现字符串拼接
* **使用场景：**

map 可以用来遍历数组**处理数据**，并**返回新的数组**

**数组中map方法**  迭代数组

![image-20250409203613550](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250409203613550.png)

**map 也称为映射。**映射是一个术语，表示两个元素的集之间元素相互”对应“的关系

**map重点在于有返回值，**forEach没有返回值

**forEach遍历**

```javascript
const arr = ['red', 'blue', 'pink']
// forEach遍历  缺点：不能修改数组的值
arr.forEach(function(ele, index, array){
    console.log(ele)   // 当前遍历到的元素
    console.log(index)  // 索引
    console.log(array)  // 当前遍历的数组
    console.log(this)
}, arr)  // 第二个参数arr表示执行回调函数中this的值
```

**数组中的Join方法**

* **作用：**

  join方法用于把数组中的所有元素**转换成一个字符串**

* **语法：**

  ![image-20250409204952393](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250409204952393.png)

* **参数：**

  数组元素是通过参数里面指定的分隔符进行分隔的，**空字符串**（‘’），则所有元素之间**没有任何字符**

#### 渲染业务

**通过map+join方法渲染页面思路：**

map遍历数组**处理数据**生成tr，返回一个**数组**

然后把数组通过 join('')连接**转换成为字符串**

接着把字符串通过innerHTML**追加给tbody**



#### 新增业务

1. 对于**表单提交**事件，需要考虑**阻止表单默认行为**（**e.preventDefault()**）

2. 对于**数据填入并提交**，需要检测表单中的value值是否存在，如果有的表单**value**为**空**，就弹出警告框，并返回结束函数
3. 给arr数组追加对象，里面**存储表单获取过来的数据**
4. **渲染页面**和**重置表单**（**reset()**方法）
5. 把数组数据存储到**本地存储**中，利用**JSON.stringify()**存储为JSON字符串



#### 删除业务

1. 用**事件委托**方式，给**tbody**注册点击事件
2. 得到当前点击的索引号。渲染数据的时候，动态给a链接添加自定义属性data-id="0"
3. 根据索引号，利用splice删除这组数据
4. 重新渲染数据
5. 修改本地存储数据

关于stuid的处理

1. 新增加序号应该是**最后一条数据的stuId + 1**
* **数组长度[数组长度 - 1]**.stuId + 1 
2.  但是需要判断，如果没有数据就直接赋值为1，否则采用上面的做法
