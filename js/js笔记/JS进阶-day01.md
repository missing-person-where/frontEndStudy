# JS进阶-day01

## 作用域（scope）

规定了变量能被访问的“范围”

### 局部作用域

1. **函数作用域：**

   在函数内部声明的变量只能在函数内部被访问，外部无法直接访问

   * 函数参数也被视为函数内部的变量，无法在函数外部访问
   * 函数内部声明的变量，在外部无法被访问
   * 不同函数内部声明的变量无法互相访问
   * 函数执行完毕后，函数内部的变量被清空了

2. **块作用域：**

   使用{}包裹的代码称为代码块，代码块内部声明的变量外部将【**有可能**】无法被访问

   * let声明的变量会产生块作用域，var不会产生块作用域
   * const声明的变量也会产生块作用域
   * 不同代码块之间的变量无法互相访问
   * 推荐使用let或const

### 全局作用域

**`<script>`**标签和**.js文件**的【最外层】就是所谓的全局作用域，在此声明的变量在函数内部也可以被访问。
全局作用域中声明的变量，任何其它作用域都可以被访问

注意：

1. 为window对象动态添加的属性默认也是全局的，不推荐！
2. 函数中未使用任何关键字声明的变量为全局变量，不推荐！！！
3. 尽可能少的声明全局变量，防止全局变量被污染

### 作用域链

作用域链本质上是底层的**变量查找机制**。
> 在函数被执行时，会**优先查找当前**函数作用域中查找变量
> 如果当前作用域查找不到则会**依次逐级查找父级作用域**直到全局作用域

总结：
1. 嵌套关系的作用域串联起来形成了作用域链
2. 相同作用域链中按着从小到大的规则查找变量
3. 子作用域能够访问父作用域，父级作用域无法访问子级作用域

### JS垃圾回收机制

**垃圾回收机制(GarbageCollection)**简称**GC**
JS中**内存**的分配和回收都是**自动完成**的，内存在不使用的时候会被**垃圾回收器**自动回收

#### **内存的生命周期**
JS环境中分配的内存，一般有如下**生命周期**：

1. **内存分配：**当我们声明变量、函数、对象的时候，系统会自动为他们分配内存
2. **内存使用：**即读写内存，也就是使用变量、函数等
   日
3. **内存回收：**使用完毕，由**垃圾回收器**自动回收不再使用的内存

**说明：**

* 全局变量一般不会回收（关闭页面回收）
* 一般情况下**局部变量的值**，不用了，会被**自动回收**掉

**内存泄漏：**程序中分配的**内存**由于某种原因程序**未释放**或**无法释放**叫做**内存泄漏**



#### **算法说明**

**引用计数法**：检查复杂数据类型被引用的次数，如果被引用了，引用次数加1，如果原来的引用消失了，引用次数减1，引用次数为0就可以释放掉对应的内存

致命问题：**嵌套引用**（循环引用）

如果两个对象**相互引用**，尽管它们不再使用，垃圾回收器不会进行回收，导致内存泄漏

![image-20250412115003024](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250412115003024.png)

引用次数永远不会是0，这样的相互引用如果大量存在就会导致大量的内存泄漏

**标记清除法**

现代的浏览器已经不再使用引用计数算法了。
现代浏览器通用的大多是基于**标记清除算法**的某些改进算法，总体思想都是一致的。
核心：

1. 标记清除算法将“不再使用的对象”定义为**“无法达到的对象”**。
2. 就是从**根部**（在JS中就是全局对象）出发定时扫描内存中的对象。凡是能从**根部到达**的对象，都是还**需要使用**的。
3. 那些**无法**由根部出发触及到的**对象被标记**为不再使用，稍后进行**回收**。

![image-20250412115435964](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250412115435964.png)

### 闭包

概念：一个函数对周围状态的引用捆绑在一起，内层函数中访问到其外层函数的作用域

简单理解：**闭包 = 内层函数 + 外层函数的变量**

一段简单代码：

````javascript
function outer() {
    const a = 1
    function f() {
        console.log(a)
    }
    f()
}
outer()
````

闭包作用：封闭数据，提供操作，外部也可以访问函数内部的变量

闭包的基本格式：

```javascript
function outer() {
    const a = 1
    function f() {
        console.log(a)
    }
    return f
}
const fun = outer()
fun() // 1
// 外层函数使用内部函数的变量
```

