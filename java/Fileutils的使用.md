

## 文件操作的工具类

**https://www.cnblogs.com/xing901022/p/6120089.html**

```java
/**
 * FileUtils支持很多文件操作，如
 *
 * 文件写入
 * 读取文件
 * 创建目录
 * 拷贝文件和目录
 * 删除文件和目录
 * 从URL转换
 * 基于统配和过滤查看文件和目录
 * 比较文件内容
 * 文件的更新时间
 * 检查校验码
 *
 * /

```

##### 1: 获取基本信息

```java
void FileTest(){
        // 获取临时目录
        System.out.println(FileUtils.getTempDirectory());
        // 获取用户主目录
        System.out.println(FileUtils.getUserDirectory());
        System.out.println(FileUtils.getUserDirectoryPath());
        //以可读的方式，返回文件的大小EB, PB, TB, GB, MB, KB or bytes
        assertEquals(FileUtils.byteCountToDisplaySize(1024 * 1024 * 1025), "1 GB");
        System.out.println(FileUtils.byteCountToDisplaySize(123));
        System.out.println(FileUtils.byteCountToDisplaySize(1));
    }

```



##### 2: 获取文件的输入输出流

```java
 @Test
    public void openStream() throws IOException {
        //获取文件输入和输出的文件流
        //文件是目录或者不存在，都会跑出异常
        InputStream in = FileUtils.openInputStream(new File("D://test/test1"));
        OutputStream out = FileUtils.openOutputStream(new File("D://test/test2"));
        out = FileUtils.openOutputStream(new File("D://test/test3"),true);//是否追加的形式添加内容
    }
```

##### 3： 创建文件，文件对比

```java
FileUtils.touch(new File("D://test/test4")); // 创建文件 //创建文件，如果文件存在则更新时间；如果不存在，创建一个空文件

// 中英文都可以对比
boolean b = FileUtils.contentEquals(new File("D://test/test1"), new File("D://test/test2"));
FileUtils.contentEqualsIgnoreEOL(new File("D://test/test1"),new File("D://test/test2"),null);//忽略换行符，第三个参数是字符集

```

##### 4： 文件对象和url之间的相互转换

```java
//根据URL获取文件
File file = FileUtils.toFile(new URL("file://D://test/test1"));
System.out.println(file.getName()); //  test1
File[] files = FileUtils.toFiles(null);
        
URL[] urls = FileUtils.toURLs(new File[]{new File("D://test/test1")});
Arrays.stream(urls).forEach(System.out::println); // file:/D:/test/test1

```

##### 5：文件的拷贝

```java
//文件拷贝到目标文件夹 //第三个参数是否更新时间
FileUtils.copyFileToDirectory(new File("D://test/test1"),
                              new File("D://test/copy"),true);
// 文件的拷贝 不能存在同名的文件
FileUtils.copyFile(new File("D://test/test1"),
                   new File("D://test/test11"),true);

```

##### 6 ： 目录拷贝

```java
 //目录拷贝
File srcDir = new File("/source");
File destDir = new File("/target");
// 目标路径不存在会报错
FileUtils.copyDirectoryToDirectory(new File("/source"),new File("/target"));
// 这个是直接新建目标路径
FileUtils.copyDirectory(new File("/source"),new File("/target"));

FileUtils.copyDirectory(srcDir, destDir, DirectoryFileFilter.DIRECTORY);//仅仅拷贝目录
```



