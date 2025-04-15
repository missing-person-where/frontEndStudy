# JS进阶-day03

## 编程思想

### **面向过程**

* **定义：**按步骤完成某个功能

* **优点：**性能比面向对象高，适合和硬件相关的东西

* **缺点：**没有面向对象易维护，易复用，易扩展

### **面向对象**

* **定义：**把事务分解成为一个个对象，然后由对象之间分工和合作

* **面向对象是以对象功能划分问题，而不是步骤**

* **优点：**灵活，代码可复用，容易维护和开发，适合多人开发大项目，低耦合（解耦合）
* **缺点：**性能比面向过程低

* **特性：**
  * 封装性
  * 继承性
  * 多态性

## 构造函数

构造函数**存在浪费内存的问题**

```javascript
// 构造函数  公共的属性和方法 封装到了Star构造函数
function Star(uname, age) {
    this.uname = uname
    this.age = age
    this.sing = function() {
        console.log('唱歌')
    }
}
const ldh = new Star('刘德华', 55)
const zxy = new Star('张学友', 58)
console.log(ldh === zxy)  // false
console.log(ldh.sing === zxy.sing)  // false
```

![image-20250414201245275](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250414201245275.png)

**节省内存？**

## 原型

**解决构造函数浪费内存的问题**

### 定义

* 构造函数是通过原型分配的函数时所有对象所**共享的**。

* JavaScript规定，**每一个构造函数都有一个prototype属性**，指向另一个对象，所以我们也称为原型对象

* 这个对象可以挂载函数，对象实例化不会多次创建原型上函数，节约内存

* **我们可以把那些不变的方法，直接定义在prototype上，这样所有对象的实例就可以共享这些方法**

  ```javascript
  // 构造函数  公共的属性和方法 封装到了Star构造函数
  // 1. 公共的属性写在构造函数里面
  function Star(uname, age) {
  this.uname = uname
  this.age = age
  // this.sing = function() {
  //   console.log('唱歌')
  // }
  }
  // 2. 公共的方法写在原型对象内部
  Star.prototype.sing = function() {
  console.log('唱歌')
  }
  const ldh = new Star('刘德华', 55)
  const zxy = new Star('张学友', 58)
  ldh.sing()  // 唱歌
  zxy.sing()  // 唱歌
  // console.log(ldh === zxy)  // false
  console.log(ldh.sing === zxy.sing)  // true
  ```

* **构造函数和原型对象中的this 都指向 实例化的对象**

  ```java
  let that
  function Star(uname) {
      // that = this
      // console.log(this)
      this.uname = uname
  }
  // 原型对象的this指向实例化对象
  Star.prototype.sing = function() {
      that = this
          console.log('sing')
          // console.log(this)
  }
  // 实例对象 ldh
  const ldh = new Star('刘德华')
  ldh.sing()
  console.log(that === ldh)  // true    
  ```

* **例子-给数组扩展方法**

  ```javascript
  // 给数组扩展求最大值和求和方法
  // 1. 我们定义的方法，任何一个数组实例都可以使用
  // 2. 自定义的方法 写道 数组.prototype 身上
  // 1. 求最大值
  // const arr = [1, 2, 3]
  const arr = new Array(1, 2, 3)
  // console.log(arr)
  Array.prototype.max = function() {
      // console.log(this)  // [1, 2, 3]
      // 数组展开
      return Math.max(...this)
      // 原型函数指向实例对象 arr
  }
  
  Array.prototype.min = function() {
      // console.log(this)  // [1, 2, 3]
      // 数组展开
      return Math.min(...this)
      // 原型函数指向实例对象 arr
  }
  console.log(arr.max())  //  3
  console.log([2, 5, 9].max())  // 9
  console.log(arr.min())  // 1
  
  Array.prototype.sum = function() {
      return this.reduce((pre, cur)=>pre + cur)
  }
  console.log(arr.sum())  // 9
  console.log([1.2, 2.4, 1.4].sum()) // 5
  ```

