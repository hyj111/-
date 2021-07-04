# css面试题

## 1.标签语义化

- 什么是标签语义化

用最恰当的标签来标记内容，便于开发者阅读和写出更优雅的代码的同时让浏览器的爬虫和机器很好地解析。

- 为什么要标签语义化

1.有利于SEO，有利于搜索引擎爬虫更好的理解我们的网页，从而获取更多有效信息，提升网页权重。

2.便于团队开发和维护，语义化的HTML可以让开发者更容易看的明白，提高团队效率和协调能力。

- 都有哪些标签，都是啥意思

块级（display：block）：div 、p、h1~h6、hr、ul、ol、li、dl、dd、form、header、footer、main、nav、sector、arcitcle、pre、table、tbody、thead、th、tr、tfoot

行级（display：inline）：a、span、i、strong、em、small、code、

行内块（display：inline-block）：img、input

* 块级标签和行内标签和行内块标签的区别

块级标签：独自占领一行、可设置宽高；

行内标签：可以和别的行内标签在一行内显示、不可设置宽高；

行内块标签：既能在同一行显示，也能设置宽高

* 这三个标签如何转换

```css
display:block ; /*转为块级标签*/
display:inline; /*转为行内标签*/
display:inline-block; /*转为行内块标签*/
```

* display除了这几个值还有哪些
* `display:none`

  * 让元素隐藏，可以怎么做？

    ```css
    1.opacity:0; /*设置透明度为0占位*/
    2.visibility:hidden;/*占位*/
    3.display:none;/*不占位*/
    4.height:0;overflow:hidden; /*不占位*/
    5.position:absolute;left:-9999px;top:-9999px.;让元素隐藏，可以怎么做？
    ```

  * `display:none`和`visibility:hidden`,`opacity:0`的区别？

    display: none隐藏后的元素不占据任何空间

    visibility: hidden隐藏后的元素空间依旧保留

    display: none是非继承属性，子孙节点消失是因为元素从渲染树中消失造成的；visibility: hidden是继承属性，子孙节点消失由于继承了hidden，可通过visibility: visible让子孙节点现行。
    
    opacity:0 可以进行DOM事件监听。
    
    

## 2.盒子水平垂直居中

题目：我有一个大盒子，有一个小盒子，想让小盒子在大盒子中水平垂直居中。

方案一 .使用定位，需要知道小盒子的具体宽高

```css
 html,body{
            position: relative;
        }
        .box{
            width: 100px;
            height: 50px;
            position: absolute;
            top: 50%;
            left: 50%;
            margin-top: -25px; /*为box高度的一半*/
            margin-left: -50px; /*为box宽度的一半*/
        }
```

方案二 . 使用定位不需要知道小盒子的长度和宽度，但是它得有宽高

```css
	html,body{
            position: relative;
        }    
	.box{       
            position: absolute;
            top: 0;
            bottom: 0;
            left: 0;
            right: 0;
            margin: auto;
        } 
```

方案三 . 使用定位不需要知道小盒子的长度和宽度，可以靠内容撑开，利用css的方法

```css
  html,body{
            position: relative;
        } 
  .box{
            position: absolute;
            top: 50%;
            left: 50%;
        }
```

方案四 使用flex布局

```css
   body {
            display: flex;
            /* 沿着主轴垂直 */
            justify-content: center; 
            /* 沿着交叉轴垂直 */
            align-items: center;
        }
```

方案五 使用js

获取屏幕的宽和高，获取盒子的宽和高，当前屏幕的宽和高减去盒子的宽和高的一半，作为`top`和`left`值。

```js
  let HTML = document.documentElement
        let winW = HTML.clientWidth //获取屏幕的宽
        let winH = HTML.clientHeight//获取屏幕的高
        let box = document.querySelector(".box")   
        let boxW = box.offsetWidth //获取的是盒子最终的宽
        let boxH = box.offsetHeight //获取的是盒子最终的高
        box.style.position = "absolute"
        box.style.left = (winW-boxW) / 2 +'px';
        box.style.top = (winH-boxH) / 2 +'px';
```

方案 六  display: table-cell;

```css
      body {
            display: table-cell;
            vertical-align: middle;
            text-align: center;
            /* 父级必须有固定宽高才能实现 */
            height: 100vh;
            width: 100vw;
        }
        .box {
            display: inline-block;
        }
```

