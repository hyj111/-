# 题目1：

```js
  let a = {},
        b = '0',
        c = 0
        a[b] = '你好'
        a[c] = '小老弟'
        console.log(a[b]); //小老弟
```

分析：在对象中属性名不能重复，数字属性名==数字属性名即，后面赋值的会覆盖前面的赋值的。

```js
// 在上面代码中
console.log(a[0] == a['0']) //true
```

# 题目2：

```js
    let a = {},
        b = Symbol('1'),
        c = Symbol('1')
        a[b] = '你好'
        a[c] = '小老弟'
        console.log(a[b]); //你好
```

分析：`Symbol`函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的`Symbol`函数的返回值是不相等的。

```js
// 有参数的情况
let s1 = Symbol('foo');
let s2 = Symbol('foo');

s1 === s2 // false
```

# 题目3

```js
    let a = {},
        b={
            n:"1"
        },
        c = {
            m:'2'
        }
        a[b] = '你好'
        a[c] = '小老弟'
        console.log(a[b]); //小老弟
```

分析：对象中以对象作为`key`，`key`都是`[object Object]`

```js
   let a = {},
        b={
            n:"1"
        }
        a[b] = '你好'
        console.log(a);
```

![image-20200920143052332](面试(二)js.assets/image-20200920143052332.png)

所以题目3两个key都是`[object Object]`，所以会被覆盖。

# 题目4（闭包）

```js
    var test = (function(i){
        return function(){
            alert(i*=2) //'4'
        }
    })(2)
    test(5)
```

分析:**答案为字符串4**，因为`alert`输出的都会调用`toString()`，其实是一个立即函数里面返回一个函数，`test`等于一个函数，函数中没有形参，`i`就去上一层作用域中查找。立即执行函数形成了闭包，不会被销毁。

题目变换

```js
    var test = (function(i){
        return function(i){
            alert(i*=2) // '10'
        }
    })(2)
    test(5)
```

# **题目5（闭包，函数重写）**

```js
 var a = 0,
    b = 0;
    function A(a) {
        A = function(b) {
            alert(a + b++)
        };
        alert(a++);
    }
    A(1);   //1
    A(2)   //4
```

分析：先执行外面第一层函数，重写了A函数，并输出1，第二次调用A函数就是调用，里面的那个函数了，由于形成了闭包，a=2，不销毁，里面函数往上一级作用域寻找a，最后输出4.

**题目6（深浅克隆）**







# **题目7 面向对象+变量提升+运算符优先级**

```js
    function Foo(){
        getName = function(){
            console.log(1);
        }
        return this;
    }

    Foo.getName = function() {
        console.log(2);
    }
    Foo.prototype.getName = function(){
        console.log(3);
    }

    var getName = function(){
        console.log(4);
    }
    function getName() {
        console.log(5);
    }
    Foo.getName();//2
    getName();//4
    Foo().getName();//1
    getName();//1
    new Foo.getName() //先进行成员访问再new  2
    new Foo().getName()//3
    new new Foo().getName();//3
```

分析：首先进行函数和变量提升，函数提升要比变量提升的优先级要高一些，且不会被变量声明覆盖，但是会被变量赋值之后覆盖。

第一步：

```js
  function Foo(){
        getName = function(){
            console.log(1);
        }
        return this;
    }
	var getName = function() {
        console.log(4);
    }
    Foo.getName = function() {
        console.log(2);
    }
    Foo.prototype.getName = function(){
        console.log(3);
    }


```

以上变量提升完成

**注意**

```js
  function Foo(){
        getName = function(){
            console.log(1);
        }
        return this;
    }
    console.log(Foo.getName) //undifined
```

**第二步**开始执行 `Foo.getName()`，得到结果为2

**第三步**开始执行`getName()`得到结果4

**第四步**开始执行`Foo().getName();`得到结果为1,先执行`Foo()`得到结果是`this`

且给全局`getName`重新赋值了，全局下的`getName` = `function(){console.log(1)}`

