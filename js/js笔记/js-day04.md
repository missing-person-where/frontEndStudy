# js-day04

## 对象

### 对象的认识

* 对象是一种**数据类型**
* **无序**的数据的集合

### 对象的特点

* 无序的数据的集合
* 可以详细的描述某个事物

## 对象的使用

### 对象的**声明**

```javascript
let 对象名 = {}  // 常用

let 对象名 = new Object()
```

对象由**属性**和**方法**组成

* 属性：信息或叫特征（名词）如手机尺寸，颜色等
* 方法：功能或叫行为（动词）如手机打电话，发短信，玩游戏等

```javascript
let 对象名 = {
    属性名: 属性值,
    方法名: 函数
}
```

```javascript
// 1.声明对象
let pink = {
    uname: 'pink老师',
    age: 18,
    gender: '女',
}

console.log(pink)
console.log(typeof pink) // object
```

### **对象的增删改查**

```javascript

```

#### **查(对象名.属性)**

```javascript
// 声明
let product = {
    goods: `小米 (MI)`,
    name: '小米10 青春版',
    num: '10012816204',
    weight: '0.55kg',
    address:'中国大陆'
}

console.log(product.address)
console.log(product.weight)
```

#### **改(对象名.属性 = 新值)**

```javascript
let pink = {
    uname: 'pink老师',
    age: 18,
    gender: '女',
}
console.log(pink.gender)  // 女
// 改
pink.gender = '男'
console.log(pink.gender)  // 男
```

#### **增(对象名 .新属性 = 值)**

```javascript
let pink = {
    uname: 'pink老师',
    age: 18,
    gender: '女',
}
// 增
pink.hobby = '足球'
console.log(pink)  // {uname: 'pink老师', age: 18, gender: '女', hobby: '足球'}
```

#### **删(了解 **delete 对象名.属性**)**

```javascript
let pink = {
    uname: 'pink老师',
    age: 18,
    gender: '女',
}
console.log(pink.gender)  // 女
pink.gender = '男'
console.log(pink.gender)  // 男
pink.hobby = '足球'
console.log(pink)  // {uname: 'pink老师', age: 18, gender: '男', hobby: '足球'}
delete pink.age
console.log(pink) // {uname: 'pink老师', gender: '男', hobby: '足球'}
```

#### **查的另外一种方式**

对象名['属性']

```javascript
let product = {
    goods: `小米 (MI)`,
    'goods-name': '小米10 青春版',
    num: '10012816204',
    weight: '0.55kg',
    address:'中国大陆'
}
product.price = '3999元'
console.log(product)

console.log(product.goods)
console.log(product.num)
console.log(product.price)

// 查的另外一种方式 对象名['属性名']
console.log(product['goods-name'])
console.log(product['address'])
```

#### **对象中的方法**

**方法同样支持增删改查**

**方法调用：对象名.方法名**

```javascript
let obj = {
    uname: '刘德华',
    // 方法
    sing: function(x, y) {
        // console.log('冰雨')
        console.log(x, y)
    }
}
// 方法调用 对象名.方法名
obj.sing(1, 2)
console.log(obj.sing(1, 2))  // undefined

// document.write()	
```

#### **遍历对象**

```javascript
// for in 不推荐遍历数组
// let arr = ['pink', 'red', 'blue']
// for (let k  in arr){
//   console.log(k) // 数组的下标，索引号（字符串类型）
//   console.log(arr[k])  // arr[k]
// }  

// 1. 遍历对象 for in
let obj = {
    uname: '蔡徐坤',
    age: 16,
    gender: '男'
}
for (let k in obj){
    console.log(k)  // 'uname' 'age' 'gender'
    console.log(obj[k])
}
```

#### **渲染表格-案例**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    table {
      width: 600px;
      text-align: center;
    }

    table, 
    th, 
    td {
      border: 1px solid #ccc;
      border-collapse: collapse;
    }

    caption {
      font-size: 18px;
      margin-bottom: 10px;
      font-weight: 700;
    }

    tr {
      height: 40px;
      cursor: pointer;
    }

    table tr:nth-child(1) {
      background-color: #ddd;
    }

    table tr:not(:first-child):hover {
      background-color: #eee;
    }
  </style>
