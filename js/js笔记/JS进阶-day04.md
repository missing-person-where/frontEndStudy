# JS进阶-day04

## 深浅拷贝

### 浅拷贝

首先浅拷贝和深拷贝只针对**引用类型**

浅拷贝：拷贝的是地址，**只会拷贝一层**

常见方法：

1.拷贝对象：Object .**assign**() / 展开运算符 {**...obj**} 拷贝对象

2.拷贝数组：`Array.prototype.concat()` 或者 [...arr]

```javascript
const obj = {
    uname: 'pink', 
    age: 18, 
    family: {
        baby: '小pink'
    }
}
// 浅拷贝
// const o = {...obj}
const o = {}
Object.assign(o, obj)
console.log(o)  
o.age = 20
o.family.baby = '老baby'
console.log(o)  
console.log(obj) 
```

### 深拷贝

首先浅拷贝和深拷贝只针对**引用类型**

浅拷贝：拷贝的是对象，而不是地址

常见方法：

1.通过递归（自己调用自己的函数，容易栈溢出，需要有退出条件return）实现

```javascript
const obj = {
    uname: 'pink', 
    age: 18, 
    hobby: ['乒乓球', '足球'], 
    family: {girl: 'Lpink', boy: 'Rpink'}
}
const o = {}
// 拷贝函数
function deepCopy(newObj, oldObj) {
    for (let k in oldObj) {
        // 处理数组的问题
        if (oldObj[k] instanceof Array) {
            newObj[k] = []
            deepCopy(newObj[k], oldObj[k])
        } 
        // 处理对象的问题
        else if(oldObj[k] instanceof Object) {
            newObj[k] = {}
            deepCopy(newObj[k], oldObj[k])
        } 
        else {
            // k 属性名(字符串)  oldObj[k]  属性值
            newObj[k] = oldObj[k]
        }
    }
    return newObj
}
deepCopy(o, obj)
console.log(o)
o.age = 20
o.hobby[0] = '篮球'
o.family.girl = 'gPink'
console.log(o)
console.log(obj)
console.log([1, 23] instanceof Object)
```

2.`lodash`/`cloneDeep`

```html
<!-- 先引用 -->
<script src="./lodash.min.js"></script>
<script>
    const obj = {
        uname: 'pink', 
        age: 18, 
        hobby: ['乒乓球', '足球'], 
        family: {girl: 'Lpink', boy: 'Rpink'}
    }
const o = _.cloneDeep(obj)
console.log(o)
o.family.girl = 'gPink'
console.log(o)
console.log(obj)
</script>
```

3.通过`JSON.stringfy()`实现

```javascript
const obj = {
    uname: 'pink', 
    age: 18, 
    hobby: ['乒乓球', '足球'], 
    family: {girl: 'Lpink', boy: 'Rpink'}
}
// 对象转成 JSON 字符串
// JSON.stringify(obj)
const o = JSON.parse(JSON.stringify(obj))
o.family.girl = '121'
console.log(obj)
```

## 异常处理

### throw 抛异常

* 会终止程序
* throw 异常信息
* throw new Error(异常信息)可以显示更多提示信息

```javascript
 function fn(x, y) {
     if (!x || !y) {
         // throw '传参有误'
         throw new Error('传参有误')
     }

     return x + y
 }
console.log(fn())
```



### try/catch 捕获异常

try 试试 catch 拦住 finally 最后

```javascript
function fn() {
    try {
        // 可能出错的代码
        const p = document.querySelector('.p')
        p.style.color = 'red'
    } catch (err) {
        // 拦截错误，提示浏览器提供的错误信息，但是不中断程序的执行
        console.log(err.message)
        // 需要加return才能中断程序
        throw new Error('选择器出错了')
        // return
    } finally {
        // 不管程序是否报错，一定会执行
        alert('弹出对话框')
    }

    console.log(111)
}
fn()
```



### debugger

在JS代码任意位置写debugger，相当于在某个位置打了一个断点

```javascript
const obj = {
    uname: 'pink', 
    age: 18, 
    hobby: ['乒乓球', '足球'], 
    family: {girl: 'Lpink', boy: 'Rpink'}
}
const o = {}
// 拷贝函数
function deepCopy(newObj, oldObj) {
    debugger
    for (let k in oldObj) {
        // 处理数组的问题
        if (oldObj[k] instanceof Array) {
            newObj[k] = []
            deepCopy(newObj[k], oldObj[k])
        } 
        // 处理对象的问题
        else if(oldObj[k] instanceof Object) {
            newObj[k] = {}
            deepCopy(newObj[k], oldObj[k])
        } 
        else {
            // k 属性名(字符串)  oldObj[k]  属性值
            newObj[k] = oldObj[k]
        }
    }
    return newObj
}
deepCopy(o, obj)
console.log(o)
o.age = 20
o.hobby[0] = '篮球'
o.family.girl = 'gPink'
console.log(o)
console.log(obj)
console.log([1, 23] instanceof Object)
```



