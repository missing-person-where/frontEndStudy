# JS进阶-day02

## 深入对象

### 创建对象的三种方式

**1.利用对象字面量创建**

```javascript
const obj = {uname: 'admin'}
console.log(obj)  // {uname:'admin'}
```

**2.利用new Object()创建对象**

```javascript
// const obj = new Object()
// obj.uname = 'admin'
// console.log(obj)  // {uname:'admin'}

const obj = new Object({uname: 'admin'})
console.log(obj)  // {uname:'admin'}
```

**3.利用构造函数创建对象**

### 构造函数

* **是什么：**是一种特殊的函数，主要用来初始化对象
* **使用场景：**常规的{...}语法允许创建一个对象。比如我们创建了佩奇的对象，继续创建乔治的对象还需要重新写一遍，此时可以通过**构造函数**来**快速创建多个类似的对象**。
* **约定：**
  1. 命名首字母大写
  2. 只能通过new操作符执行这个函数

```javascript
 // 1. 创建一个猪的构造函数
function Pig(uname, age) {
    this.uname = uname
    this.age = age
    // 默认返回值是一个对象，不用写返回值，写返回值无效
}
const pig = new Pig('佩奇', 18)
// const pig = {uname: '佩奇', age: 18}
console.log(pig)
console.log(new Pig('乔治', 7))
```

**构造函数实例化过程：**

1. 创建新的空对象

2. 构造函数this指向新对象
3. 执行构造函数代码，修改this，添加新属性
4. 返回新对象

### 实例成员&静态成员

实例成员：

通过构造函数创建的对象称为实例对象，实例对象的属性和方法称为实例成员（实例属性和实例方法）

说明：

1. 为构造函数传入参数，创建解构相同但是值**不同的对象**
2. 构造函数创建的实例对象**彼此独立**互不影响

```javascript
function Pig(name) {
    this.name = name
}
const peiqi = new Pig('佩奇')
const qiaozi = new Pig('乔治')
peiqi.name = '小猪佩奇'  // 实例属性
peiqi.sayHi = ()=>{   // 实例方法
    console.log('hi~');
}
console.log(peiqi)
console.log(qiaozi)
console.log(peiqi === qiaozi)  // false

```



**静态成员：**

**构造函数**的属性和方法被称为静态成员（**静态成员和静态方法**）

说明：

1. 静态成员只能通过构造函数访问
2. 静态方法中的this指向构造函数

比如 `Date.now() ` ` Math.PI`  `Math.random()`

```javascript
function Pig(name) {
    this.name = name
}
Pig.eyes = 2  // 静态属性
Pig.sayHi = function() {   // 静态方法
    console.log(this)
}
console.log(Pig.eyes)  // 2
Pig.sayHi()   
```



## 内置构造函数

其实字符串，数值，布尔等基本数据类型都有专门的构造函数，这些被称为包装类型。JS中几乎所有的数据都可以基于构造函数创建

```javascript
// const str = 'pink'
// console.log(str.length)  // 4
// const num = 12
// console.log(num.toFixed(2))  // 12.00
// const str = 'pink'
// js底层完成的，把简单数据类型包装成了引用数据类型
// const str = new String('pink')
```

**引用类型**

* Object，Array，RegExp，Date等

**包装类型**

* String，Number，Boolean等

### Object

* 作用：Object.keys 静态方法获取对象中所有属性（键）

* 语法：

  ```javascript
  const o = { uname: 'pink', age: 18 }
  // 1. 获得所有属性名
  console.log(Object.keys(o))  // ['uname', 'age']
  // 2. 获得所有属性值
  console.log(Object.values(o))  // ['pink', 18]
  ```

  

* 返回值：一个数组

### Array

#### 数组常见方法-其他方法

   `forEach map reduce filter find every`

1. 实例方法`join`数组元素**拼接为字符串**，**返回字符串(重点)**

2. 实例方法`find`查找元素，返回**符合测试条件**的**第一个数组元素值**，如果**没有符合条件**的则返回**undefined**(重点)

   ```javascript
   const arr = ['red', 'blue', 'green']
   const re = arr.find(function(element) {
       return element === 'blue'
   })
   console.log(re) // red
   
   const data = [
       {
           name: '小米',
           price: 1999
       },
       {
           name: '华为',
           price: 3999
       },
   ]
   // 筛选小米的品牌 filter过滤
   const mi = data.find(ele=>{
       console.log(111)  // 只会循环一次
       return ele.name === '小米'
   })
   console.log(mi)  // {name: '小米', price: 1999}
   ```