display: table-cell本身是控制文本的，所以要将块级元素转为行内块元素

## 3.css的盒模型

**标准盒模型`box-sizing:content-box`**

就是指width和height包含的是内容的宽和高，不代表盒子的宽和高。

盒子的宽高 =  内容的宽高+border(边框)+padding(内边距)

**怪异盒模型`box-sizing:border-box`**

width和height的大小就是指盒子的宽高

话术：其实我们最常用的就是标准盒模型，他的`width`和`height`指的是内容宽高而不是盒子的宽高，当我们设置了`width`和`height`，当我们加边框或者加内边距的时候就会引起盒子大小的变化，css3给我们提供了一种新的盒子模型，怪异盒模型他的`width`和`height`指的是盒子的宽高，不管我们怎么调边框和内边距，盒子大小都不会发生变化。

## 4.经典布局方案

## 圣杯布局

**方案一** 使用margin负值和定位解决

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<style>
    html,body {
        margin: 0;
        padding: 0;
        height: 100%;
        overflow: hidden;
    }
    .clearfix:after {
        content: "";
        display: block;
        visibility: hidden;
        clear: both;
    }
    .container {
        width: 800px;
        margin: auto;
        padding: 0 100px;
        background-color: pink;
    }
    .center {
        width: 100%;
        height: 200px;
        float: left;
        background-color: red;
    }
    .left,.right {
        float: left;
        width: 100px;
        height: 200px;
        background-color: blue;
    }
    .left {
        margin-left: -100%;
        position: relative;
        left: -100px;
    }
    .right {
        margin-left: -100px;
        position: relative;
        right: -100px;
    }
</style>
<body>
    <div class="container clearfix">
        <div class="center">中</div>
        <div class="left">左</div>
        <div class="right">右</div>
    </div>
</body>
</html>
```

**方案二** calc

```css
    .center {
        float: left;
        width: calc(100% - 400px);
        background-color: seagreen;
        height: 100px;
    }
```

**方案三** flex布局

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
    body{
        padding: 0;
        margin: 0;
    }
    html,body {
        height: 100%;
        overflow: hidden;
    }
    .container {
        display: flex;
    }
    .center {
        width: calc(100% - 400px);
        background-color: seagreen;
        height: 100px;
        flex: 1;  
        order: 2;     
    }
    .left,.right {      
        width: 200px;
        height: 100px;
        background-color: red;
    }
    .left {
        order: 1;
    }
    .right {
        order: 3;
    }
    </style>
</head>
<body>
    <div class="container">
        <div class="center">中</div>
        <div class="left">左</div>     
        <div class="right">右</div>
    </div>
</body>
</html>
```



## 双飞翼布局

**方案一**

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<style>
      html,body {
        margin: 0;
        padding: 0;
        height: 100%;
        overflow: hidden;
    }
    .clearfix:after {
        content: "";
        display: block;
        visibility: hidden;
        clear: both;
    }
    .container {      
        width: 100%;
        float: left;
        background-color: pink;
    }
    .center {
        margin: 0 100px;
        height: 300px;
    }
    .left,.right {
        float: left;
        width: 100px;
        height: 200px;
        background-color: blue;
    }
    .left {
      margin-left: -100%;    
    }
    .right {
       margin-left: -100px;
    }
</style>
<body class="clearfix">
    <div class="container ">
        <div class="center">中</div>
    </div>
    <div class="left">左</div>
    <div class="right">右</div>
</body>