## 处理this

### 普通函数

谁调用，就指向谁

普通函数没有明确调用者时 this值 为 window，严格模式下(js代码开头声明`'use strict'`)没有调用者时 this 的值为undefined

```javascript
// 普通函数 谁调用，就指向谁
console.log(this)  // window
function fn() {
    console.log(this)  // window
}
window.fn()
window.setTimeout(function() {
    console.log(this)  // window
}, 1000)
document.querySelector('button').addEventListener('click', function(){
    console.log(this)  // 指向button
}) 
const obj = {
    sayHi: function() {
        console.log(this)  // 指向obj
    }
}
obj.sayHi()
```

### 箭头函数

1.箭头函数默认会绑定外层this的值，所以箭头函数this的值和外层的this是一样的

2.箭头函数中的this引用的就是最近作用域中的this

3.向外层作用域，一层一层查找this，直到有this的定义

事件监听的回调函数如果要用到this，不要写箭头函数，原型对象中的函数要用到原来对象的引用（this），也不要写成箭头函数，构造函数也不能用箭头函数，因为会用到this

### 改变this

有些方法中可以动态指定被调用函数中this的指向

### `call()`

用来调用函数，同时指定被调用函数中this的值

* 语法

> `fun.call(thisArg, arg1, arg2, ...)`
>
> `thisArg`: 在fun函数 运行时指定 的this的值
>
> `arg1, arg2...`:fun函数传递的其它参数
>
> 返回值就是函数的返回值，call就是用来调用函数的

```javascript
const obj = {
    uname: 'pink'
}
function fn(x, y) {
    console.log(this)
    console.log(x, y)
}
// 1. 调用函数
// 2. 改变this指向
// fn()
fn.call(obj, 1, 2)
```

### `apply()`

用来调用函数，同时指定被调用函数中this的值

用来调用函数，同时指定被调用函数中this的值

* 语法

> `fun.apply(thisArg, [argsArray])`
>
> `thisArg`: 在fun函数 运行时指定 的this的值
>
> `argsArray`:fun函数传递的其它参数，必须以数组传递
>
> 返回值就是函数的返回值，call就是用来调用函数的
>
> apply主要与数组有关，比如使用`Math.max()`  求数组最大值

```javascript
const obj = {
    uname: 'pink'
}
function fn(x, y) {
    console.log(this)
    console.log(x, y)
}
// 1. 调用函数
// 2. 改变this指向
// fn.apply(thisArg, [argsArray])
// fn()
fn.apply(obj, [1, 2])
// 3. 返回值：调用函数的返回值

// 使用场景：求数组最大值
const arr = [100 ,44, 77]
const max = Math.max.apply(Math, arr)
const min = Math.min.apply(null, arr)
console.log(max, min)

// 使用场景：求数组最大值
console.log(Math.max(...arr))
```

### `blind()`-重要

* bind() 方法不会调用函数。但是能改变函数内部this指向
* 语法

> `fun.call(thisArg, arg1, arg2, ...)`
>
> `thisArg`: 在fun函数 运行时指定 的this的值
>
> `arg1, arg2...`:fun函数传递的其它参数
>
> 返回有指定this值的**新函数（原函数拷贝）**
>
> 如果不想修改原函数，但是需要修改this的值，可以使用这个方法生成一个指定this的原函数的拷贝函数

## 性能优化

### 防抖(debounce)

* **防抖：**单位时间内，频繁触发事件，**只执行最后一次**

* **例子：**王者回城，被打断就得重新来
* **使用场景：**
* **搜索框搜索输入**。只需用户**最后**一次输入完成，再发生请求。
* 手机号，邮箱验证**输入检测**

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Document</title>
  <style>
    .box {
      width: 300px;
      height: 300px;
      background-color: #ccc;
      color: #fff;
      text-align: center;
      font-size: 100px;
    }
  </style>
</head>

<body>
  <div class="box">0</div>
  <script src="./lodash.min.js"></script>
  <script>
    // 利用防抖实现性能优化
    // 需求：鼠标在盒子上移动，里面的数字就会变化 + 1
    const box = document.querySelector('.box')
    let i = 1
    function mouseMove() {
      box.innerHTML = i++
      // 如果里面存在大量消耗性能的代码，比如dom操作，比如数据处理，可能造成卡顿
    }
    // 添加事件
    // box.addEventListener('mousemove', mouseMove)

  </script>
</body>

