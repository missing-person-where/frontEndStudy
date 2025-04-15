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