</html>
```

## 5 描述一下脚本`<script>`放在`<head>`和放到`<body>`底部的区别

放`<head>`中的情况：脚本会优先加载，但加载过程中，`<body>`还没加载完，会使脚本访问不到`<body>`中的元素。
放`<body>`底部：脚本在`<body>`加载后加载，能够保证脚本有效地访问`<body>`的元素。

## 6.HTML data-* 属性

使用 data-* 属性来嵌入自定义数据：

```html
<ul>
<li data-animal-type="鸟类">喜鹊</li>
<li data-animal-type="鱼类">金枪鱼</li> 
<li data-animal-type="蜘蛛">蝇虎</li> 
</ul>
```

data-* 属性用于存储页面或应用程序的私有自定义数据。

data-* 属性赋予我们在所有 HTML 元素上嵌入自定义 data 属性的能力。

```js
<script>
function showDetails(animal) {
    var animalType = animal.getAttribute("data-animal-type");
    alert(animal.innerHTML + "是一种" + animalType + "。");
}
</script>
```

`getAttribute（）`方法以及`setAttribute()`方法操作自定义属性

**使用data-属性选择器好处：绑定DOM强相关数据，js传递数据**

## 7.cookie，sessionStorage，localStorage区别

localStorage：本地存储  sessionStorage：会话存储

差异性：（面试经常问） 

相同点：都是存储数据，存储在web端，并且都是同源 

不同点： 

（1）cookie 只有4K 小 并且每一次同源HTTP请求中携带。一般是加密的字符串，用来标记用户身份

（2）session和local浏览器本地存储的一种方式请求不会携带以键值对形式存储，并且容量比cookie要大的多 

（3）session 是临时会话，当窗口被关闭的时候就清除掉 ，而 local永久存在，除非手动删除，cookie有过期时间 

（4）cookie 和local都可以支持多窗口共享，而session不支持多窗口共享 但是都支持a链接跳转的新窗口

## 8.为什么要初始化css样式

因为浏览器的兼容问题，不同浏览器对某些元素默认样式是不同的，如果没有进行css样式的初始化会导致在每一个浏览上的效果会不一样，影响到最终的布局。出现浏览器之间页面显示差距。

## 9.什么是postcss，有什么作用

postcss是一个平台。

基于这个平台可以用一些插件来优化css代码，如autoprefixer插件，就基于postcss，为css加上不同浏览器前缀。

## 10 页面导入样式时，使用link和@import有什么区别？

- link属于XHTML标签，除了加载CSS外，还能用于定义RSS,定义rel连接属性等作用；而@import是CSS提供的，只能用于加载CSS;
- 页面被加载的时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载;
- import是CSS2.1 提出的，只在IE5以上才能被识别，而link是XHTML标签，无兼容问题;

## 11`<img>`的`title`和`alt`有什么区别

- `title`通常当鼠标滑动到元素上的时候显示
- `alt`是`<img>`的特有属性，是图片内容的等价描述，用于图片无法加载时显示、读屏器阅读图片。可提图片高可访问性，除了纯装饰图片外都必须设置有意义的值，搜索引擎会重点分析。

## 12viewport

```html
 <meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />
     width    设置viewport宽度，为一个正整数，或字符串‘device-width’
     device-width  设备宽度
     height   设置viewport高度，一般设置了宽度，会自动解析出高度，可以不用设置
     initial-scale    默认缩放比例（初始缩放比例），为一个数字，可以带小数
     minimum-scale    允许用户最小缩放比例，为一个数字，可以带小数
     maximum-scale    允许用户最大缩放比例，为一个数字，可以带小数
     user-scalable    是否允许手动缩放
```

怎样处理 移动端 `1px` 被 渲染成 `2px`问题?

- `mate`标签中的 `viewport`属性 ，`initial-scale` 设置为 `0.5`

- `rem` 按照设计稿标准走即可

## 13 BFC

### 什么是BFC

BFC全称 Block Formatting Context 即`块级格式上下文`，简单的说，BFC是页面上的一个隔离的独立容器，不受外界干扰或干扰外界

### 如何触发BFC

- `float`不为 none
- `overflow`的值不为 visible
- `position` 为 absolute 或 fixed
- `display`的值为 inline-block 或 table-cell 或 table-caption 或 grid

### BFC的渲染规则是什么

- BFC是页面上的一个隔离的独立容器，不受外界干扰或干扰外界
- 计算BFC的高度时，浮动子元素也参与计算（**即内部有浮动元素时也不会发生高度塌陷**）
- BFC的区域不会与float的元素区域重叠
- BFC内部的元素会在垂直方向上放置
- BFC内部两个相邻元素的margin会发生重叠

### BFC的应用场景

- **清除浮动**：BFC内部的浮动元素会参与高度计算，因此可用于清除浮动，防止高度塌陷
- **避免某元素被浮动元素覆盖**：BFC的区域不会与浮动元素的区域重叠
- **阻止外边距重叠**：属于同一个BFC的两个相邻Box的margin会发生折叠，不同BFC不会发生折叠