

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


void testInput() throws IOException {

        // 拷贝文件   /target/anm 不存在会新建
        FileUtils.copyInputStreamToFile(
                new FileInputStream("D:\\marcpoint\\demo\\src\\main\\resources\\static\\test.txt"),
                new File("/target/anm")
        );

        FileUtils.copyURLToFile(new URL("file:/test"),new File("/test"));

    }

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

##### 7： 构建文件过滤器进行拷贝

```java
@Test
void fileFilterTest() throws IOException {
        File srcDir = new File("D:\\marcpoint\\demo\\src\\main\\resources\\static");
        File destDir = new File("/target");
        IOFileFilter txtSuffixFilter = FileFilterUtils.suffixFileFilter(".txt");// 创建.txt过滤器 todo 学习一下文件文件过滤器的使用
        IOFileFilter txtFiles = FileFilterUtils.andFileFilter(FileFileFilter.FILE, txtSuffixFilter);
        // 创建包含目录或者txt文件的过滤器
        FileFilter filter = FileFilterUtils.orFileFilter(DirectoryFileFilter.DIRECTORY, txtFiles);
        // Copy using the filter
        FileUtils.copyDirectory(srcDir, destDir, filter);// 只是文件中带有.txt结尾的文件进行拷贝
    }
```

##### 8: 删除文件

```java

@Test
void  testDelete() throws IOException {
        // 删除文件 会将目录也一并删除
        FileUtils.deleteDirectory(new File("/target"));//删除目录下所有的内容
        FileUtils.deleteQuietly(new File("/target"));//如果是目录，会级联删除；不会抛出异常 
       // 只会清空文件夹中的内容 不会删除目录
        FileUtils.cleanDirectory(new File("/target"));
    }

//强制删除
FileUtils.forceDelete(new File("/target"));
```

##### 9： 判断文件夹是否包含

```java
//判断文件是否存在  不管是文件夹还是文件只要名字是abc都是true
 boolean b = FileUtils.directoryContains(new File("/target"), new File("/target/abc"));
 System.out.println(b);
```

##### 10： 读取文件内容 和Ioutils内容差不多

```java
//读取目标文件，内部调用IOUtils.toString(inputstream,"utf-8")
String str = FileUtils.readFileToString(new File("/dir"),"utf-8");
//内部调用IOUtils.toByteArray(in)
byte[] bytes = FileUtils.readFileToByteArray(new File("/dir"));
//内部调用IOUtils.readLines(in, Charsets.toCharset(encoding));
List<String> strs = FileUtils.readLines(new File("/dir"),"utf-8");
//内部调用IOUtils.lineIterator(in, encoding)
FileUtils.lineIterator(new File("/dir"),"utf-8");

//如果是文件，直接读取文件大小；如果是目录，级联计算文件下的所有文件大小
FileUtils.sizeOf(new File("/target"));//返回Long
FileUtils.sizeOfAsBigInteger(new File("/target"));//返回BigInteger
FileUtils.sizeOfDirectory(new File("/target"));
FileUtils.sizeOfDirectoryAsBigInteger(new File("/target"));
```

##### 11：文件的写入

```java
//四个参数分别为：目标文件，写入的字符串，字符集，是否追加
 FileUtils.writeStringToFile(new File("/target"),"string","utf-8",true);

//write可以接受charsequence类型的数据，string,stringbuilder和stringbuffer都是实现了charsequence接口
FileUtils.write(new File("/target"),"target char sequence","utf-8",true);
FileUtils.writeByteArrayToFile(new File("/target"),"bytes".getBytes());//(file,字符数组)
FileUtils.writeByteArrayToFile(new File("/target"),"bytes".getBytes(),true);//(file,字符数组，是否追加)
FileUtils.writeByteArrayToFile(new File("/target"),"bytes".getBytes(),0,10);//(file,字符数组，起始位置，结束位置)
FileUtils.writeByteArrayToFile(new File("/target"),"bytes".getBytes(),0,10,true);//(file,字符数组，起始位置，结束位置，是否追加)

//writeLines多了一个lineEnding参数
FileUtils.writeLines(new File("/target"),"utf-8", FileUtils.readLines(new File("/target"),"utf-8"));

```

##### 12: 强制删除创建文件

```java
//强制删除
FileUtils.forceDelete(new File("/target"));
//在JVM
FileUtils.forceDeleteOnExit(new File("/target"));

// 强制创建文件目录，如果文件存在，会抛出异常
FileUtils.forceMkdir(new File("/target"));

//强制创建父级目录
FileUtils.forceMkdirParent(new File("/xxxx/target"));
```

##### 13: 文件的移动

```java
FileUtils.moveDirectory(new File("/target"),new File("/file"));
FileUtils.moveDirectoryToDirectory(new File("/target"),new File("/file"),true);
FileUtils.moveFile(new File("/target"),new File("/file"));
FileUtils.moveFileToDirectory(new File("/target"),new File("/dir"),true);
FileUtils.moveToDirectory(new File("/target"),new File("/dir"),true);
```

##### 14: 文件的新旧对比

```java
//对比文件新旧
FileUtils.isFileNewer(new File("/target"),new File("/file"));
FileUtils.isFileOlder(new File("/target"), new Date());
```