```js
   function Foo(){
        getName = function(){
              console.log(1);
        }
        return this;
    }
     Foo.getName = function() {
        console.log(2);
    }
	var getName = function(){
		 console.log(1);
	}
    Foo.prototype.getName = function(){
        console.log(3);
    }
```

，`this`指向调用他的对象也就是`window`

`window.getName()`得到的结果就是1

**第五步**开始执行`new Foo.getName()`得到结果2[根据运算符优先级](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)，优先级高的先执行 **成员访问**的优先级高于**new（无参列表）**

所以先执行`Foo.getName()`再`new`相当于

```js
new function(){
	console.log(2)
}
```

**第六步**开始执行`new Foo().getName()`得到结果3，根据运算符优先级**成员访问**优先级和**new（有参列表）**相同，先`new Foo()`得到一个实例对象，实例的getName()没有，然后跟着原型链往上查找，就找到实例的prototype，输出3.

# 题目8 event loop ,同步异步

首先得知道几个概念：

**队列（Queue）**先进先出。

事件队列（Event Queue）：事件队列分为，微任务队列，和宏任务队列（包括`setTimeout`，`setInterval`，`setImmediate`），

```js
    async function async1() {
        console.log('async1 start');
        await async2();
        console.log('async1 end');
    }

    async function async2() {
        console.log('async2');
    }

    console.log('script start');

    setTimeout(function(){
        console.log('setTimeout');
    },0)

    async1();

    new Promise(function(resolve){
        console.log('promise1');
        resolve()
    }).then(function(){
        console.log('promise2');
    });
    console.log('script end');
// script start , async1 start，async2 , promise1 , script end, async1 end ，promise2 , setTimeout
```

分析：

1.先执行`console.log('script start')`

2.执行到定时器，把定时器加入到宏任务队列中。

3.执行` async1()输出`async1 start

4.执行`await async2()`，输出`async2`, `console.log('async1 end');`这段代码加入到微任务队列中。

5.执行` new Promise`,输出`promise1`,`then`下面的代码加入到微任务队列中。

6.往下执行输出`script end`,到此主栈代码结束。

7.执行微任务队列中代码，**先进先出** 依次输出`async1 end `，`promise2`微任务队列结束

8.执行宏任务队列，输出`setTimeout`

# 题目9 

问题：a为何值时，能成立

```js
var a = ?;
if(a==1 && a==2 && a==3){
	console.log(1)
}
```

**解决方案1：**

```js
    var a = {
        i:1,
        toString(){
            return this.i++
        }
    };
```

 两边值类型不同的时候，默认调用`toString()`方法，用`toString()`方法的返回值分别和1，2，3比较，所以在`a`中设置一个值`i`，每次返回之后`+1`，就能每次都相等了。

**解决方案2：**

首先需要了解一个属性` Object.defineProperty`

**注意：**在defineProperty geter拦截器中不能再次获取当前属性，不然会进入死循环。

```js
  // 监听obj里面的name属性
    Object.defineProperty(obj,'name',{
        get(){
            console.log('获取');
        },
        set() {
            console.log('设置');
        }
    })
```

当获取obj.name的数据时打印 ’获取‘，当给obj.name设置值的时候打印 '设置'。

在全局下加一个`var`相当于给`window`添加一个属性 var a = 1 相当于 window.a = 1,进行if判断的时候会触发` Object.defineProperty`中的`getter`方法，用返回的结果进行比较。所以这个方法也叫数据劫持方法。

```js
    var i = 0;
    Object.defineProperty(window,'a',{
        get() {
            console.log("调用");
          return ++i       
        },
    });
   
    if(a==1 && a==2 && a==3){
	    console.log(1)
    }
```

**解决方案3：**还是采用了会默认会调取`toString()`的方式，重写`toString()`方法

```js
   var a = [1, 2, 3]
    a.toString = function(){
        return a.shift()
    }
    if (a == 1 && a == 2 && a == 3) {
        console.log(1)
    }
```

# 题目10 == 和  === 的区别

(1)  "=="叫做相等运算符，"==="叫做严格运算符。