闭包应用：实现数据的私有

比如，我们要做个统计函数调用次数，函数调用一次，就++

```javascript
// 闭包应用，统计函数的调用次数
let i = 0
function fn() {
    i++
    console.log(`fn函数被调用了${i}次`)
}
fn()
fn()
i = 1000

```

**但是i是全局变量，容易被修改**

```javascript
// 闭包方式：统计函数的调用次数
function count() {
    let i = 0
    function fn() {
        i++
        console.log(`fn函数被调用了${i}次`)
    }

    return fn
}
const fun = count()
fun()  // 1
fun()  // 2
```



### 变量提升

它允许在变量声明之前即被访问（仅仅存在于var声明变量）

```javascript
// 1. 把所有var声明的变量提升到 当前作用域的最前面
// 2. 只是提升声明，不提示赋值
// var num
// console.log(num + '件')
// num = 10
// console.log(num)

function fn() {
    console.log(num)
    var num = 10
    }
fn()
```

注意：

1. 变量在未声明即被访问会报语法错误
2. 变量在var声明之前即被访问，变量的值为 undefined
3. 变量只会提升到前面被声明，不会被赋值
4. let/const声明的变量不存在变量提升
5. 变量提升出现在相同作用域中
6. **实际开发中，推荐先声明再访问变量**

## 函数进阶

### 函数提升

与变量提升类似，函数在声明之前即可被调用

```javascript
// function fn(){
//   console.log('函数提升')
// }
// 1. 会把所有函数声明提升到当前作用域的最前面
// 2. 只提升函数声明，不提升函数调用
fn()
function fn(){
    console.log('函数提升')
}

fun()  // 提升声明，但是不提升赋值，因此报错
var fun = function() {
    console.log('函数表达式')
}
// 函数表达式必须先赋值，再调用
```

总结：

1. 函数提升能够使函数的声明调用更灵活
2. 函数表达式不存在提升（函数体）的现象
3. 函数提升出现在相同作用域当中

### 函数参数

形参，实参，**默认参数**

#### 动态参数

**arguments** 是函数内部的伪数组变量，它包含调用函数时传入的所有实参

```javascript
 function getSum() {
     // arguments 动态参数 只存在于函数里面
     // 1. 是伪数组
     let sum = 0
     // console.log(arguments) [2, 3, 4]
     for (let i = 0; i < arguments.length; ++i) {
         sum += arguments[i]
     }
     console.log(sum)
 }
getSum(2, 3, 4)   // 9
getSum(1, 2, 3, 4)  // 10
```

总结：

1. arguments只存在于函数当中
2. 本质是一个伪数组（对象）
3. 可以用for循环遍历

#### 剩余参数

`（类型不用写）... 数组名`

```javascript
function getSum(a, b, ... arr) {
    // 返回的是一个Array对象
    console.log(arr)  // 使用不需要写 ...
}
getSum(2, 3)
getSum(1, 2, 3)
```



区别：arguments会捕获函数的所有传参，但是 ... arr这种语法可以传入**其它参数**，要求是**只能有一个**这样的形式参数，而且**其它的参数**必须**在这种剩余参数之前**，arguments是**伪数组**，arr是**真数组**

提倡使用 ...arr这种剩余参数  



展开运算符(...), 将一个数组进行展开

```javascript
const arr = [1, 2, 3, 4, 5]
console.log(...arr)  // 1 2 3 4 5
```
**运用场景**：求数组**最大值**（最小值），**合并数组**等

```javascript
const arr = [1, 2, 3, 4, 5]
// 求数组最大值和最小值
// ...arr === 1, 2, 3....
console.log(Math.max(...arr))   // 5
console.log(Math.min(...arr))   // 1
```

```javascript
const arr1 = [1, 2, 3, 4, 5]
// 合并数组
const arr2 = [11, 7, 9]
const arr = [...arr1, ...arr2]
console.log(arr)   // [1, 2, 3, 4, 5, 11, 7, 9]
```


说明：

1. 不会修改原数组

剩余参数：函数参数使用，得到的是真数组

展开运算符：用于数组，展开

### 箭头函数

