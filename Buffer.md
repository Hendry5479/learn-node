# Buffer和二进制
Buffer中存储的是二进制数据
字节byte = 8位二进制  
1byte = 8bit，1kb=1024byte，1M=1024kb
编程语言的int类型是4个字节，long类型是8个字节
TCP传输字节流，在写入和读取时都需要说明字节个数
RGB的值分别都是255，所以在计算机中都是用一个字节存储

**1.create new Buffer**
``` JavaScript
const buffer = new Buffer(message)
const buffer = Buffer.from(message)
```
中文一般是3个字节

**2.解码**
`buffer.toString() ` 默认utf8

**3.alloc**
`buffer = Buffer.alloc(8)`
`buffer[1] = 0x88`

**4.文件**
``` JavaScript
const fs = require('fs');
fs.readFile('./test.txt', (err, data) => {
  console.log(data); // <Buffer 48 65 6c 6c 6f 20 57 6f 72 6c 64>
  console.log(data.toString()); // Hello World
})

fs.readFile('./zznh.jpg', (err, data) => {
  console.log(data); // <Buffer ff d8 ff e0 ... 40418 more bytes>
});

const sharp = require('sharp');
sharp('./test.png')
  .resize(1000, 1000)
  .toBuffer()
  .then(data => {
    fs.writeFileSync('./test_copy.png', data);
  })
```

# Stream
**流的概念**：连续字节的一种表现形式和抽象概念，流应该是可读可写的
直接读写文件无法控制一些细节的操作；
Node中很多对象基于流实现：http模块的Request和Response，process.stdout
所有的流都是EventEmitter的实例

**流（Stream）的分类**：
Writable：例如 fs.createWriteStream()
Readable：例如 fs.createReadStream()
Duplex：同时为Readable和Writable,例如 net.Socket
Transform：Duplex可以在写读时修改转换数据的流,例如zlib.createDeflate()

# Readable
`createReadStream(start, end)`
``` JavaScript
const read = fs.createReadStream("./foo.txt", {
  start: 3,
  end: 8,
  highWaterMark: 4
});
// 监听data事件获取读取到的数据；
read.on("data", (data) => {
  console.log(data);
});
read.on('open', (fd) => {
  console.log("文件被打开");
})
read.on('end', () => {
  console.log("文件读取结束");
})
read.on('close', () => {
  console.log("文件被关闭");
})
// 在某一个时刻暂停和恢复读取：
read.on("data", (data) => {
  console.log(data);
  read.pause();
  setTimeout(() => {
    read.resume();
  }, 2000);
});
```

# Writable
createWriteStream(flags, start)
flags：默认w，如果追加写入使用 a或者 a+；

``` JavaScript
const writer = fs.createWriteStream("./foo.txt", {
  flags: "a+",
  start: 8
});

writer.write("你好啊", err => {
  console.log("写入成功");
});

writer.on("open", () => {
  console.log("文件打开");
})

writer.on("finish", () => {
  console.log("文件写入结束");
})
```