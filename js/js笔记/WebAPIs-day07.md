# WebAPIs-day07

## 实战-放大镜效果

### 1.根据鼠标悬停到的小图切换对应中图

1. 通过**事件委托（事件捕获）**的方式 **`mouseenter`**

   注意：**`mouseenter`没有事件冒泡**

   ```javascript
   // 获取元素
   // 大盒子
   const large = document.querySelector('.large')
   // 中盒子
   const middle = document.querySelector('.middle')
   // 小盒子
   const small = document.querySelector('.small')
   // 1. 根据鼠标悬浮到的小图切换对应中图
   // 1.1 事件委托监听
   // 事件捕获方式 
   small.addEventListener('mouseenter', function(e){
       // console.log(e.target.tagName)
       if (e.target.tagName === 'LI') {
           // console.log(111)
           console.log(e.target.children[0].src)
           // 切换active
           this.querySelector('li.active').classList.remove('active')
           e.target.classList.add('active')
           // 根据图片地址切换中等图片
           middle.querySelector('img').src = e.target.children[0].src
   
           // 给大盒子切换背景图片
           large.style.backgroundImage = `url(${e.target.src})`
       }
   }, true)		
   ```

2. 通过**事件委托（事件冒泡）**的方式 **`mouseover`**

   ```javascript
   // 事件冒泡方式
   // 由于mouseenter没有冒泡 可以使用mouseover 
   small.addEventListener('mouseover', function (e) {
       // console.log(e.target.tagName)
       if (e.target.tagName === 'IMG') {
           // console.log(111)
           console.log(e.target.src)
           // 切换active
           e.target.parentNode.classList.remove('active')
           e.target.classList.add('active')
           // 根据图片地址切换中等图片
           middle.querySelector('img').src = e.target.src
   
           // 给大盒子切换背景图片
           large.style.backgroundImage = `url(${e.target.src})`
       }
   })
   ```

### 2.鼠标经过离开中图，需要显示隐藏大盒子（对应大图）

* 离开延时200ms（通过setTimeout()）隐藏大盒子

* 注意：显示大盒子操作之前，需要清除之前设置的定时器

  ```javascript
  // 2. 鼠标经过中等盒子显示大盒子，离开200ms后隐藏大盒子
  middle.addEventListener('mouseenter', show)
  middle.addEventListener('mouseleave', hide)
  let timeId = 0
  
  function show() {
      // 显示前去除隐藏的定时器
      clearTimeout(timeId)
      large.style.display = 'block'
  }
  
  function hide() {
      timeId = setTimeout(function () {
          large.style.display = 'none'
      }, 200)
  }
  ```

### 3.鼠标经过大盒子，大盒子继续显示，离开隐藏大盒子

**原理同上**

```javascript
// 3. 鼠标经过大盒子显示大盒子，离开200ms后隐藏大盒子
large.addEventListener('mouseenter', show)
large.addEventListener('mouseleave', hide)
```



### 4.鼠标经过 离开中等盒子，显示 隐藏遮罩层

  ```javascript
  const layer = document.querySelector('.layer')
  
  middle.addEventListener('mouseenter', function () {
  
      layer.style.display = 'block'
  
  })
  
  
  
  middle.addEventListener('mouseleave', function () {
  
      layer.style.display = 'none'
  
  })
  ```



### 5.根据鼠标坐标移动黑色遮罩盒子并切换对应大盒子背景图显示位置

```javascript
// 5. 根据鼠标坐标移动黑色遮罩盒子并切换对应大盒子背景图显示位置
middle.addEventListener('mousemove', function (e) {
    // console.log(e.pageX, e.pageY)   // 鼠标在页面中的坐标
    // 获取 鼠标在中等盒子中的坐标 = 鼠标在页面中的坐标 - middle在页面的坐标  
    // console.log(large.getBoundingClientRect())

    let x = e.pageX - middle.getBoundingClientRect().left
    let y = e.pageY - middle.getBoundingClientRect().top - document.documentElement.scrollTop
    // console.log(x, y)

    // 更新位置
    // 黑色遮罩层应该在middle盒子内部
    if (x >= 0 && x <= 400 && y >= 0 && y <= 400) {
        // 黑色盒子不是一直移动
        let mx = 0, my = 0
        if (x < 100) mx = 0
        else if (x >= 100 && x <= 300) mx = x - 100
        else mx = 200
        layer.style.left = mx + 'px'  // 一定要加单位，否则无效

        if (y < 100) my = 0
        else if (y >= 100 && y <= 300) my = y - 100
        else my = 200
        layer.style.top = my + 'px'  // 一定要加单位，否则无效

        // 移动大盒子背景图片  中等盒子移动  的2倍
        large.style.backgroundPositionX = -2 * mx + 'px'
        large.style.backgroundPositionY = -2 * my + 'px'
    }
})
```