### constructor属性

* **在哪里？：**每个原型对象都有constructor属性

* **作用：**该属性**指向**该原型对象的**构造函数**

* **使用场景：**如果有多个对象的实例方法，可以通过原型对象形式赋值，但是要让constructor重新指向原来创造原型对象的构造函数

  ```javascript
  // constructot   构造函数
  function Star() {
  
  }
  // Star.prototype.sing = function() {
  //   console.log('唱歌')
  // }
  // Star.prototype.dance = function() {
  //   console.log('跳舞')
  // }
  console.log(Star.prototype)
  Star.prototype = {
      // 重新指回原来创造原型对象的构造函数
      constructor: Star,
      sing: function() {
          console.log('唱歌')
      }, 
      dance: function() {
          console.log('跳舞')
      }, 
  }
  console.log(Star.prototype)
  
  // const ldh = new Star()
  // console.log(Star.prototyper)
  // console.log(Star.prototype.constructor === Star)  // true
  ```

### 对象原型

**对象都有一个属性`__proto__`**指向构造函数prototype 原型对象，之所以对象可以使用构造函数 prototype原型对象的属性和方法，就是因为有`__proto__`原型的存在

注意:

* `__proto_`是`JS`非标准属性，**只读，不可修改**
* `[[prototype]]`和`__proto__`意义相同
* 用来表明当前实例对象指向哪个原型对象prototype
* `__proto__`对象原型里面也有一个constructor属性，**指向创建该实例对象的构造函数**

![image-20250414213002194](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250414213002194.png)

**理解对象构造函数-原型对象-对象原型的关系**

```javascript
function Person() {
    this.name = name
}
const peppa = new Person('佩奇')
// 原型对象与对象原型互相指向
console.log(Person.prototype) // 原型对象
console.log(peppa.__proto__) // 对象原型
console.log(Person.prototype === peppa.__proto__)
// 原型对象和对象原型的constructor属性都指向构造函数
console.log(Person.prototype.constructor)
console.log(peppa.__proto__.constructor)
console.log(Person.prototype.constructor === Person)
console.log(peppa.__proto__.constructor === Person)
```

### 原型继承

`JS`中大多是借助**原型对象**实现**继承**的特性

```javascript
// 继续抽取 封装  公共的部分放在原型上
// const Person1 = {
//   ears: 2, 
//   head: 1
// }
// const Person2 = {
//   ears: 2, 
//   head: 1
// }
// 构造函数  new出来的对象 结构一样，但是对象不一样
function Person() {
    this.ears = 2
    this.head = 1
}
// 女人  构造函数  继承 Person
function Woman() {
}
// Woman通过原型继承Person
// Woman.prototype = Person1
Woman.prototype = new Person()
// console.log(new Person())

// 指会原来的构造函数
Woman.prototype.constuctor = Woman

// 给女人添加一个方法 生孩子
Woman.prototype.baby = function() {
    console.log('baby')
}
const red = new Woman()
console.log(red)
console.log(Woman.prototype)


// 男人  构造函数  继承 Person
function Man() {
}
// Man.prototype = Person2
Man.prototype = new Person()
Man.prototype.constructor = Man
const pink = new Man()
console.log(pink)
console.log(Man.prototype)
```



### 原型链以及`instancof`

基于原型对象的继承使得不同构造函数的原型对象关联在一起，并且这种关联的关系是一种链状的结构，我们将原型对
象的链状结构关系称为原型链 

![image-20250415131825323](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250415131825323.png)

只要有对象实例，就有对象原型，对象原型指向原型对象，只要是对象就有原型对象，原型对象constructor指向相应的构造函数

只要是对象就有`__proto__`，只要有原型`prototype`就有`constructor`指向对应的构造函数

#### 查找规则

