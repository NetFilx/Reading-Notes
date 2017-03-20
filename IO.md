# File
1. 获取给定路径下所有文件或者文件夹，若想获得一个目录下面的所有文件，可以使用递归方式
2. 使用FilenameFilter或者FileFilter过滤文件，两者区别是方法传递的参数不同，FilenameFilter效率略高，过滤的时候一般结合正则表达式使用

# 输入和输出
1. 任何自InputStream或Reader派生的类都包含有名字为read的方法，用于读取单个字节或者字节数组，同样自OutputStream或Writer派生的类都有write方法，用于写单个字节或者字节数组
2. InputStream的类型

|              类               |                    功能                    |               构造器参数<br>使用                |
| :--------------------------: | :--------------------------------------: | :--------------------------------------: |
|     ByteArrayInputStream     |         允许将内存的缓冲区当作InputStream使用         | 缓冲区，字节将从中取出 <br>作为一种数据源：将其与FilterInputStream对象相连以提供有用接口 |
| StringBufferInputStream（已弃用） |          将String转换成InputStream           | 字符串，底层实现使用StringBuffer <br>作为一种数据源：将其与FilterInputStream对象相连以提供有用接口 |
|       FileInputStream        |                用于从文件中读取信息                | 缓冲区，字节将从中取出<br>作为一种数据源：将其与FilterInputStream对象相连以提供有用接口 |
|       PipedInputStream       | 产生用于写入相关PipedOutputStream的数据，实现“*管道化*”概念 | PipedOutputStream<br>作为多线程中的数据源：将其与FilterInputStream对象相连以提供有用接口 |
|     SequenceInputeStream     |        将两个或者多个InputStream对象转化成单个         | 两个InputStream对象或者一个容纳InputStream对象容器的Enumeration<br>作为一种数据源：将其与FilterInputStream对象相连以提供有用接口 |
|      FilterInputStream       | 抽象类，作为“装饰器”的接口。其中“装饰器”为其他的InputStream类提供有用的功能 |           见FilterInputStream表            |

3. OutputStream类型

|           类           |                    功能                    |               构造器参数<br>使用                |
| :-------------------: | :--------------------------------------: | :--------------------------------------: |
| ByteArrayOutputStream |     在内存中创建一个缓冲区。所有送往“流”的数据都要放置在此缓冲区中     | 缓冲区初始尺寸<br>用于指定数据的目的地，将其与FilterOutputStream对象相连以提供有用的接口 |
|   FileOutputStream    |                用于将信息写至文件                 | 字符串，表示文件名，文件或者FileDescriptor对象<br>用于指定数据的目的地，将其与FilterOutputStream对象相连以提供有用的接口 |
|   PipedOutputStream   | 任何写入其中的信息都会自动作为相关的PipedInputStream的输出，实现“*管道化*”的概念 | PipedInputStream<br>用于指定数据的目的地，将其与FilterOutputStream对象相连以提供有用的接口 |
|  FilterOutputStream   | 抽象类，作为“装饰器”的接口。其中“装饰器”为其他的OutputStream类提供有用的功能 |           见FilterOutputStream表           |

4. FilterInputStream 类型

|             类              |                    功能                    |               构造器参数<br>使用                |
| :------------------------: | :--------------------------------------: | :--------------------------------------: |
|      DataInputStream       | 与DataOutputStream搭配使用，因此我们可以按照可移植方式从流读取基本数据类型 |      InputStream<br>包含由于读取基本数据的全部接口      |
|    BufferedInputStream     |    使用它可以防止每次读区时都得进行实际写操作。代表“*使用缓冲区*”     | InputStream，可以指定缓冲区大小（可选）<br>本质上不提供接口，只不过是向进程中添加缓冲区所必需的。与接口对象搭配 |
| LineNumberInputStream（已弃用） | 跟踪输入流的行号，可调用getLineNumber和setLineNumber  |   InputStream<br>仅增加行号，因此可能要与接口对象搭配使用    |
|    PushbackInputStream     |    具有“*能弹出一个字节的缓冲区*”。因此可以将读到的最后一个字符回退    | InputStream<br>通常作为编译器的扫描器，之所以包含在内是因为Java编译器的需要，我们可能永远不会用到 |

5. FilterOutputStream类型

|          类           |                    功能                    |               构造器参数<br>使用                |
| :------------------: | :--------------------------------------: | :--------------------------------------: |
|   DataOutputStream   | 与DataInputStream搭配使用，因此可以按照可移植方式向流中写入基本数据类型 |     OutputStream<br>包含用于写入基本数据的全部接口      |
|     PrintStream      | 用于产生格式化输出。其中DataOutputStream处理数据的存储，PrintStream处理显示 | OutputStream，可以用boolean值来指示是否在每次换行的时候清空缓冲区（可选）应该是对OutputSteam对象的“final”封装，可能会经常使用它 |
| BufferedOutputStream | 使用它以避免每次发送数据时都要进行实际的写操作。代表“*使用缓冲区*”。可以调用flush清空缓冲区 | OutputStream，可以指定缓冲区大小（可选）<br>本质上并不提供接口，只不过是向进程中添加缓冲区所必需的。与接口对象搭配 |

