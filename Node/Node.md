# node

![image-20210629102135968](C:\Users\heyongjian\AppData\Roaming\Typora\typora-user-images\image-20210629102135968.png)

## node的运行方式

- 脚本模式

`node path/filename.js`

- 交互模式

`node (回车) 进入交互模式` `js代码（回车）运行代码` `.exit或者连续两次ctrl+c 退出交互模式`

交互模式下，使用tab自动补全，可以探索js对象（例如：Math.然后按两次tab键 ）

## 全局对象与全局函数

### 全局对象

在node中全局对象为global，对应浏览器中全局对象的window

- 在**交互模式**下，声明的变量和函数都属于global

```js
var a = 1
global.a （1）
```

- 在**脚本模式**下，声明的变量和函数都不属于全局对象global 

```js
var a = 1
global.a(undefined)
```

### 全局函数

- node 先执行主进程，再执行事件队列
- `setImmediate()在事件队列开始之前执行`
- `process.nextTick()`在主进程结束后立即执行

## 内置模块

### console

- `console.time和consloe.timeEnd`可以计算运行时间

```js
console.time('time')
for(let i=0;i<10000;i++){}
console.timeEnd('time');
// 可以计算出for循环所需的时间
```

### process

```js
// 输出node版本
console.log(process.version);

// 输出操作系统架构
console.log(process.arch);

// 输出操作系统平台
console.log(process.platform);

// 输出当前工作目录
console.log(process.cwd());

// 自定义环境变量
process.env.NODE_ENV = 15
// 环境变量
console.log(process.env);

// 获取进程的编号
console.log(process.pid);

// 杀死进程 process.kill(进程编号)
process.kill()
```

### path

- 常用的一些用法，具体的去官网查看

```js
// 引入path模块
const path = require('path')

// 获取当前文件所在的目录
console.log(__dirname);
console.log(process.cwd());

// 获取当前文件的路径
console.log(__filename);

// 文件路径合并
path.join('/foo', 'bar', 'baz/asdf', 'quux', '..');
// 返回: '/foo/bar/baz/asdf';
```