3. 实例方法`every`检测数组所有元素是否**都符合指定条件**，如果所有元素**都通过**检测返回**true**，**否则**返回**false**(重点)

   ```javascript
   const arr = [0, 0, 0, 1]
   const ret = arr.every(ele => ele === 0)
   console.log(ret)  // false
   console.log([10, 20, 30].every(ele => ele >= 10))  // true
   ```

4. 实例方法`some`检测数组中的元素是否满足指定条件如果数组中**有元素满足条件**返回**true**，否则返回**false**

   ```javascript
   console.log([10, 20, 30].some(ele => ele >= 20))  // true
   ```

5. 实例方法`concat`**合并**两个数组，**返回生成新数组**

6. 实例方法`sort`对原数组单元值**排序**

7. 实例方法`splice`**删除**或**替换**原数组单元

8. 实例方法`reverse`**反转数组**

9. 实例方法`findIndex`**查找元素的索引值**

**将伪数组转换成真数组**
静态方法 `Array.from()`

```html
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>

<script>
    const lis = document.querySelectorAll('ul li')
    // console.log(lis)
    // lis.pop()
    // 伪数组转换成真数组
    const liss = Array.from(lis)
    liss.pop()
    console.log(liss)
</script>
```

### String

#### 常见实例方法

1. `length()`

2. `split('分隔符')`   <=> `join('连接符')`

   ```javascript
   const str = 'red,blue,green'
   console.log(str.split(','))  // ['red', ' blue', ' green']
   
   const str1 = '2022-4-8'
   const arr1 = str1.split('-')
   console.log(arr1)  // ['2022', '4', '8']
   ```

3. `substring(需要截取的第一个字符的索引号[，结束的索引号])`

   ```javascript
   // 2. substring
   const str = 'hello World!'
   console.log(str.substring(6))  // World!
   console.log(str.substring(6, 11)) // World [6, 11) 左闭右开
   ```

4. `startsWith(检测字符串[, 检测位置索引号])`

   ```javascript
   console.log("helloworld".startsWith('hello'))  // true
   console.log("helloworld".startsWith('hi'))  // false
   console.log("helloworld".startsWith("hello", 2))  // false
   ```

5. `includes(搜索的字符串[, 检测位置索引号])`

   ```javascript
   // 4. includes
   console.log("helloworld".includes("owo"))  // true
   console.log("helloworld".includes("owo", 5))  // false
   ```

6. `toUpperCase()`

7. `toLowerCase()`

   ```javascript
   console.log("helloWOrlD".toUpperCase())  // HELLOWORLD
   console.log("helloWOrlD".toLowerCase())  // helloworld

8. `indexOf(某个字符串)`

   ```javascript
   console.log("hellohe".indexOf('he'))  // 0
   console.log("hellow".indexOf("owo")) // -1

9. `endsWith(某个字符)`

   ```javascript
   console.log("helloworld".endsWith('world'))  // true
   console.log("helloworld".endsWith('word'))  // false
   console.log("helloworld".endsWith("hello", 6))  // false
   ```

10. `replace(要替换替换字符串，要替换成为的字符串)`用于替换字符串，支持正则

    ```javascript
    console.log("helloworldhello".replace("hello", "hi"))  // hiworldhello
    const pt = /\d+/
    console.log("abc121abda0131afaj1312".replace(pt, "_"))
    console.log("abc121abda0131afaj1312".replace(/[a-z]+/g, "*"))  // *121*0131*1312
    ```

11. `match(字符串)`用于查找字符串，支持正则

    ```javascript
    // 8. match
    console.log("abedHelloworlDgth".match('[a-e]'))  // ['a', index: 0, input: 'abedhelloworldgth', groups: undefined]
    console.log("abedhelloworldgth".match(/[a-h]/gi))  //  ['a', 'b', 'e', 'd', 'h', 'e', 'd', 'g', 'h']
    ```

**转换为字符串**

```javascript
const num = 10
console.log(String(num))  // 字符串的10
console.log(num.toString())  // 字符串的10
```

### Number

常用方法：

`toFixed()` 设置保留小数位的长度

`Number()` 传数字

```javascript
// toFixed不传参 四舍五入
console.log(10.423.toFixed())  // 10
console.log(10.923.toFixed())  // 11

// 保留2位小数
console.log(10.923.toFixed(2))  // 10.92
const num1 = 10
// console.log(10.toFixed(2))
console.log(num1.toFixed(2))  // 10.00
```