6. **所以一般的搭配都是FilterInputStream(InputStream),或者FilterOutputStream(OutputStream)**

# Reader和Writer

1. InputStreamReader可以吧InputStream转化成Reader，OutputStreamWriter可以把OutputStream转化成Writer
2. 尽量尝试使用Reader和Writer，无法编译的时候使用面向字节的类库
3. 无论何时我们使用readLine，都不应该使用DataInputStream，而应该使用BufferedReader，除此之外，DataInputStream仍是I/O类库的首选成员

# RandomAccessFile

1. 只有RandomAccessFile支持搜寻方法，并且只适用于文件
2. 适用于大小已知的记录组成文件，构造器需要指定“*随机读（r）*”还是“*既读又写（rw）*”，不支持只写文件
3. 考虑使用“*内存映射文件*”来替代RandomAccessFile

# I/O流的典型实用方式

1. 缓冲输入文件

```java
public static  String read(String fileName) throws IOException {
   BufferedReader in = new BufferedReader(new FileReader(fileName));
   String s;
   StringBuilder sb = new StringBuilder();
   while((s = in.readLine())!=null){
      sb.append(s+"\n");
   }
   in.close();
   return sb.toString();
}
public static void main(String[] args) throws IOException {
   System.out.println(read("/Users/limbo/Documents/Project/Java/HomeWork/src/cn/limbo/homework/Home0.java"));
}
```
2.从内存中输入

```java
public static String read(String fileName) throws IOException {
   BufferedReader in = new BufferedReader(new FileReader(fileName));
   String s;
   StringBuilder sb = new StringBuilder();
   while ((s = in.readLine()) != null) {
      sb.append(s + "\n");
   }
   in.close();
   return sb.toString();
}

public static void main(String[] args) throws IOException {
   StringReader in = new StringReader(read("/Users/limbo/Documents/Project/Java/HomeWork/src/cn/limbo/homework/Home0.java"));
   int c;
   while ((c = in.read()) != -1)
      System.out.println((char)c);
}
```

3. 格式化的内存输入(其中的read方法沿用上面的)

- 第一种写法（使用异常来控制文件的末尾）

```Java
public static void main(String[] args) throws IOException {
   try{
      DataInputStream in = new DataInputStream(new ByteArrayInputStream(read(
            "/Users/limbo/Documents/Project/Java/HomeWork/src/cn/limbo/homework/Home0.java"
      ).getBytes()));
      while(true){
         System.out.print((char)in.readByte());
      }
   }catch (EOFException e){
      System.err.println("End of stream");
   }
}
```

- 第二种写法（使用available方法控制文件末尾）

```java
public static void main(String[] args) throws IOException {
   DataInputStream in = new DataInputStream(new ByteArrayInputStream(read("/Users/limbo/Documents/Project/Java/HomeWork/src/cn/limbo/homework/Home0.java").getBytes()));
   while(in.available()!=0){
      System.out.print((char)in.readByte());
   }
}
```

- 建议使用available方法来控制文件末尾的问题，但是available方法的工作方式会随着所读取的媒介类型的不同而有所不同，字面的意思就是“*在没有阻塞的情况下所能读取的字节数*”，对于文件，这意味着整个文件，对于不同类型的流，可能就不是这样，因此要谨慎使用。我们也可以通过异常来检测输入的末尾，但是使用异常进行流控制，被认为是对异常特性的错误使用。

4. 基本文件输出

```java
static String file = "BasicFileOutput.txt";
public static String read(String fileName) throws IOException {
   BufferedReader in = new BufferedReader(new FileReader(fileName));
   String s;
   StringBuilder sb = new StringBuilder();
   while ((s = in.readLine()) != null) {
      sb.append(s + "\n");
   }
   in.close();
   return sb.toString();
}
public static void main(String[] args) throws IOException {
   BufferedReader in = new BufferedReader(new StringReader(read("/Users/limbo/Documents/Project/Java/HomeWork/src/cn/limbo/homework/Home0.java")));
   //这里也可以使用PrintWriter out = new PrintWriter(file);进行简化开发
   PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter(file)));
   int lineCount = 1;
   String s;
   while((s = in.readLine())!=null)
      out.println(lineCount++ +": " + s);
   out.close();//如果不关闭的话，无法将缓冲区中的刷新清空，那也就不会写入
}
```

5. 存储和恢复数据

- 如果使用DataOutputStream写入数据，Java保证我们可以使用DataInputStream准确地读取数据