目的：引入箭头函数的目的是更简短的函数写法并且不绑定this，箭头函数的语法比函数表达式更简洁
使用场景：箭头函数更适用于那些本来**需要匿名函数**的地方

属于表达式函数，不存在函数提升

#### **基本语法：**

```javascript
const fn = ()=>{
    console.log(123)
}
fn()  // 123
```

```javascript
const fn = (x)=>{
    console.log(x)
}
fn(100)  // 100
```

只有一个参数的时候，可以省略小括号
```javascript
// 只有一个参数的时候，可以省略小括号
const fn = x=>{
    console.log(x)
}
fn(100)  // 100
```
只有一行代码的时候，可以省略大括号，并自动作为返回值被返回
```javascript
// 只有一行代码的时候，可以省略大括号
const fn = x=>console.log(x)
fn(121)  // 121
```

只有一行代码的时候，可以省略return

```javascript
// 只有一行代码的时候，可以省略return
const fn = x => x + x
console.log(fn(1))  // 2
```

阻止表单默认行为，简写

```javascript
const form = document.querySelector('form')
form.addEventListener('click', ev=>ev.preventDefault())
```

可以直接返回一个对象，但是要用 () 包起来

加括号的函数体返回对象字面量表达式

````javascript
const fn = (uname)=> ({uname: uname})
console.log(fn('刘德华'))  // {uname: '刘德华'}
````

#### 箭头函数参数

1. 普通函数有arguments 动态参数
2. **箭头函数**没有arguments动态参数，但是**有...arr剩余参数**

```javascript
// 1. 利用箭头函数求和
const getSum = (...arr)=>{
    // arguments  // Uncaught ReferenceError: arguments is not defined
    let sum = 0
    for (let i = 0; i < arr.length; ++i){
        sum += arr[i]
    }
    return sum
}
const res = getSum(2, 3, 4)
console.log(res)  // 5
```

#### 箭头函数this

在箭头函数出现之前，每一个新函数根据它是被**如何调用**的来定义这个函数的this值，非常令人讨厌。

```javascript
// this指向： 谁调用这个函数，this就指向谁
console.log(this)   // window
// 普通函数
function fn() {
    console.log(this)  // window
}
window.fn()  // window

// 对象方法里面的this
const obj = {
    name: 'andy', 
    sayHi: function() {
        console.log(this)  // obj
    }
}
obj.sayHi()
```

**箭头函数不会创建自己的this**,它只会从自己的**作用域链的上一层**沿用this。

```javascript
// 2. 箭头函数的this  是上一层作用域链的this
const fn = ()=>{
    console.log(this)  // window
}
fn()
// 对象方法的箭头函数的 this
const obj = {
    uname: 'red', 
    sayHi: ()=>{
        console.log(this)  // this指向 window
    },
    p: this
}
console.log(obj.p)   // window
obj.sayHi()    // window
```

```javascript
const obj = {
    uname: '张三',
    sayHi: function() {
        console.log(this)   // obj
        let i = 10
        const count = ()=>{
            console.log(this)  // obj
        }
        count()
    }
}
console.log(obj.sayHi())
```

>**DOM事件回调函数不太推荐使用箭头函数，特别是需要用到this的时候**
>事件回调函数使用箭头函数时，this 为全局的window

## 解构赋值

#### 数组解构

数组解构是将数组的单元值**快速批量赋值**给一系列变量的简洁语法

基本语法：

1. 赋值运算符 = 右侧的 [] 用于批量声明变量，右侧数组的单元之被赋值给左侧的变量
2. 变量的顺序对应数组单元值得位置依次赋值操作

```javascript
// const arr = [100, 60, 80]
// 数组解构
// const [max, min, avg] = arr
const [max, min, avg] = [100, 60, 80]
// 等价于：
// const max = arr[0]
// const min = arr[1]
// const avg = arr[2]
console.log(max)  // 100
console.log(min)  // 60
console.log(avg)  // 80
```

**典型运用，交换两个变量**

```javascript
// 典型运用，交换两个变量
let a = 1
let b = 2;  // 必须加分号
[b, a] = [a, b]
console.log(a, b)  // 2 1
```

**JS必须加分号情况**

1. 立即执行函数
2. 数组解构

**变量多，数组单元值少的情况**

```javascript
// 1. 变量多于单元值 undefined
const [a, b, c, d] = [1, 2, 3]
console.log(a)  // 1
console.log(b)  // 2
console.log(c)  // 3
console.log(d)  // undefined
```

