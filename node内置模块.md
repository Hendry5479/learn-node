## path
window使用 \或者 \\ 作为分隔符，目前也支持 /
在Mac OS、Linux的Unix操作系统上使用 / 来作为文件路径的分隔符；
### path常见的API
从路径中获取信息
dirname：获取文件的父文件夹；
basename：获取文件名；
extname：获取文件扩展名；
路径的拼接：path.join(basepath, filename)
将文件和文件夹拼接：path.resolve
``` JavaScript
const path = require('path');
const basePath = '/User/why';
const filename = '../abc.txt';
const filepath = path.resolve(basePath, filename);
console.log(filepath); // D:\User\abc.txt
```
resolve函数会判断拼接的路径前面是否有 /或../或./，会返回对应的拼接路径；


# fs：File System
API提供三种操作方式：
方式一：同步操作，代码会被阻塞
方式二：异步回调函数，代码不阻塞，传入回调函数，当获取到结果时，回调函数被执行
方式三：异步Promise，代码不阻塞，通过 fs.promises 调用，返回Promise，通过then、catch处理
 
**文件描述符 File descriptors**
在 POSIX 系统上，每个进程，内核都维护着一张当前打开着的文件和资源的表格
每个打开的文件都分配了一个文件描述符，文件操作都使用文件描述符来标识和跟踪文件

**fs.open()**：分配新的文件描述符，可用于读取数据、写入、或请求文件信息

#### 文件的读写
`fs.readFile(path[, options], callback)`
`fs.writeFile(file, data[, options], callback)`
**文件写入**：
``` JavaScript
fs.writeFile('../foo.txt', content, {}, err => {
  console.log(err);
})
```
大括号是option参数：flag：写入的方式，encoding：字符的编码
flag选项：
w 打开文件写入， w+如果不存在则创建文件；
r+ 打开文件读写，不存在则抛出异常，r打开文件读取，读取时的默认值；
a打开要写入的文件，将流放在文件末尾。如果不存在则创建文件；
a+打开文件以进行读写，将流放在文件末尾。如果不存在则创建文件

文件夹操作
新建文件夹：fs.mkdir()或fs.mkdirSync()
获取文件夹的内容
文件写入：
``` JavaScript
function readFolders(folder) {
  fs.readdir(folder, {withFileTypes: true} ,(err, files) => {
    files.forEach(file => {
      if (file.isDirectory()) {
        const newFolder = path.resolve(dirname, file.name);
        readFolders(newFolder);
      } else {
        console.log(file.name);
      }
    })
  })
}
readFolders(dirname);
```
文件重命名
`fs.rename('../why', '../coder', err => {})`


# events模块
Node的核心API都基于**异步事件驱动**
对象**Emitters**发出事件，监听这个事件Listeners，传入回调函数
回调函数会在监听到事件时调用

发出和监听事件都通过**EventEmitter**完成，它们都属于events对象
emitter.on(name, listener)：监听事件，也可以使用addListener；
emitter.off(name, listener)：移除事件监听，也可以使用removeListener；
emitter.emit(name[, ...args])：发出事件，可以携带参数；

**emitter常见的属性**
eventNames()：返回EventEmitter对象注册的事件字符串数组
getMaxListeners()：返回最大监听器数量，通过setMaxListeners()修改，默认10
listenerCount(事件名称)：返回某一个事件名称，监听器个数；
listeners(事件名称)：返回监听器上所有的监听器数组；

**方法补充**
once(name, listener)：事件监听一次
prependListener()：将监听事件添加到最前面
prependOnceListener()：将监听事件添加到最前面，但是只监听一次
removeAllListeners([name])：移除监听器


