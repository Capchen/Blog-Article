fs模块是唯一一个同时提供同步和异步API的模块

- fs.readdirSync(__dirname) 同步读取一个文件
- fs.readdir('.',function(err, files){}) 异步读取文件


### Stream
流，比起使用fs.readFile和rs.writeFile方法对文件进行读写操作，
使用Stream API来进行读写操作可以避免想上面的方法一样需要一次分配内存来进行操作，stream可以分段处理，这样在读取大文件或者上传大文件时有足够的操作空间。