(2) ==，equality -> 等同 的意思， 两边值类型不同的时候，要先进行类型转换为同一类型后，再比较值是否相等。 

(3) "=="表示只要值相等即可为真，而"==="则要求不仅值相等，而且也要求类型相同。

```js
对象==字符串 //true 对象.toString()变为字符串
null == undefined //true 但是和其他值比较就不相等了
NaN == NaN //false
//除了这三条剩下的转换为数字
```

# 什么是闭包，解决了什么问题

闭包就是可以访问其他函数中变量的一个函数，通常在一个函数内部再去创建另外一个函数，子函数调用父函数的局部变量，被访问到的局部变量会始终保存再内存之中。

**它的最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中，不会在调用后被自动清除。**

# 原型和原型链分别是什么？有什么特点？

**prototype：原型**

```js
function Person(name, age){ 
    this.name = name;
    this.age = age;
 }
let person01 = new Person('小明', 18);
1. Person.prototype.constructor == Person // **准则1：原型对象（即Person.prototype）的constructor指向构造函数本身**
2. person01.__proto__ == Person.prototype // **准则2：实例（即person01）的__proto__和原型对象指向同一个地方**


```

**原型是每一个JS函数中都自带的一个属性，他的值是一个对象，叫做原型对象**

原型的作用：为实例化对象提供共享的属性和方法。

首先有一个构造函数，`构造函数.prototype` 指向一个原型对象，构造函数里的`constructor`指向构造函数本身，当使用构造函数new一个对象实例，`对象实例.__proto__`指向`构造函数.prototype`

**原型链**：

当访问每一个实例对象的属性时，先在对象中查找，没有就去实例对象的原型对象里找，如果原型对象里没有，则再往上一层查找，也就是原型对象的原型中查找，一直找到最上层的原型也就是Object为止。

# 谈谈对this指向的理解

* this总是指向**函数的直接调用者**
* 如果有new关键字，this指向new出来的那个对象
* 在事件中，this指向触发这个事件的对象。

# 谈谈你对webpack的看法

* webpack是一个js的模块打包工具，可以使用webpack管理项目中的js模块依赖。
* webpack提供一些默认配置，比如devServer，我们可以利用devServer来快速启动一个开发时的web服务器。
* 因为webpack默认只能打包js文件，所以webpack提供了loader的概念，我们可以使用loader来预处理一些文件，并且可以打包除js之外的静态资源。

# 谈谈你对promise的理解