**变量多，数组单元值少的情况**

```javascript
// 2. 变量少于单元值
const [a, b] = [1, 2, 3]
console.log(a)  // 1
console.log(b)  // 2
```

**利用剩余参数解决数组单元值多于变量**

```javascript
// 3. 利用剩余参数解决数组单元值多于变量
const [a, b, ...c] = [1, 2, 3, 4]
console.log(a)  // 1
console.log(b)  // 2
console.log(c)  // [3, 4]
```

剩余参数还是一个**真数组**

**防止有undefined传递单元值的情况，可以设置默认值**

```javascript
 // 4. 防止有undefined传递单元值的情况，可以设置默认值
// const [a = 0, b = 0] = [1, 2]
const [a = 0, b = 1] = []
console.log(a)  // 0
console.log(b)  // 1
```

**按需导入赋值**

```javascript
// 5. 按需导入赋值
const [a, b, , d] = [1, 2, 3, 4]
console.log(a)  // 1
console.log(b)  // 2
console.log(d)  // 4
```

**支持多维数组的解构**

```javascript
 // const arr = [1, 2, [3, 4]]
// console.log(arr[0])  // 1
// console.log(arr[1])   // 2
// console.log(arr[2][0])  // 3
// console.log(arr[2][1]) // 4

// 多维数组解构
// const arr = [1, 2, [3, 4]]
// const [a, b, c] = [1, 2, [3, 4]]
// console.log(a)  // 1
// console.log(b)  // 2
// console.log(c)  // [3, 4]

const [a, b, [c, d]] = [1, 2, [3, 4]]
console.log(a)  // 1
console.log(b)  // 2
console.log(c)  // 3
console.log(d)  // 4
```



#### 对象解构

对象解构是将对象属性和方法快速批量赋值给一系列变量的简洁语法
**1．基本语法：**

1. 赋值运算符＝左侧的 {} 用于批量声明变量，右侧对象的属性值将被赋值给左侧的变量
2. 对象属性的值将被赋值给与属性名**相同的**变量
3. 注意解构的变量名不要和外面的变量名冲突，否则报错
4. 对象中找不到与变量名一致的属性时变量值为undefined

```javascript
// const obj = {
//   uname: '李四',
//   age: 18
// }
// obj.uname
// const uname = 'halo'
// 解构语法
const {uname, age} = {
    uname: '李四',
    age: 18
}
// 等价于 const uname = obj.uname
// 变量名必须和属性名一致
console.log(uname)  // 李四
console.log(age)    // 18
```

**2.给新的变量名赋值：**

```javascript
// 对象解构的变量名 可以重新改名  旧变量名:新变量名
const {uname: username, age} = {
    uname: '李四',
    age: 18
}
console.log(username)  // 李四
console.log(age)    // 18
```

**3.数组对象解构**

```javascript
// 2. 解构数组对象
const pig = [
    {
        uname: '佩奇',
        age: 6
    }
]
const [{uname, age}] = pig
console.log(uname)  // 佩奇
console.log(age)    // 6
```

**4.多级对象解构**

```javascript
const pig = {
    name: '佩奇',
    family: {
        mother: '猪妈妈', 
        father: '猪爸爸',
        sister: '乔治'
    },
    age: 10
}
// 多级对象解构
const {name, family:{mother, father, sister}, age} = pig
console.log(name)
console.log(mother)
console.log(father)
console.log(sister)
console.log(age)
```

```javascript
const person = [
    {
        name: '佩奇',
        family: {
            mother: '猪妈妈', 
            father: '猪爸爸',
            sister: '乔治'
        },
        age: 10
    }
]
const [{name, family:{mother, father, sister}, age}] = person
console.log(name)
console.log(mother)
console.log(father)
console.log(sister)
console.log(age)
```

#### forEach遍历数组

* forEach() 方法用于调用数组的每个元素，并将元素传递给回调函数

* 主要使用场景：**遍历数组的每个元素**

* **语法：**

  ```javascript
  被遍历的数组.forEach(function(当前数组元素, 当前数组元素索引){
      
  })
  ```