</html>
```

利用防抖处理-鼠标滑过盒子显示文字

要求：鼠标在盒子上移动，鼠标停止500ms之后，里面的数字才会变化+1

实现方式：

1.**lodash** 提供的防抖处理

```javascript
// 利用lodash库实现防抖 - 500ms之后采取+1
// 语法：_.debunce(fun, 时间)
box.addEventListener('mousemove', _.debounce(mouseMove, 500))
```

2.**手写**一个防抖函数处理

```javascript
// 手写防抖函数
// 核心是利用 setTimeout定时器实现
// 1. 声明定时器变量
// 2. 每次事件触发的时候都要判断是否有定时器，如果有先清除以前的定时器
// 3. 如果没有定时器，开启定时器，存入到定时器变量中
// 4. 定时器里面写函数调用 
function debounce(fn, t) {
    let timer
    // return 返回一个匿名函数
    return function() {
        if (timer) {
            // 清除原来的定时器
            clearTimeout(timer)
        }
        timer = setTimeout(function(){
            fn()  // 调用要执行的函数
        }, t)
    }
}
box.addEventListener('mousemove', debounce(mouseMove, 500))
```

### 节流(throttle)

* **节流：**单位时间内，频繁触发事件，**只执行一次**
* 例子：
* **王者技能冷却**，期间无法继续释放技能
* **和平精英 98k** 换子弹期间不能射击
* 使用场景：
* 高频事件：鼠标移动**mosuemove**，页面尺寸缩放**resize**，滚动条滚动**scroll等等**

案例：鼠标在盒子上移动，不管移动多少次，**每隔500ms才+1**

实现方式：

1.**lodash** 提供的节流函数处理

```javascript
// 利用lodash库实现防抖 - 500ms之后采取+1
// 语法：_.throttle(fun, 时间)
box.addEventListener('mousemove', _.throttle(mouseMove, 3000))
```

2.**手写**一个节流函数处理

```javascript
// 手写节流函数
// 1.声明一个定时器变量
// 2.当鼠标每次移动都需要判断是否有定时器，如果有定时器就不开启新定时器
// 3.如果没有定时器开启定时器，需要存到变量中
// -定时器里面调用执行的函数
// -定时器里面需要把定时器清空
function throttle(fn, t) {
    // 声明一个记录定时器的变量
    let timer = null
    return function () {
        if (!timer) {
            timer = setTimeout(function () {
                fn()
                // fn执行完毕关闭定时器
                // 不能用clearTimeout
                timer = null
            }, t)
        }
    }
}
box.addEventListener('mousemove', throttle(mouseMove, 500))
```

## 节流-综合案例

两个事件：

1：ontimeupdate  事件在视频/音频（audio/video）当前的播放位置发送改变时触发

2：onloadeddata 事件在当前帧的数据浇在完成且还没有足够的数据播放视频/音频（audio/video）的下一帧时触发

ontimeupdate需要节流，触发频次很高，设定1s执行一次

### 页面打开，可以记录上一次的视频播放位置

思路：
1. 在ontimeupdate事件触发的时候，每隔1秒钟，就记录当前时间到本地存储
2. 下次打开页面，onloadeddata事件触发，就可以从本地存储取出时间，让视频从取出的时间播放，如果没有就默认为0s
3. 获得当前时间video.currentTime

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="referrer" content="never" />
  <title>综合案例</title>
  <style>
    * {
      padding: 0;
      margin: 0;
      box-sizing: border-box;
    }

    .container {
      width: 1200px;
      margin: 0 auto;
    }

    .video video {
      width: 100%;
      padding: 20px 0;
    }

    .elevator {
      position: fixed;
      top: 280px;
      right: 20px;
      z-index: 999;
      background: #fff;
      border: 1px solid #e4e4e4;
      width: 60px;
    }

    .elevator a {
      display: block;
      padding: 10px;
      text-decoration: none;
      text-align: center;
      color: #999;
    }

    .elevator a.active {
      color: #1286ff;
    }

    .outline {
      padding-bottom: 300px;
    }
  </style>
</head>

<body>
  <div class="container">
    <div class="header">
      <a href="http://pip.itcast.cn">
        <img src="https://pip.itcast.cn/img/logo_v3.29b9ba72.png" alt="" />
      </a>
    </div>
    <div class="video">
      <video src="https://v.itheima.net/LapADhV6.mp4" controls></video>
    </div>
    <div class="elevator">
      <a href="javascript:;" data-ref="video">视频介绍</a>
      <a href="javascript:;" data-ref="intro">课程简介</a>
      <a href="javascript:;" data-ref="outline">评论列表</a>
    </div>
  </div>
  <script src="lodash.min.js"></script>
  <!-- <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script> -->
  <script>
    // 1. 获取元素  对视频进行操作
    const video = document.querySelector('video')
    video.ontimeupdate = _.throttle(() => {
      // 获得当前视频的时间
      // console.log(video.currentTime)
      // 把当前的时间存储到本地存储
      localStorage.setItem('currentTime', video.currentTime)
    }, 1000)

    // 打开页面触发事件，从本地存储取出记录时间  复制给video.currentTime
    video.onloadeddata = () => {
      // console.log(111)
      // console.log(localStorage.getItem('currentTime'))
      video.currentTime = localStorage.getItem('currentTime') || 0
    }

  </script>
</body>

</html>
```