</head>
<body>
  <h2>学生信息</h2>
  <p>将数据渲染到表格中</p>

  <table>
    <caption>学生列表</caption>
    <tr>
      <th>序号</th>
      <th>姓名</th>
      <th>年龄</th>
      <th>性别</th>
      <th>家乡</th>
    </tr>
    <!-- script写在这里 -->
    <script>
      let students =[
        {name:'小明',age: 18, gender: '男', hometown: '河北省'},
        {name: '小红', age: 19, gender: '女', hometown: '河南省'},
        {name:'小刚',age: 17, gender: '男', hometown:'山西省'},
        {name: '小丽', age:18, gender: '女',hometown:'山东省'},
      ]
      for (let i = 0; i < students.length; ++i){
        document.write(`
          <tr>
            <td>${i+1}</td>
            <td>${students[i].name}</td>
            <td>${students[i].age}</td>
            <td>${students[i].gender}</td>
            <td>${students[i].hometown}</td>
          </tr>
        `)
      }
    </script>
  </table>

</body>
</html>
```

### 认识null

**null 类似于 let obj = {}，都是对象，将来要存放对象可以设置默认值为null**

**但是它们是不相等的**

## 内置对象

### Math

**常用方法**

| 方法   | 作用               |
| ------ | ------------------ |
| random | 生成[0, 1)的随机数 |
| ceil   | 向上取整           |
| floor  | 向下取整           |
| round  | 四舍五入           |
| max    | 找最大值           |
| min    | 找最小值           |
| pow    | 幂运算             |
| abs    | 绝对值             |

更多其它方法参考`mdn`文档

[Math对象在线文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math)

演示部分函数的使用

```javascript
// 属性
console.log(Math.PI) // 3.141592653589793
// 方法
// ceil 天花板 向上取整
console.log(Math.ceil(1.1)) // 2
console.log(Math.ceil(1.9)) // 2
console.log(Math.ceil(1.5)) // 2
// floor 地板
console.log(Math.floor(1.1)) // 1
console.log(Math.floor(1.9)) // 1
console.log(Math.floor(1.5)) // 1
// 四舍五入（round）
console.log(Math.round(1.1)) // 1
console.log(Math.round(1.5)) // 2
console.log(Math.round(1.9)) // 2
console.log(Math.round(-1.1)) // -1
console.log(Math.round(-1.5)) // -1
console.log(Math.round(-1.9))  // -2

// 取整函数 parseInt(1.2)  // 1
// 取整函数 parseInt('12px') // 12

console.log(Math.max(1, 2, 3, 4, 5))
console.log(Math.min(1, 2, 3, 4, 5))
console.log(Math.abs(-1))
console.log(Math.pow(6, 7))

console.log(Math.random())
```

### 随机数

```javascript
console.log(Math.random())
    
// 0-10
console.log(parseInt(Math.random() * 10) + 1)
let arr = ['red', 'green', 'blue']
let r = arr[parseInt(arr.length * Math.random())]
console.log(r)


// 5-10
console.log(Math.floor(Math.random() * (5 + 1)) + 5)

// N - M
// 0 M - N
// Math.floor(Math.random() * (M - N + 1)) + N
// [0, M - N] + N ==> [N, M]

function getRandInt(N, M){
    return Math.floor(Math.random() * (M - N + 1)) + N
}

console.log(getRandInt(4, 8))
```

### 随机点名案例

````javascript
let names = ['赵云','黄忠','关羽','张飞','马超','刘备','曹操']
// 1. 得到一个随机数 names数组索引 取值范围是 [0, names.length - 1]
function getRandInt(N, M){
    return parseInt(Math.random() * (M - N + 1)) + N
}
let idx = getRandInt(0, names.length - 1)
document.write(names[idx])

// splice(idx, n) idx:索引 n: 删除个数
names.splice(idx, 1)
console.log(names)
````

### 随机颜色案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    div {
      margin: 50px auto;
      background-color: pink;
      width: 400px;
      height: 400px;
    }
  </style>
</head>
<body>

  <div></div>
  
  <script>
    function getRandInt(N, M){
      return parseInt(Math.random() * (M - N + 1)) + N
    }

    function getRandomColor(flag){
      let color = '';
      if (flag){
        color += '#'
        let available_characters = ['0','1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f']
        for (let i = 0; i < 6; ++i){
          let r = getRandInt(0, available_characters.length - 1)
          color += available_characters[r]
        }
      }else {
        color += 'rgb('
        for (let i = 0; i < 3; ++i){
          let r = getRandInt(0, 255)
          color += r
          if (i != 2){
            color += ','
          }
        }
        color += ')'
      }
      return color
    }

    console.log(getRandomColor(true))
    console.log(getRandomColor(false))
    console.log(getRandomColor())

    let c = getRandomColor(false)
    console.log('c: ' + c)
    
    let div = document.getElementsByTagName('div')[0]
    div.style.backgroundColor = c
    
  </script>
</body>
</html>
```

### 渲染学成在线

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>学成在线</title>
    <link rel="stylesheet" href="./css/base.css">
    <link rel="stylesheet" href="./css/index.css">