* **例子:**

  ```javascript
  const arr = ['red', 'green', 'purple']
  const res = arr.forEach(function(item, index){
      console.log(item, index)
  })
  console.log(res)
  /*
  red 0
  green 1
  purple 2
  undefined
  */
  ```

* 注意：

* forEach主要时用来遍历数组

* 参数当前数组元素时必须要写的，索引号可选

**渲染商品案例**

```html
<div class="list">
    <!-- <div class="item">
    <img src="" alt="">
        <p class="name"></p>
<p class="price"></p>
</div> -->
</div>
<script>
    const goodsList = [
        {
            id: '4001172',
            name: '称心如意手摇咖啡磨豆机咖啡豆研磨机',
            price: '289.00',
            picture: 'https://yanxuan-item.nosdn.127.net/84a59ff9c58a77032564e61f716846d6.jpg',
        },
        {
            id: '4001594',
            name: '日式黑陶功夫茶组双侧把茶具礼盒装',
            price: '288.00',
            picture: 'https://yanxuan-item.nosdn.127.net/3346b7b92f9563c7a7e24c7ead883f18.jpg',
        },
        {
            id: '4001009',
            name: '竹制干泡茶盘正方形沥水茶台品茶盘',
            price: '109.00',
            picture: 'https://yanxuan-item.nosdn.127.net/2d942d6bc94f1e230763e1a5a3b379e1.png',
        },
        {
            id: '4001874',
            name: '古法温酒汝瓷酒具套装白酒杯莲花温酒器',
            price: '488.00',
            picture: 'https://yanxuan-item.nosdn.127.net/44e51622800e4fceb6bee8e616da85fd.png',
        },
        {
            id: '4001649',
            name: '大师监制龙泉青瓷茶叶罐',
            price: '139.00',
            picture: 'https://yanxuan-item.nosdn.127.net/4356c9fc150753775fe56b465314f1eb.png',
        },
        {
            id: '3997185',
            name: '与众不同的口感汝瓷白酒杯套组1壶4杯',
            price: '108.00',
            picture: 'https://yanxuan-item.nosdn.127.net/8e21c794dfd3a4e8573273ddae50bce2.jpg',
        },
        {
            id: '3997403',
            name: '手工吹制更厚实白酒杯壶套装6壶6杯',
            price: '99.00',
            picture: 'https://yanxuan-item.nosdn.127.net/af2371a65f60bce152a61fc22745ff3f.jpg',
        },
        {
            id: '3998274',
            name: '德国百年工艺高端水晶玻璃红酒杯2支装',
            price: '139.00',
            picture: 'https://yanxuan-item.nosdn.127.net/8896b897b3ec6639bbd1134d66b9715c.jpg',
        },
    ]
const list = document.querySelector('.list')
// forEach + 数据解构 + 字符串拼接
let content = ''
goodsList.forEach(function(item){
    // 数组解构
    const {picture:src, name, price} = item
    const part = `
        <div class="item">
          <img src="${src}" alt="">
          <p class="name">${name}</p>
          <p class="price">${price}</p>
        </div>
      `
    content += part
})  
list.innerHTML = content

// map + 数据解构 + join
// const content = goodsList.map(function(item){
//   const {picture:src, name, price} = item
//   return `
//     <div class="item">
//       <img src="${src}" alt="">
//       <p class="name">${name}</p>
//       <p class="price">${price}</p>
//     </div>
//   `
// })
// list.innerHTML = content.join('')

```



## 综合案例

#### 筛选数据 filter 方法（重点）

* filter() 方法创建一个新的数组，新数组中的元素通过检查指定数组中符合条件的所有数组元素

* **主要使用场景：筛选数组符合条件的值，并返回筛选过后的新数组**

* **语法：**

  ```javascript
  要被筛选的数组.filter(function(currentValue, index){
      // 筛选条件返回
      return condition(currentValue)
  })
  要被筛选的数组.filter((currentValue, index)=>condition(currentValue))

* **返回值：**返回一个包含所有符合条件的数组。如果没有符合的元素返回空数组

* **参数：**currentValue必须写，index可选

* **例子：**

  ```javascript
  const arr = [10, 20, 30]
  // const newArr = arr.filter(function(item, index){
  //   // console.log(item)
  //   // console.log(index)
  //   return item >= 20
  // })
  // console.log(newArr)
  
  
  const newArr = arr.filter(item => item >= 20)
  console.log(newArr)
  ```