* Promise 是异步编程的一种解决方案 ，`Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。
* Promise的构造函数接收一个函数作为参数，函数中的参数分别是resolve和reject，这两个参数也是一个函数，用来标记异步操作的执行状态，Promise中有三个状态 pendding，fulfilled，reject只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。
* 当promise异步操作完成的时候，我们可以调用resolve函数，来标记当前的异步操作已经完成了
* 而reject，是在异步操作失败的时候调用，用来标记当前异步操作失败
* 这些标记的状态可以通过promise实例对象的.then方法和.catch方法接收。其中.then方法是异步完成的回调，.catch是异步失败的回调。

# null和undefined的区别是什么

![image-20200922215222714](面试(二)js.assets/image-20200922215222714.png)

* undefined：表示不存在这个值，它是一个变量最原始的状态。
* null：它是一个具体的值，只不过这个值是一个空值而已。

```js
console.log(undefined == null) //true
typeof null; // "object"
typeof undefined; // "undefined"
Number(null) //0
Number(undefined)//NaN
```

# 什么是同步，什么是异步

* 同步：顺序处理，即当我们向服务器发出一个请求时，在服务器没返回结果给客户端之前，我们要一直处于等待状态直至服务器将结果返回到客户端，我们才能执行下一步操作。
* 异步：并行处理，当我们向服务器发出一个请求时，在服务器没返回结果之前，我们还是可以执行其他操作。

# 什么是EventLoop

* JS最大特点就是单线程，也就是说，同一时间只能做一件事。
* 于是把任务分为同步任务和异步任务。
* 同步任务指的是，在主线程上依次按顺序排队等待执行
* 异步任务指的是，不进入主线程，而进入任务队列，任务队列又分为微任务队列和宏任务队列，先执行微任务队列的，再执行宏任务队列的。
* 当主线程中任务完成后，主线程从任务队列中读取事件，这就是Event Loop(事件循环)。

整个执行步骤：主线程-->微任务队列-->宏任务队列

# band call apply的区别

- 都能改变this的指向
- call()/apply()是**立即调用函数**，区别在于接受参数的方式不同,apply的参数是数组。
- bind()：绑定完this后，不会立即调用当前函数，而是**将函数返回**，因此后面还需要再加`()`才能调用。

# typeof 于 instanceof的区别

* typeof:会返回一个值的类型。对于基本数据类型，除了null都可以返回正确的类型。而对于引用数据类型来说，除了函数之外，其他都会返回object。在判断不是 object 类型的数据的时候，`typeof`能比较清楚的告诉我们具体是哪一类的类型。
* ![image-20200923093859492](面试(二)js.assets/image-20200923093859492.png)

- instanceof：用来判断一个对象是否是另外一个对象的实例，只能用来**判断对象**。

![image-20200923094354024](面试(二)js.assets/image-20200923094354024.png)

# typeof的原理

js 在底层存储变量的时候，会在变量的机器码的低位1-3位存储其类型信息

- 000：对象
- 010：浮点数
- 100：字符串
- 110：布尔
- 1：整数

but, 对于 `undefined` 和 `null` 来说，这两个值的信息存储是有点特殊的。

`null`：所有机器码均为0

`undefined`：用 −2^30 整数来表示

然而用 `instanceof` 来判断的话👉

```js
null instanceof null // TypeError: Right-hand side of 'instanceof' is not an object
```

`null` 直接被判断为不是 object，这也是 JavaScript 的历史遗留bug，

# 判断一个变量是不是数组？

```js
var arr = [1, 2, 3]
Array.isArray(arr)
arr instanceof Array
arr.constructor === Array
Object.prototype.toString.call(arr) === '[object Array]'
```

# 防抖

事件响应函数在一段时间后才执行，如果在这段时间内再次调用，则重新计算执行时间，当预定的时间内没有再次调用该函数，则执行函数里面内容。

```javascript
// func是用户传入需要防抖的函数
// wait是等待时间
const debounce = (func, wait = 500) => {
  // 缓存一个定时器id
  let timer = 0
  // 这里返回的函数是每次用户实际调用的防抖函数
  return function(...args) { // 接收额外的参数
    if (timer) clearTimeout(timer) // 如果已经设定过定时器了就清空上一次的定时器
    timer = setTimeout(() => { // 开始一个新的定时器，延迟执行用户传入的方法
      func.apply(this, args)
    }, wait)
  }
}
```

上代码中形成了闭包，timer不会被销毁。

# **节流函数（throttle）**

节流函数原理:如果你持续触发事件，每隔一段时间，只执行一次事件。

# 数组扁平化

**数组扁平化就是把多维数组转化成一维数组**

## 方式一 flat()方法

```js
let a = [1,[2,3,[4,[5]]]];  
a.flat(Infinity); // [1,2,3,4,5]  a是4维数组
```

flat :平的，Infinity：无穷

## 方式二 for循环+递归

```js
function flatten(arr) {
        for(let item of arr){
            if(item instanceof Array){
                flatten(item)
            }else{
                newArr.push(item)
            }
        }
        return newArr
    }
```

遍历数组每一项，判断是否是数组，不是数组就添加到新数组中，是数组就递归。

## 方式三 如果数组的项全为数字，可以使用join()，toString()

```js
   function flatten(arr){
        return a.toString().split(',')
     }
```

先转为字符串，【】这两会被去掉，再字符串转数组，但是这个情况下，数组里面的数字变为字符串形的了。

# 事件传播的三个阶段是什么？

在**捕获**（capturing）阶段中，事件从祖先元素向下传播到目标元素。当事件达到**目标**（target）元素后，**冒泡**（bubbling）才开始。