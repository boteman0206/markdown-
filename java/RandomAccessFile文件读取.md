##### RandomAccessFile既可以读取文件内容，也可以向文件输出数据。同时，RandomAccessFile支持“随机访问”的方式，程序快可以直接跳转到文件的任意地方来读写数据。

##### 类似于python中的读取文件的f.seek(4, 0)

```tex
优势：
	与OutputStream、Writer等输出流不同的是，RandomAccessFile允许自由定义文件记录指针，RandomAccessFile可以不从开始的地方开始输出，因此RandomAccessFile可以向已存在的文件后追加内容。如果程序需要向已存在的文件后追加内容，则应该使用RandomAccessFile。
```

##### 1.RandomAccessFile的构造函数

```tx
## 读取文件的四种模式
"r" : 以只读方式打开。调用结果对象的任何 write 方法都将导致抛出 IOException。
"rw": 打开以便读取和写入。
"rws": 打开以便读取和写入。相对于 "rw"，"rws" 还要求对“文件的内容”或“元数据”的每个更新都同步写入到基础存储设备。
"rwd" : 打开以便读取和写入，相对于 "rw"，"rwd" 还要求对“文件的内容”的每个更新都同步写入到基础存储设备。
```



