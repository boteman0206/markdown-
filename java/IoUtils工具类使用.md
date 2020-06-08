##### 1：流的拷贝

```java
copy(inputstream,outputstream)
copy(inputstream,writer)
copy(inputstream,writer,encoding)
copy(reader,outputstream)
copy(reader,writer)
copy(reader,writer,encoding)
// copy内部使用的其实还是copyLarge方法。因为copy能拷贝Integer.MAX_VALUE的字节数据，即2^31-1。

// copyLarge 这个方法适合拷贝较大的数据流，比如2G以上。    
copyLarge(reader,writer) 默认会用1024*4的buffer来读取
copyLarge(reader,writer,buffer)

```



##### 2： copy复制使用， 追加内容到文件中

```java
@Test
    void copyData() throws IOException {
        InputStream is = IOUtils.toInputStream("hello world");
        File f=new 		File("D:\\marcpoint\\demo\\src\\main\\resources\\static\\abc");
        Writer out=new FileWriter(f,true); // 以追加的方式
        IOUtils.copy(is, out); // 将hello world 追加到文件f中
        out.flush();
}

```



##### 3： 生成文件流对象，read 读取文件流内容

```java
read(inputstream,byte[])
read(inputstream,byte[],offset,length) 
//offset是buffer的偏移值，length是读取的长度
read(reader,char[])
read(reader,char[],offset,length)

@Test
void  getInputStram(){

    try{
         
        byte[] bytes = new byte[4];
        InputStream is = IOUtils.toInputStream("hello world");
        IOUtils.read(is, bytes);
        System.out.println(new String(bytes));
		
        bytes = new byte[10];
        is = IOUtils.toInputStream("hello world");
        IOUtils.read(is, bytes, 2, 4);
        System.out.println(new String(bytes));
    } catch (IOException e) {
        e.printStackTrace();
    }
}
// readFully 这个方法会读取指定长度的流，如果读取的长度不够，就会抛出异常
@Test
    public void readFullyTest(){
        byte[] bytes = new byte[4];
        InputStream is  = IOUtils.toInputStream("hello world");
        try {
            IOUtils.readFully(is,bytes);
            System.out.println(new String(bytes));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```

##### 4: readLines 从流中读取内容，并转换成String的list

```java
readLines(inputstream)
readLines(inputstream,charset)
readLines(inputstream,encoding)
readLines(reader)

@Test
public void readLinesTest(){
     try{
            InputStream is = new FileInputStream("D://test1.txt");
            List<String> lines = IOUtils.readLines(is);
            for(String line : lines){
                System.out.println(line);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
 }
  
```



##### 5: skip  这个方法用于跳过指定长度的流，

```java
skip(inputstream,skip_length)
skip(ReadableByteChannel,skip_length)
skip(reader,skip_length)

@Test
    public void skipTest(){
        InputStream is = IOUtils.toInputStream("hello world");
        try {
            IOUtils.skip(is,4);
            System.out.println(IOUtils.toString(is,"utf-8")); // o world
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

// skipFully 这个方法类似skip，只是如果忽略的长度大于现有的长度，就会抛出异常
skipFully(inputstream,toSkip)
skipFully(readableByteChannel,toSkip)
skipFully(inputstream,toSkip)

```



##### 6: write 这个方法可以把数据写入到输出流中

```java
write(byte[] data, OutputStream output)
write(byte[] data, Writer output)
write(byte[] data, Writer output, Charset encoding)
write(byte[] data, Writer output, String encoding)
write(char[] data, OutputStream output)
write(char[] data, OutputStream output, Charset encoding)
write(char[] data, OutputStream output, String encoding)
write(char[] data, Writer output)
write(CharSequence data, OutputStream output)
write(CharSequence data, OutputStream output, Charset encoding)
write(CharSequence data, OutputStream output, String encoding)
write(CharSequence data, Writer output)
write(StringBuffer data, OutputStream output)
write(StringBuffer data, OutputStream output, String encoding)
write(StringBuffer data, Writer output)
write(String data, OutputStream output)
write(String data, OutputStream output, Charset encoding)
write(String data, OutputStream output, String encoding)
write(String data, Writer output)

@Test
    public void writeTest(){
        try {
            OutputStream os = new FileOutputStream("E:/test.txt");
            IOUtils.write("hello write!",os);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```