1. 当访问一个对象的属性（或方法）时，首先查找这个**对象自身**有没有该属性
2. 如果没有就查找它的原型（也就是 `__proto__`指向的**prototype 原型对象**）
3. 如果还没有就查找原型对象的原型（**Object的原型对象**）
4. 依次类推找到 Object为止（**null**）
5. `__proto`对象原型的意义在于为对象成员查找机制提供一个方向，或者说是一条路线
6. 可以使用 `instanceof` 运算符 用于检测构造函数的`prototype` 属性是否出现在某个实例对象的原型链上

## 综合案例

**模态框封装**

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>面向对象封装消息提示</title>
  <style>
    .modal {
      width: 300px;
      min-height: 100px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
      border-radius: 4px;
      position: fixed;
      z-index: 999;
      left: 50%;
      top: 50%;
      transform: translate3d(-50%, -50%, 0);
      background-color: #fff;
    }

    .modal .header {
      line-height: 40px;
      padding: 0 10px;
      position: relative;
      font-size: 20px;
    }

    .modal .header i {
      font-style: normal;
      color: #999;
      position: absolute;
      right: 15px;
      top: -2px;
      cursor: pointer;
    }

    .modal .body {
      text-align: center;
      padding: 10px;
    }

    .modal .footer {
      display: flex;
      justify-content: flex-end;
      padding: 10px;
    }

    .modal .footer a {
      padding: 3px 8px;
      background: #ccc;
      text-decoration: none;
      color: #fff;
      border-radius: 2px;
      margin-right: 10px;
      font-size: 14px;
    }

    .modal .footer a.submit {
      background-color: #369;
    }
  </style>
</head>

<body>
  <button id="delete">删除</button>
  <button id="login">登录</button>
  <button id="add">新增</button>

  <!-- <div class="modal">
    <div class="header">温馨提示 <i>x</i></div>
    <div class="body">您没有删除权限操作</div>
  </div> -->


  <script>
    // 1. Modal 构造函数封装 - 模态框
    function Modal(title = '', msg = '') {
      // 创建modal 模态框盒子
      // 1.1 创建div标签
      this.modalBox = document.createElement('div')
      // 1.2 给div添加类名modal
      this.modalBox.className = 'modal'
      // 1.3 modal盒子内部填充2个div标签并且修改文字内容
      this.modalBox.innerHTML = `
        <div class="header">${title} <i>x</i></div>
        <div class="body">${msg}</div>
      `
      console.log(this.modalBox)
    }
    // new Modal('温馨提示', '你没有权限删除')
    // new Modal('友情提示', '你还没有登录呢')

    // 2. 给构造函数原型对象挂载open打开方法
    Modal.prototype.open = function() {
      // 先判断页面是否有modal盒子，如果有就删除，否则添加
      const box = document.querySelector('.modal')
      box && box.remove()
      // 注意这个方法不要用箭头函数
      // 把刚才创建的modalBox显示到body中
      document.body.append(this.modalBox)

      // 等到盒子显示出来，就可以绑定事件了
      this.modalBox.querySelector('i').addEventListener('click', () => {
        // 这个地方需要用到箭头函数
        // 这个this指向实例对象
        this.close()
      })
    }

    // 3. 给构造函数原型对象挂载 close方法
    Modal.prototype.close = function() {
      this.modalBox.remove()
    }

    // 测试点击删除按钮
    document.querySelector('#delete').addEventListener('click', () => {
      // 先调用 Modal构造函数
      const del = new Modal('温馨提示', '你没有权限删除')
      // 实例对象调用 open 方法
      del.open()
    })

    // 测试点击登录按钮
    document.querySelector('#login').addEventListener('click', () => {
      // 先调用 Modal构造函数
      const login = new Modal('友情提示', '你还没有注册呢')
      // 实例对象调用 open 方法
      login.open()
    })

    // 测试点击登录按钮
    document.querySelector('#add').addEventListener('click', () => {
      // 先调用 Modal构造函数
      const login = new Modal('强烈的提示', '你没有新增权限')
      // 实例对象调用 open 方法
      login.open()
    })
  </script>
</body>

</html>
```