</head>
<body>


    <!-- 精品推荐课程 -->
    <div class="course wrapper">
        <!-- 标题 -->
        <div class="hd">
            <h3>精品推荐</h3>
            <a href="#" class="more">查看全部</a>
        </div>
        <!-- 内容 -->
        <div class="bd">
            <ul>
                <script>
                  let data = [
                    {
                      src:'uploads/class1.png',
                      title:'ThinkPHP5.0博客系统实战项目演练',
                      num:1125
                    },
                    {
                      src:'uploads/class2.png',
                      title:'Android网络动态图片加载实战',
                      num:1357
                    },
                    {
                      src:'uploads/class3.png',
                      title:'Angular2大前端商城实战项目演练',
                      num:22250
                    },
                    {
                      src:'uploads/class4.png',
                      title:'Angular2大前端商城实战项目演练',
                      num:22250
                    },
                    {
                      src:'uploads/class3.png',
                      title:'Angular2大前端商城实战项目演练',
                      num:22250
                    },
                    {
                      src:'uploads/class4.png',
                      title:'Angular2大前端商城实战项目演练',
                      num:22250
                    },
                    {
                      src:'uploads/class2.png',
                      title:'Android网络动态图片加载实战',
                      num:1357
                    },
                  ]
                  for (let i = 0; i < data.length; ++i){
                    document.write(`
                      <li>
                        <a href="#">
                            <div class="pic"><img src="${data[i].src}" alt=""></div>
                            <div class="text">
                                <h4>${data[i].title}</h4>
                                <p><span>高级</span> · <i>${data[i].num}</i>人在学习</p>
                            </div>
                        </a>
                      </li>
                    `)
                  }
               </script>
            </ul>
        </div>
    </div>

</body>
</html>
```

```css
// css/base.css
/* 基础公共样式，清除默认样式+设置通用样式 */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

li {
    list-style: none;
}

body {
    font: 14px/1.5 "Microsoft Yahei", "Hiragino Sans GB", "Heiti SC", "wenQuanYi Micro Hei", sans-seif;
    color: #333;
}

a {
    color: #333;
    text-decoration: none;
}


// css/index.css
/* 首页css样式 */
/* 版心 */
.wrapper {
    margin: 0 auto;
    width: 1200px;
}

body {
    background-color: #f3f5f7;
}


/* 推荐课程 */
.course {
    margin-top: 15px;
}

/* 标题：公共类，与其它区域公用 */
.hd {
    display: flex;
    justify-content: space-between;
    height: 60px;
    line-height: 60px;
}

.hd h3 {
    font-size: 21px;
    font-weight: 400;
}

.hd .more {
    padding-right: 20px;
    background: url(../images/more.png) no-repeat right center;
    font-size: 14px;
    color: #999;
}

/* 课程内容，公共类 */
.bd ul {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
}

.bd li {
    margin-bottom: 14px;
    width: 228px;
    height: 271px;
    background-color: pink;
}

.bd li .pic {
    height: 156px;
}

.bd li .text {
    padding: 20px;
    height: 115px;
    background-color: #fff;
}

.bd li .text h4 {
    margin-bottom: 13px;
    height: 40px;
    font-size: 14px;
    line-height: 20px;
    font-weight: 400;
}

.bd li .text p {
    font-size: 14px;
    line-height: 20px;
    color: #999;
}

.bd li .text p span {
    color: #fa6400;
}

.bd li .text p i {
    font-style: normal;
}

.hd ul {
    display: flex;
}

.hd li {
    margin-right: 60px;
    font-size: 16px;
}

.hd li .active {
    color: #00a4ff;
}

.bd {
    display: flex;
    justify-content: space-between;
}

.bd .left {
    width: 228px;
    /* background-color: pink; */
}

.bd .right {
    width: 957px;
    /* background-color: pink; */
}

.bd .right .top {
    margin-bottom: 15px;
    height: 100px;
}

```

## 扩展-数据类型存储的认识

### 基本数据类型

* 简单的数据类型在储存中变量中存储的是**值**，因此叫做**值类型** 
  * string，number，boolean，null，undefined
* **简单数据类型**的**值**一般存储到**栈**中

### 引用类型

* 复杂的数据类型存储的是**地址**（引用），因此叫做**引用类型**
  * 通过**new** 关键字创建的对象（系统对象，自定义对象），如Object，Array，Date等
* **复杂数据类型**的**值**一般存储到**堆**中，但是变量存储的**地址**还是在**栈**中

**简易图解**

![image-20250330123402517](C:\Users\yhx\AppData\Roaming\Typora\typora-user-images\image-20250330123402517.png)
