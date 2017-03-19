# File
1. 获取给定路径下所有文件或者文件夹，若想获得一个目录下面的所有文件，可以使用递归方式
2. 使用FilenameFilter或者FileFilter过滤文件，两者区别是方法传递的参数不同，FilenameFilter效率略高，过滤的时候一般结合正则表达式使用

# 输入和输出
1. 任何自InputStream或Reader派生的类都包含有名字为read的方法，用于读取单个字节或者字节数组，同样自OutputStream或Writer派生的类都有write方法，用于写单个字节或者字节数组

2. InputStream类型

   ​
|            类            |                    功能                    |                 构造器参数使用                  |
| :---------------------: | :--------------------------------------: | :--------------------------------------: |
|  ByteArrayInputStream   |         允许将内存的缓冲区当作InputStream使用         | 缓冲区，字节将从中取出 作为一种数据源：将其与FilterInputStream对象相连以提供有用接口 |
| StringBufferInputStream |          将String转换成InputStream           | 字符串，底层实现使用StringBuffer   作为一种数据源：将其与FilterInputStream对象相连以提供有用接口 |
|     FileInputStream     |                用于从文件中读取信息                | 缓冲区，字节将从中取出作为一种数据源：将其与FilterInputStream对象相连以提供有用接口 |
|    PipedInputStream     |  产生用于写入相关PipedOutputStream的数据，实现“管道化”概念  | PipedOutputStream   作为多线程中的数据源：将其与FilterInputStream对象相连以提供有用接口 |
|  SequenceInputeStream   |        将两个或者多个InputStream对象转化成单个         | 两个InputStream对象或者一个容纳InputStream对象容器的Enumeration  作为一种数据源：将其与FilterInputStream对象相连以提供有用接口 |
|    FilterInputStream    | 抽象类，作为“装饰器”的接口。其中“装饰器”为其他的InputStream类提供有用的功能 |           见FilterInputStream表            |