#### 7: writeLines 这个方法可以把string的List写入到输出流中

```java
writeLines(Collection<?> lines, String lineEnding, OutputStream output)
writeLines(Collection<?> lines, String lineEnding, OutputStream output, Charset encoding)
writeLines(Collection<?> lines, String lineEnding, OutputStream output, String encoding)
writeLines(Collection<?> lines, String lineEnding, Writer writer)
    
@Test
    public void writeLinesTest() throws IOException {
        List<String> lines = new ArrayList();
        lines.add("hello");
        lines.add("list");
        lines.add("to");
        lines.add("file");
        OutputStream os = new FileOutputStream("E:/test.txt");
        IOUtils.writeLines(lines,IOUtils.LINE_SEPARATOR,os);//  IOUtils.LINE_SEPARATOR  指定分隔符号
    }
```

##### 8： close 关闭URL连接

```java
close(URLConnection conn)
// closeQuietly 忽略nulls和异常，关闭某个流
close(URLConnection conn)
closeQuietly(Closeable... closeables)
closeQuietly(Closeable closeable)
closeQuietly(InputStream input)
closeQuietly(OutputStream output)
closeQuietly(Reader input)
closeQuietly(Selector selector)
closeQuietly(ServerSocket sock)
closeQuietly(Socket sock)
closeQuietly(Writer output)
```



##### 9: contentEquals 比较两个流是否相同

```java
contentEquals(InputStream input1, InputStream input2)
contentEquals(Reader input1, Reader input2)
// contentEqualsIgnoreEOL  比较两个流，忽略换行符  

@Test
    public void contentEqualsTest(){
        InputStream is1 = IOUtils.toInputStream("hello123");
        InputStream is2 = IOUtils.toInputStream("hello123");

        try {
            System.out.println(IOUtils.contentEquals(is1,is2));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

```



##### 10 : lineIterator 读取流，返回迭代器

```java
lineIterator(InputStream input, Charset encoding)
lineIterator(InputStream input, String encoding)
lineIterator(Reader reader)

```

##### 11: toBufferedInputStream 把流的全部内容放在另一个流中

```java
toBufferedInputStream(InputStream input) // 原来的流的内容已经不存在了
toBufferedInputStream(InputStream input, int size)
```

##### 12： toBufferedReader 返回输入流

```java
toBufferedReader(Reader reader)
toBufferedReader(Reader reader, int size)
   
@Test
    void  testr() throws IOException {
        FileReader fileReader = new FileReader("D:\\marcpoint\\demo\\src\\main\\resources\\static\\test");
        BufferedReader bufferedReader = IOUtils.toBufferedReader(fileReader);
        System.out.println(IOUtils.toString(bufferedReader));
    }
```

##### 13: toByteArray 返回字节数组

```java
toByteArray(InputStream input)
toByteArray(InputStream input, int size)
toByteArray(InputStream input, long size)
toByteArray(Reader input)
toByteArray(Reader input, Charset encoding)
toByteArray(Reader input, String encoding)
toByteArray(String input)
toByteArray(URI uri)
toByteArray(URL url)
toByteArray(URLConnection urlConn)
```

##### 14: toCharArray 返回字符数组

```java
toCharArray(InputStream is)
toCharArray(InputStream is, Charset encoding)
toCharArray(InputStream is, String encoding)
toCharArray(Reader input)
```

#### 15: toInputStream 返回输入流

```java
toInputStream(CharSequence input)
toInputStream(CharSequence input, Charset encoding)
toInputStream(CharSequence input, String encoding)
toInputStream(String input)
toInputStream(String input, Charset encoding)
toInputStream(String input, String encoding)
```

##### 16: toString  返回字符串

```java
toString(byte[] input)
toString(byte[] input, String encoding)
toString(InputStream input)
toString(InputStream input, Charset encoding)
toString(InputStream input, String encoding)
toString(Reader input)
toString(URI uri)
toString(URI uri, Charset encoding)
toString(URI uri, String encoding)
toString(URL url)
toString(URL url, Charset encoding)
toString(URL url, String encoding)
```



##### 17: 读取url文件流对象

```java
@Test
void testTostring() throws IOException {
     URL url = new URL("https://cloud.tencent.com/developer/article/1022130");
     String string = IOUtils.toString(url, "utf-8");
     System.out.println(string);
    }
```

