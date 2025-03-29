# js-day03

## 函数

### 有参函数

```javascript
// 求start到end的值
    function getSum(start, end){
      let s = 0
      for (let i = start; i <= end; ++i){
        s += i
      }
      console.log(s)
    }
    getSum(50, 100)
```



### 默认参数

```javascript
function getSum(x = 0, y = 0){
      // 函数不设置默认参数。那么默认值是undefined
      // console.log(x, y)
      document.write(x + y)
    }
    getSum(1, 2)
    getSum()

    // 求start到end的值
    function getSum(start=0, end=0){
      let s = 0
      for (let i = start; i <= end; ++i){
        s += i
      }
      console.log(s)
    }
    getSum(50, 100)
```



### 有返回值的函数

```javascript
// 函数的返回值
function fn(){
    return 20
}
console.log(fn())  // fn() =20

// 求和
function add(x = 0, y = 0){
    return x + y
    // return后面的语句永远也不会执行
    console.log('hello World')
}
let sum = add(3, 4)
console.log(sum)
console.log(add(100, 300))

// 最大公约数 gcd
function gcd(a = 0, b = 0){
    return !b ? a : gcd(b, a % b)
}
let a = +prompt('输入一个数')
let b = +prompt('输入一个数')
console.log(`${a}和${b}的最大公约数为${gcd(a, b)}`)
```

