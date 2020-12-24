# Node的REPL
REPL是Read-Eval-Print Loop的简称，翻译为“读取-求值-输出”循环；
REPL是一个交互式的编程环境，浏览器的console和node环境就可以看成一个REPL

`node index.js env=development xxx`命令会给node传递参数xxx
获取参数则是在**process**这个内置对象中
![](https://cdn.jsdelivr.net/gh/Hendry5479/learn-react/img/20201224125937.png)
其中的argv属性是一个数组，里面包含了目标参数；
**argv属性的由来**：
在C/C++程序中的main函数中可以获取到两个参数：
argc：argument counter，传递参数的个数
argv：argument vector，传入的具体参数
vector在C++、Java中是一种数组结构，在JavaScript中也是数组，用于存储参数信息；

在代码中将参数信息遍历：
``` JavaScript
console.log(process.argv);
process.argv.forEach(item => {
  console.log(item);
});
```

# 特殊的全局对象
这些全局对象可以在模块中任意使用，但是在命令行交互中不可以使用；
包括：**__dirname、__filename、exports、module、require()**
### __dirname
获取当前文件所在的路径，不包括后面的文件名
### __filename
获取当前文件所在的路径和文件名称，包括后面的文件名称

# 常见的全局对象
### process
提供了Node进程中相关的信息，比如Node的运行环境、参数信息等；
### console
提供了调试控制台
#### 定时器函数
在Node中使用定时器有好几种方式：
setTimeout(callback, delay[, ...args])：callback在delay毫秒后执行一次；
setInterval(callback, delay[, ...args])：callback每delay毫秒重复执行一次；
setImmediate(callback[, ...args])：callback在IO事件后的回调的“立即”执行；
process.nextTick(callback[, ...args])：添加到下一次tick队列中；


### global
process、console、setTimeout等都有被放到global中

window和global的区别是什么？
在浏览器中，全局变量在window上，比如document、setInterval、alert、console等
在浏览器中执行的代码，如果在顶级范围内通过var定义一个属性，默认会被添加到window
``` JavaScript
var name = 'c';
console.log(window.name); // c
```
在node中通过var定义变量，它只是在当前模块中有一个变量，不会放到全局中：
``` JavaScript
var name = 'c';
console.log(global.name); // undefined
```