* **对Infinity的理解，表示正无穷，前面添加-号，表示负无穷**
    
    ```javascript
    // 2. 初始化数据
    const goodsList = [
        {
            id: '4001172',
            name: '称心如意手摇咖啡磨豆机咖啡豆研磨机',
            price: '289.00',
            picture: 'https://yanxuan-item.nosdn.127.net/84a59ff9c58a77032564e61f716846d6.jpg',
        },
        {
            id: '4001594',
            name: '日式黑陶功夫茶组双侧把茶具礼盒装',
            price: '288.00',
            picture: 'https://yanxuan-item.nosdn.127.net/3346b7b92f9563c7a7e24c7ead883f18.jpg',
        },
        {
            id: '4001009',
            name: '竹制干泡茶盘正方形沥水茶台品茶盘',
            price: '109.00',
            picture: 'https://yanxuan-item.nosdn.127.net/2d942d6bc94f1e230763e1a5a3b379e1.png',
        },
        {
            id: '4001874',
            name: '古法温酒汝瓷酒具套装白酒杯莲花温酒器',
            price: '488.00',
            picture: 'https://yanxuan-item.nosdn.127.net/44e51622800e4fceb6bee8e616da85fd.png',
        },
        {
            id: '4001649',
            name: '大师监制龙泉青瓷茶叶罐',
            price: '139.00',
            picture: 'https://yanxuan-item.nosdn.127.net/4356c9fc150753775fe56b465314f1eb.png',
        },
        {
            id: '3997185',
            name: '与众不同的口感汝瓷白酒杯套组1壶4杯',
            price: '108.00',
            picture: 'https://yanxuan-item.nosdn.127.net/8e21c794dfd3a4e8573273ddae50bce2.jpg',
        },
        {
            id: '3997403',
            name: '手工吹制更厚实白酒杯壶套装6壶6杯',
            price: '99.00',
            picture: 'https://yanxuan-item.nosdn.127.net/af2371a65f60bce152a61fc22745ff3f.jpg',
        },
        {
            id: '3998274',
            name: '德国百年工艺高端水晶玻璃红酒杯2支装',
            price: '139.00',
            picture: 'https://yanxuan-item.nosdn.127.net/8896b897b3ec6639bbd1134d66b9715c.jpg',
        },
  ]
    
    function renderByRange(arr, min = -Infinity, max = Infinity) {
        let content = ''
        arr.forEach(item=>{
            // 数据解构
            const {name, price, picture:src} = item
          // console.log(price)
    
            // 筛选价格拼接字符串
            // if (price >= min && price < max) {
            //   content += `
            //     <div class="item">
            //       <img src="${src}" alt="">
            //       <p class="name">${name}</p>
            //       <p class="price">${price}</p>
            //     </div>
            //   `
            // }
            content += `
              <div class="item">
                <img src="${src}" alt="">
                <p class="name">${name}</p>
                <p class="price">${price}</p>
              </div>
            `
        })
        // console.log(content)
        document.querySelector('.list').innerHTML = content
  }
    
    // 页面一打开就渲染数据
  renderByRange(goodsList)
    
    // 事件委托监听点击的是哪个范围
    document.querySelector('.filter').addEventListener('click', (e)=>{
        // e.target.tagName  e.target.dataset.index
        const {tagName, dataset:{index:idx}} = e.target
        if (tagName === 'A') {
            // console.dir(e.target)
            // console.log(e.target.dataset.index)
            // console.log('点击了')
    
            // const idx = e.target.dataset.index  // 注意返回的是字符串
            // console.log(idx)
            // 根据data-index计算价格最小值和最大值 [min, max)
            let min, max
            switch (idx) {
                case '1':
                    min = 0
                    max = 100
                    break 
                case '2':
                    min = 100
                    max = 300
                    break
                case '3':
                    min = 300
                    // Infinity 表示正无穷大
                    max = Infinity
                    break
                case undefined:
                    min = -Infinity
                    max = Infinity
            }
            // console.log("min: " + min)
            // console.log("max: " + max)
            const arr = goodsList.filter(item=>item.price > min && item.price <= max)
            // 根据价格范围渲染数据
            // renderByRange(arr, min, max)
            renderByRange(arr)
        }
    })
    ```



