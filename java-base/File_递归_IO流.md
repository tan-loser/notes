[TOC]

1、目前是怎么样存储数据的？弊端是什么

- 在内存中存储的数据是用来处理、修改、运算的，不能长久保存。

2、计算机中，有没有一块硬件可以永久存储数据的？

- 磁盘中数据的形式就是文件，文件是数据的载体。

**思路：**

1、先要定位文件

- File类可以定位文件：进行删除、获取文本本身信息等操作。
- 但是不能读写文件内容。

2、读写文件数据

- IO流技术可以对硬盘中的文件进行读写

3、今日总体学习思路

- 先学会使用File类定位文件以及操作文件本身
- 然后学习IO流读写文件数据。

# File类概述

- File类在包java.io.File下、代表操作系统的文件对象（文件、文件夹）。
- File类提供了诸如：定位文件，获取文件本身的信息、删除文件、创建文件（文件夹）等功能。
- **但是不能读写文件内容。**

**File类创建对象**

| 方法名称                                     | 说明                                               |
| -------------------------------------------- | -------------------------------------------------- |
| **public** File(String pathname)             | 根据文件路径创建文件对象                           |
| **public** File(String parent, String child) | 从父路径名字符串和子路径名字符串创建文件对象       |
| **public** File(File parent, String child)   | 根据父路径对应文件对象和子路径名字符串创建文件对象 |

- File对象可以定位文件和文件夹
- File封装的对象仅仅是一个路径名，这个路径可以是存在的，也可以是不存在的。

**绝对路径和相对路径**

- 绝对路径：从盘符开始

  ```
  File file1 = new File(“D:\\itheima\\a.txt”);
  ```

- 相对路径：不带盘符，默认直接到当前工程下的目录寻找文件。

  ```
  File file3 = new File(“模块名\\a.txt”); 
  ```

```java
public static void main(String[] args) {
        // 1、创建File对象（指定了文件的路径）
        // 路径写法： D:\resources\xueshan.jpeg
        //          D:/resources/xueshan.jpeg
        //          File.separator
    
    //方法一：
        File f = new File("D:\\resources\\xueshan.jpeg");
    //方法二：
        File f = new File("D:/resources/xueshan.jpeg");
    //方法三：
        File f = new File("D:" + File.separator+"resources"+ File.separator +"xueshan.jpeg");
    
        long size = f.length(); // 是文件的字节大小
        System.out.println(size);

        // 2、File创建对象，支持绝对路径 支持相对路径（重点）
        File f1 = new File("D:\\resources\\beauty.jpeg"); // 绝对路径
        System.out.println(f1.length());

        // 相对路径：一般定位模块中的文件的。 相对到工程下！！
        File f2 = new File("file-io-app/src/data.txt");
        System.out.println(f2.length());

        // 3、File创建对象 ，可以是文件也可以是文件夹
        File f3 = new File("D:\\resources");
        System.out.println(f3.exists()); // 判断这个路径是否存在，这个文件夹存在否

    }
```







## File类的常用API

**File类的判断文件类型、获取文件信息功能**

| 方法名称                        | 说明                                       |
| ------------------------------- | ------------------------------------------ |
| public boolean isDirectory()    | 测试此抽象路径名表示的File是否为文件夹     |
| public boolean isFile()         | 测试此抽象路径名表示的File是否为文件       |
| public boolean exists()         | 测试此抽象路径名表示的File是否存在         |
| public String getAbsolutePath() | 返回此抽象路径名的绝对路径名字符串         |
| public String getPath()         | 将此抽象路径名转换为路径名字符串           |
| public String getName()         | 返回由此抽象路径名表示的文件或文件夹的名称 |
| public long lastModified()      | 返回文件最后修改的时间毫秒值               |

```java
File f2 = new File("file-io-app\\src\\data.txt");
        // a.获取它的绝对路径。
        System.out.println(f2.getAbsolutePath());
        // b.获取文件定义的时候使用的路径。
        System.out.println(f2.getPath());
        // c.获取文件的名称：带后缀。
        System.out.println(f2.getName());
        // d.获取文件的大小：字节个数。
        System.out.println(f2.length()); // 字节大小
        // e.获取文件的最后修改时间
        long time1 = f2.lastModified();
        System.out.println("最后修改时间：" + new SimpleDateFormat("yyyy/MM/dd HH:mm:ss").format(time1));
        // f、判断文件是文件还是文件夹
        System.out.println(f2.isFile()); // true
        System.out.println(f2.isDirectory()); // false
        System.out.println(f2.exists()); // true
```



**创建文件、删除文件功能**

File类创建文件的功能

| 方法名称                       | 说明                 |
| ------------------------------ | -------------------- |
| public boolean createNewFile() | 创建一个新的空的文件 |
| public boolean mkdir()         | 只能创建一级文件夹   |
| public boolean mkdirs()        | 可以创建多级文件夹   |

File类删除文件的功能

| 方法名称                | 说明                                   |
| ----------------------- | -------------------------------------- |
| public boolean delete() | 删除由此抽象路径名表示的文件或空文件夹 |

- delete方法默认只能删除文件和空文件夹。默认不能删除非空文件夹。
- delete方法直接删除不走回收站

```java
public static void main(String[] args) throws IOException {
        File f = new File("file-io-app\\src\\data.txt");
        // a.创建新文件，创建成功返回true ,反之 ,不需要这个，以后文件写出去的时候都会自动创建
        System.out.println(f.createNewFile());
        File f1 = new File("file-io-app\\src\\data02.txt");
        System.out.println(f1.createNewFile()); // （几乎不用的，因为以后文件都是自动创建的！）

        // b.mkdir创建一级目录
        File f2 = new File("D:/resources/aaa");
        System.out.println(f2.mkdir());

        // c.mkdirs创建多级目录(重点)
        File f3 = new File("D:/resources/ccc/ddd/eee/ffff");
//        System.out.println(f3.mkdir());
        System.out.println(f3.mkdirs()); // 支持多级创建

        // d.删除文件或者空文件夹
        System.out.println(f1.delete());
        File f4 = new File("D:/resources/xueshan.jpeg");
        System.out.println(f4.delete()); 
    
    	// 占用一样可以删除,当其它应用在用的文件也可以删除
        // 只能删除空文件夹,不能删除非空文件夹.
        File f5 = new File("D:/resources/aaa");
        System.out.println(f5.delete());
    }
```





**遍历文件夹**

File类的遍历功能

| 方法名称                        | 说明                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| public String[] list()          | 获取当前目录下所有的"一级文件名称"到一个字符串数组中去返回。 |
| public File[] listFiles()(常用) | 获取当前目录下所有的"一级文件对象"到一个文件对象数组中去返回（重点） |

listFiles方法注意事项：

1. 当调用者不存在时，返回null
2. 当调用者是一个文件时，返回null
3. 当调用者是一个空文件夹时，返回一个长度为0的数组
4. 当调用者是一个有内容的文件夹时，将里面所有文件和文件夹的路径放在File数组中返回
5. 当调用者是一个有隐藏文件的文件夹时，将里面所有文件和文件夹的路径放在File数组中返回，包含隐藏内容
6. 当调用者是一个需要权限才能进入的文件夹时，返回null

```java
// 1、定位一个目录
File f1 = new File("D:/resources");
String[] names = f1.list();
for (String name : names) {
    System.out.println(name);
}

// 2.一级文件对象
// 获取当前目录下所有的"一级文件对象"到一个文件对象数组中去返回（重点）
File[] files = f1.listFiles();
for (File f : files) {
    System.out.println(f.getAbsolutePath());
}

// 注意事项
File dir = new File("D:/resources/ddd");
File[] files1 = dir.listFiles();
System.out.println(Arrays.toString(files1));
```







# 方法递归的应用



**文件搜索**

分析：

1. 先定位出的应该是一级文件对象

2. 遍历全部一级文件对象，判断是否是文件

3. 如果是文件，判断是否是自己想要的

4. 如果是文件夹，需要继续递归进去重复上述过程

   ```java
   public static void main(String[] args) {
           // 2、传入目录 和  文件名称
           searchFile(new File("D:/") , "eDiary.exe");
       }
   
       /**
        * 1、搜索某个目录下的全部文件，找到我们想要的文件。
        * @param dir  被搜索的源目录
        * @param fileName 被搜索的文件名称
        */
       public static void searchFile(File dir,String fileName){
           // 3、判断dir是否是目录
           if(dir != null && dir.isDirectory()){
               // 可以找了
               // 4、提取当前目录下的一级文件对象
               File[] files = dir.listFiles(); // null  []
               // 5、判断是否存在一级文件对象，存在才可以遍历
               if(files != null && files.length > 0) {
                   for (File file : files) {
                       // 6、判断当前遍历的一级文件对象是文件 还是 目录
                       if(file.isFile()){
                           // 7、是不是咱们要找的，是把其路径输出即可
                           if(file.getName().contains(fileName)){
                               System.out.println("找到了：" + file.getAbsolutePath());
                               // 启动它。
                               try {
                                   Runtime r = Runtime.getRuntime();
                                   r.exec(file.getAbsolutePath());
                               } catch (IOException e) {
                                   e.printStackTrace();
                               }
                           }
                       }else {
                           // 8、是文件夹，需要继续递归寻找
                           searchFile(file, fileName);
                       }
                   }
               }
           }else {
               System.out.println("对不起，当前搜索的位置不是文件夹！");
           }
       }
   ```

   

**删除非空文件夹**

分析：

1. File默认不可以删除非空文件夹

2. 我们需要遍历文件夹，先删除里面的内容，再删除自己。

   ```java
   public static void main(String[] args) {
       deleteDir(new File("D:/new"));
   }
   
   /**
      删除文件夹，无所谓里面是否有内容，都可以删除
    * @param dir
    */
   public static void deleteDir(File dir){
       // 1、判断dir存在且是文件夹
       if(dir != null && dir.exists() && dir.isDirectory()){
           // 2、提取一级文件对象。
           File[] files = dir.listFiles();
           // 3、判断是否存在一级文件对象，存在则遍历全部的一级文件对象去删除
           if(files != null && files.length > 0){
               // 里面有内容
               for (File file : files) {
                   // 4、判断file是文件还是文件夹，文件直接删除
                   if(file.isFile()){
                       file.delete();
                   }else {
                       // 递归删除
                       deleteDir(file);
                   }
               }
           }
           // 删除自己
           dir.delete();
       }
   }
   ```

   

**拷贝文件**

将某个磁盘的文件夹拷贝到另一个文件夹下去，包括文件夹中的全部信息

分析：

1. IO默认不可以拷贝文件夹
2. 我们需要遍历文件夹，如果是文件则拷贝过去，如果是文件夹则要进行1-1创建，再递归。





# 字符集

## 常见字符集介绍

计算机底层可以表示十进制编号。计算机可以给人类字符进行编号存储，这套编号规则就是**字符集**

**SCII字符集：**

- ASCII(American Standard Code for Information Interchange，美国信息交换标准代码)：包括了数字、英文、符号。
-  ASCII使用1个字节存储一个字符，一个字节是8位，总共可以表示128个字符信息，对于英文，数字来说是够用的。

```
01100001‬ = 97  => a
‭01100010‬ = 98  => b
```

**GBK：**

- window系统默认的码表。兼容ASCII码表，也包含了几万个汉字，并支持繁体汉字以及部分日韩文字。
- **注意**：GBK是中国的码表，**一个中文以两个字节的形式存储**。但不包含世界上所有国家的文字。

**Unicode码表：**

-  unicode（又称统一码、万国码、单一码）是计算机科学领域里的一项业界字符编码标准。
- 容纳世界上大多数国家的所有常见文字和符号。
- 由于Unicode会先通过UTF-8，UTF-16，以及 UTF-32的编码成二进制后再存储到计算机，其中最为常见的就是UTF-8。

**注意：**

1. Unicode是万国码，以UTF-8编码后一个**中文一般以三个字节的形式存储。**
2. UTF-8也要兼容ASCII编码表。
3. 技术人员都应该使用UTF-8的字符集编码。
4. 编码前和编码后的字符集需要一致，否则会出现中文乱码。

<img src="image\utf-8.png" alt="汉字存储和展示过程解析" />

1. 字符串常见的字符底层组成是什么样的？

- 英文和数字等在任何国家的字符集中都占1个字节
- GBK字符中一个中文字符占2个字节
- UTF-8编码中一个中文1般占3个字节

2. 编码前的字符集和编码好的字符集有什么要求？

- 必须一致，否则会出现中文字符乱码
- 英文和数字在任何国家的编码中都不会乱码



## 字符集的编码、解码操作

**String编码**

| 方法名称                            | 说明                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| byte[] getBytes()                   | 使用平台的默认字符集将该 String编码为一系列字节，将结果存储到新的字节数组中 |
| byte[] getBytes(String charsetName) | 使用指定的字符集将该 String编码为一系列字节，将结果存储到新的字节数组中 |

String解码

| 构造器           | 说明               |
| -------- | ----------------------- |
| String(byte[] bytes)  | 通过使用平台的默认字符集解码指定的字节数组来构造新的 String |
| String(byte[] bytes, String charsetName) | 通过指定的字符集解码指定的字节数组来构造新的 String         |

```java
public static void main(String[] args) throws Exception {
    // 1、编码：把文字转换成字节（使用指定的编码）
    String name = "abc我爱你中国";
    
    // byte[] bytes = name.getBytes(); // 以当前代码默认字符集进行编码 （UTF-8）
    byte[] bytes = name.getBytes("GBK"); // 指定编码
    
    System.out.println(bytes.length);
    System.out.println(Arrays.toString(bytes));

    // 2、解码：把字节转换成对应的中文形式（编码前 和 编码后的字符集必须一致，否则乱码 ）
    // String rs = new String(bytes); // 默认的UTF-8
    String rs = new String(bytes, "GBK"); // 指定GBK解码
    
    System.out.println(rs);
}
```







# IO流概述

IO流也称为输入、输出流，就是用来读写数据的。

IO流概述

- I表示intput，是数据从硬盘文件读入到内存的过程，称之输入，负责读。
- O表示output，是内存程序的数据从内存到写出到硬盘文件的过程，称之输出，负责写。

<img src="D:\桌面文件\笔记\notes\java-base\image\IO分类.png" alt="IO流分类" style="zoom:90%;" />

**总结流的四大类:**

- 字节输入流：以内存为基准，来自磁盘文件/网络中的数据以字节的形式读入到内存中去的流称为字节输入流。
- 字节输出流：以内存为基准，把内存中的数据以字节写出到磁盘文件或者网络中去的流称为字节输出流。
- 字符输入流：以内存为基准，来自磁盘文件/网络中的数据以字符的形式读入到内存中去的流称为字符输入流。
- 字符输出流：以内存为基准，把内存中的数据以字符写出到磁盘文件或者网络介质中去的流称为字符输出流。



# 字节流的使用

![IO流体系](image\IO体系.png)

```
IO流的体系：
             字节流                                   字符流
    字节输入流            字节输出流               字符输入流        字符输出流
    InputStream          OutputStream           Reader           Writer  (抽象类)
    FileInputStream      FileOutputStream       FileReader       FileWriter(实现类，可以使用的)
```

## 文件字节输入流

1. **每次读取一个字节**

   文件字节输入流：FileInputStream

   作用：以内存为基准，把磁盘文件中的数据以字节的形式读取到内存中去。

   | 构造器                                  | 说明                               |
   | --------------------------------------- | ---------------------------------- |
   | public FileInputStream(File file)       | 创建字节输入流管道与源文件对象接通 |
   | public FileInputStream(String pathname) | 创建字节输入流管道与源文件路径接通 |

   | 方法名称                       | 说明                                                   |
   | ------------------------------ | ------------------------------------------------------ |
   | public int read()              | 每次读取一个字节返回，如果字节已经没有可读的返回-1     |
   | public int read(byte[] buffer) | 每次读取一个字节数组返回，如果字节已经没有可读的返回-1 |

   **每次读取一个字节存在什么问题？**

   - 性能较慢
   - 读取中文字符输出无法避免乱码问题。

​		

```java
public static void main(String[] args) throws Exception {
        // 1、创建一个文件字节输入流管道与源文件接通。
        InputStream is = new FileInputStream(new File("file-io-app\\src\\data.txt"));
        // 简化写法
        InputStream is = new FileInputStream("file-io-app\\src\\data.txt");

        // 2、读取一个字节返回 （每次读取一滴水）
//        int b1 = is.read();
//        System.out.println((char)b1);
			....
//        int b4 = is.read(); // 读取完毕返回-1
//        System.out.println(b4);

        // 3、使用循环改进
        // 定义一个变量记录每次读取的字节    a  b  3   爱
        //                              o o  o   [ooo]
        int b;
        while (( b = is.read() ) != -1){
            System.out.print((char) b);
        }
    }
```





2. **每次读取一个字节数组**

   文件字节输入流：FileInputStream

   作用：以内存为基准，把磁盘文件中的数据以字节的形式读取到内存中去。

   | 方法名称                       | 说明                                                   |
   | ------------------------------ | ------------------------------------------------------ |
   | public int read()              | 每次读取一个字节返回，如果字节已经没有可读的返回-1     |
   | public int read(byte[] buffer) | 每次读取一个字节数组返回，如果字节已经没有可读的返回-1 |

   **每次读取一个字节数组存在什么问题？**

   - 读取的性能得到了提升
   - 读取中文字符输出无法避免乱码问题。

```java
public static void main(String[] args) throws Exception {
        // 1、创建一个文件字节输入流管道与源文件接通
        InputStream is = new FileInputStream("file-io-app/src/data02.txt");

        // 2、定义一个字节数组，用于读取字节数组
//        byte[] buffer = new byte[3]; // 3B
    
//        int len = is.read(buffer);
//        System.out.println("读取了几个字节：" + len);
//        String rs = new String(buffer);
//        System.out.println(rs);
//
//        int len1 = is.read(buffer);
//        System.out.println("读取了几个字节：" + len1);
//        String rs1 = new String(buffer);
//        System.out.println(rs1);
          // buffer = [a b c]
  
          // buffer = [a b c]  ==>  [c d c]
//        int len2 = is.read(buffer);
//        System.out.println("读取了几个字节：" + len2);
    
    注意：当新读出的数据填不满数组时，没填的位置保持原来的数据不变
    因此，因采用String的另一种有参构造器：string(数组，开始位置，结束位置)
          // 读取多少倒出多少
//        String rs2 = new String(buffer,0 ,len2);
//        System.out.println(rs2);
//
//        int len3 = is.read(buffer);
//        System.out.println(len3); // 读取完毕返回-1

        // 3、改进使用循环，每次读取一个字节数组
        byte[] buffer = new byte[3];
        int len; // 记录每次读取的字节数。
        while ((len = is.read(buffer)) != -1) {
            // 读取多少倒出多少
            System.out.print(new String(buffer, 0 , len));
        }
    }
```





3. **一次读完全部字节**

   1. 如何使用字节输入流读取中文内容输出不乱码呢？

      定义一个与文件一样大的字节数组，一次性读取完文件的全部字节。

   2. 直接把文件数据全部读取到一个字节数组可以避免乱码，是否存在问题？

      如果文件过大，字节数组可能引起内存溢出。

   **方式一**
    自己定义一个字节数组与文件的大小一样大，然后使用读取字节数组的方法，一次性读取完成

   | 方法名称                       | 说明                                                   |
   | ------------------------------ | ------------------------------------------------------ |
   | public int read(byte[] buffer) | 每次读取一个字节数组返回，如果字节已经没有可读的返回-1 |

   **方式二**
    官方为字节输入流InputStream提供了如下API可以直接把文件的全部数据读取到一个字节数组中

   | 方法名称                                        | 说明                                                         |
   | ----------------------------------------------- | ------------------------------------------------------------ |
   | public byte[] readAllBytes() throws IOException | 直接将当前字节输入流对应的文件对象的字节数据装到一个字节数组返回 |

```java
public static void main(String[] args) throws Exception {
    
    //方法一：
        // 1、创建一个文件字节输入流管道与源文件接通
        File f = new File("file-io-app/src/data03.txt");
        InputStream is = new FileInputStream(f);

        // 2、定义一个字节数组与文件的大小刚刚一样大。
//        byte[] buffer = new byte[(int) f.length()];
//        int len = is.read(buffer);
//        System.out.println("读取了多少个字节：" + len);
//        System.out.println("文件大小：" + f.length());
//        System.out.println(new String(buffer));

    //方法二：
        // 读取全部字节数组
        byte[] buffer = is.readAllBytes();
        System.out.println(new String(buffer));

    }
```





## 文件字节输出流

1. 写字节数据到文件

   文件字节输出流：FileOutputStream
   作用：以内存为基准，把内存中的数据以字节的形式写出到磁盘文件中去的流。

   | 构造器                                                   | 说明                                           |
   | -------------------------------------------------------- | ---------------------------------------------- |
   | public FileOutputStream(File file)                       | 创建字节输出流管道与源文件对象接通             |
   | public FileOutputStream(File file，boolean append)       | 创建字节输出流管道与源文件对象接通，可追加数据 |
   | public FileOutputStream(String filepath)                 | 创建字节输出流管道与源文件路径接通             |
   | public FileOutputStream(String filepath，boolean append) | 创建字节输出流管道与源文件路径接通，可追加数据 |

   **文件字节输出流（FileOutputStream）写数据出去的API**

   | 方法名称                                             | 说明                         |
   | ---------------------------------------------------- | ---------------------------- |
   | public void write(int a)                             | 写一个字节出去               |
   | public void write(byte[] buffer)                     | 写一个字节数组出去           |
   | public void write(byte[] buffer , int pos , int len) | 写一个字节数组的一部分出去。 |

   **流的关闭与刷新**

   | 方法    | 说明                                                         |
   | ------- | ------------------------------------------------------------ |
   | flush() | 刷新流，还可以继续写数据                                     |
   | close() | 关闭流，释放资源，但是在关闭之前会先刷新流。一旦关闭，就不能再写数据 |

   1. **字节输出流如何实现数据追加更多操作**

      public FileOutputStream(String filepath，boolean append)

      创建字节输出流管道与源文件路径接通，可追加数据

   2. **字节输出流如何实现写出去的数据能换行**
      - os.write(“\r\n”.getBytes())

   3. **如何让写出去的数据能成功生效？**
      - flush()刷新数据
      - close()方法是关闭流，关闭包含刷新，关闭后流不可以继续使用了。

   ```java
   public static void main(String[] args) throws Exception {
       // 1、创建一个文件字节输出流管道与目标文件接通
       OutputStream os = new FileOutputStream("file-io-app/src/out04.txt" , true); // 追加数据管道
       //OutputStream os = new FileOutputStream("file-io-app/src/out04.txt"); // 先清空之前的数据，写新数据进入
   
       // 2、写数据出去
       // a.public void write(int a):写一个字节出去
       os.write('a');
       os.write(98);
       os.write("\r\n".getBytes()); // 换行
       // os.write('徐'); // [ooo]不能写中文，中文不止一个字节
   
       // b.public void write(byte[] buffer):写一个字节数组出去。
       byte[] buffer = {'a' , 97, 98, 99};
       os.write(buffer);
       os.write("\r\n".getBytes()); // 换行
   
       byte[] buffer2 = "我是中国人".getBytes();
       //byte[] buffer2 = "我是中国人".getBytes("GBK");
       //IDEA默认采用UTF-8编码，若想采用其它格式的字符编码可以用如上方法
       os.write(buffer2);
       os.write("\r\n".getBytes()); // 换行
   
   
       // c. public void write(byte[] buffer , int pos , int len):写一个字节数组的一部分出去。
       byte[] buffer3 = {'a',97, 98, 99};
       os.write(buffer3, 0 , 3);
       os.write("\r\n".getBytes()); // 换行
   
       // os.flush(); // 写数据必须，刷新数据 可以继续使用流
       os.close(); // 释放资源，包含了刷新的！关闭后流不可以使用了
   }
   ```

   

2. 文件拷贝

   ![文件字节复制流程](D:\桌面文件\笔记\notes\java-base\image\copy_byte_process.png)

   **需求：**

   - 把某个视频复制到其他目录下的“b.avi”

   **思路：**

   1. 根据数据源创建字节输入流对象
   2. 根据目的地创建字节输出流对象
   3. 读写数据，复制视频
   4. 释放资源

   

   **字节流适合做一切文件数据的拷贝吗？**
   任何文件的底层都是字节，拷贝是一字不漏的转移字节，只要前后文件格式、编码一致没有任何问题。

```java
/**
 *   目标：学会使用字节流完成文件的复制（支持一切文件类型的复制）
 */
public static void main(String[] args) {
    try {
        // 1、创建一个字节输入流管道与原视频接通
        InputStream is = new FileInputStream("file-io-app/src/out04.txt");

        // 2、创建一个字节输出流管道与目标文件接通
        OutputStream os = new FileOutputStream("file-io-app/src/out05.txt");

        // 3、定义一个字节数组转移数据
        byte[] buffer = new byte[1024];
        int len; // 记录每次读取的字节数。
        while ((len = is.read(buffer)) != -1){
            os.write(buffer, 0 , len);
        }
        System.out.println("复制完成了！");

        // 4、关闭流。
        os.close();
        is.close();
    } catch (Exception e){
        e.printStackTrace();
    }
}
```





# 资源释放的方式

## try-catch-finally

- finally：在异常处理时提供finally块来执行所有清除操作，比如说IO流中的释放资源
- 特点：被finally控制的语句最终**一定会执行，除非JVM退出**( 即System.exit(0);)
- 异常处理标准格式：try….catch…finally

**try-catch-finally格式**

```
try {
	FileOutputStream fos = new FileOutputStream("a.txt");
	fos.write(97); 
} catch (IOException e) {
	e.printStackTrace();
}finally{

}
```

```java
public static int test(int a , int b){
    try {
        int c = a / b;
        return c;
    }catch (Exception e){
        e.printStackTrace();
        return -111111; // 计算出现bug.
    }finally {
        System.out.println("--finally--");
        // 哪怕上面有return语句执行，也必须先执行完这里才可以！
        // 开发中不建议在这里加return ，如果加了，返回的永远是这里的数据了，这样会出问题！
        return 100;
    }
}
```

**采用如上格式关闭流：**

因为 try 和 finally 是两个代码块，在 try 中创建的流不能在 finally 中起作用，所以要定义在外面

要判断文件指针是否为空，所以还要 try-catch-finally，

```
InputStream is = null ;
OutputStream os = null;
try{
	...
}catch (Exception e){
	e.printStackTrace();
} finally {
    // 关闭资源！
    try {
    	if(os != null) os.close();
    } catch (Exception e) {
    	e.printStackTrace();
    }
    try {
    	if(is != null) is.close();
    } catch (Exception e) {
    	e.printStackTrace();
    }
}

```

```java
InputStream is = null;
OutputStream os = null;
try {

    // System.out.println(10/ 0);

    // 1、创建一个字节输入流管道与原视频接通
    is = new FileInputStream("file-io-app/src/out04.txt");

    // 2、创建一个字节输出流管道与目标文件接通
    os = new FileOutputStream("file-io-app/src/out05.txt");

    // 3、定义一个字节数组转移数据
    byte[] buffer = new byte[1024];
    int len; // 记录每次读取的字节数。
    while ((len = is.read(buffer)) != -1){
        os.write(buffer, 0 , len);
    }
    System.out.println("复制完成了！");

    //   System.out.println( 10 / 0);

} catch (Exception e){
    e.printStackTrace();
} finally {
    // 无论代码是正常结束，还是出现异常都要最后执行这里
    System.out.println("========finally=========");
    
    try {
        // 4、关闭流。
        if(os!=null)os.close();
    } catch (IOException e) {
        e.printStackTrace();
    }
    
    try {
        if(is != null) is.close();
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```



## try-with-resource

1. finally虽然可以用于释放资源，但是释放资源的代码过于繁琐？

2. 有没有办法简化？

   

**JDK 7和JDK9中都简化了资源释放操作**

**基本做法**：手动释放资源

```java
try{
	可能出现异常的代码;
}catch(异常类名 变量名){
	异常的处理代码;
}finally{
	执行所有资源释放操作;
}
```

**JDK7改进方案**： 资源用完最终自动释放

```java
try(定义流对象){
	可能出现异常的代码;
}catch(异常类名 变量名){
	异常的处理代码;
} 
```

```java
public static void main(String[] args) {

    try (
        // 这里面只能放置资源对象，用完会自动关闭：自动调用资源对象的close方法关闭资源（即使出现异常也会做关闭操作）
        // 1、创建一个字节输入流管道与原视频接通
        InputStream is = new FileInputStream("file-io-app/src/out04.txt");
        // 2、创建一个字节输出流管道与目标文件接通
        OutputStream os = new FileOutputStream("file-io-app/src/out05.txt");

        // int age = 23; // 这里只能放资源
        //这是自己创建的资源，代码在下面
        MyConnection connection = new MyConnection(); // 最终会自动调用资源的close方法
    ) {

        // 3、定义一个字节数组转移数据
        byte[] buffer = new byte[1024];
        int len; // 记录每次读取的字节数。
        while ((len = is.read(buffer)) != -1){
            os.write(buffer, 0 , len);
        }
        System.out.println("复制完成了！");

    } catch (Exception e){
        e.printStackTrace();
    }

}
```

```java
//自定义资源，下面有简单的介绍
class MyConnection implements AutoCloseable{
    @Override
    public void close() throws IOException {
        System.out.println("连接资源被成功释放了！");
    }
}
```





**JDK9改进方案：**资源用完最终自动释放

```
定义输入流对象;
定义输出流对象;
try(输入流对象；输出流对象){
	可能出现异常的代码;
}catch(异常类名 变量名){
	异常的处理代码;
}
```

```java
public static void main(String[] args) throws Exception {

    // 这里面只能放置资源对象，用完会自动关闭：自动调用资源对象的close方法关闭资源（即使出现异常也会做关闭操作）
    
    // 1、创建一个字节输入流管道与原视频接通
    InputStream is = new FileInputStream("file-io-app/src/out04.txt");
    // 2、创建一个字节输出流管道与目标文件接通
    OutputStream os = new FileOutputStream("file-io-app/src/out05.txt");
    
    try ( is ; os ) {
        
        // 3、定义一个字节数组转移数据
        byte[] buffer = new byte[1024];
        int len; // 记录每次读取的字节数。
        while ((len = is.read(buffer)) != -1){
            os.write(buffer, 0 , len);
        }
        System.out.println("复制完成了！");

    } catch (Exception e){
        e.printStackTrace();
    }
}
```





**注意**

- JDK 7 以及 JDK 9的 try(  ) 的括号中只能放置资源对象，否则报错
- 什么是资源呢？
- 资源都是实现了Closeable/AutoCloseable接口的类对象

`public abstract class InputStream implements Closeable {}`

`public abstract class OutputStream implements Closeable, Flushable{}`

**需求：**

- 将某个磁盘的文件夹拷贝到另一个文件夹下去，包括文件夹中的全部信息

**分析：**

1. IO默认不可以拷贝文件夹
2. 我们需要遍历文件夹，如果是文件则拷贝过去，如果是文件夹则要进行1-1创建，继续复制内容。

```java
public static void main(String[] args) {
    // D:\resources
    copy(new File("D:\\resources") , new File("D:\\new"));
}

public static void copy(File src , File dest){
    // 1、判断源目录是否存在
    if(src!= null && src.exists() && src.isDirectory()){
        // 2、目标目录需要创建一下  D:\new\resources
        File destOne = new File(dest , src.getName());
        destOne.mkdirs();

        // 3、提取原目录下的全部一级文件对象
        File[] files = src.listFiles();
        // 4、判断是否存在一级文件对象
        if(files != null && files.length > 0) {
            // 5、遍历一级文件对象
            for (File file : files) {
                // 6、判断是文件还是文件夹，是文件直接复制过去
                if(file.isFile()){
                    copyFile(file, new File(destOne , file.getName()));
                }else {
                    // 7、当前遍历的是文件夹，递归复制
                    copy(file, destOne);
                }
            }
        }

    }
}

public static void copyFile(File srcFile, File destFile){
    try (
        // 1、创建一个字节输入流管道与原视频接通
        InputStream is = new FileInputStream(srcFile);
        // 2、创建一个字节输出流管道与目标文件接通
        OutputStream os = new FileOutputStream(destFile);
    ) {
        // 3、定义一个字节数组转移数据
        byte[] buffer = new byte[1024];
        int len; // 记录每次读取的字节数。
        while ((len = is.read(buffer)) != -1){
            os.write(buffer, 0 , len);
        }
    } catch (Exception e){
        e.printStackTrace();
    }
}
```





# 字符流的使用

1. 字节流读取中文输出会存在什么问题？
   - 会乱码。或者内存溢出。

2. 读取中文输出，哪个流更合适，为什么？
   - 字符流更合适，最小单位是按照单个字符读取的。

## 文件字符输入流

1. 一次读取一个字符

   文件字符输入流：Reader

   作用：以内存为基准，把磁盘文件中的数据以字符的形式读取到内存中去。

   | 构造器                             | 说明                               |
   | ---------------------------------- | ---------------------------------- |
   | public FileReader(File file)       | 创建字符输入流管道与源文件对象接通 |
   | public FileReader(String pathname) | 创建字符输入流管道与源文件路径接通 |

   | 方法名称                       | 说明                                                         |
   | ------------------------------ | ------------------------------------------------------------ |
   | public int read()              | 每次读取一个字符返回，如果字符已经没有可读的返回-1           |
   | public int read(char[] buffer) | 每次读取一个字符数组，返回读取的字符个数，如果字符已经没有可读的返回-1 |

   **字符流的好处。每次读取一个字符存在什么问题？**

   - 读取中文字符不会出现乱码（如果代码文件编码一致）
   - 性能较慢

```java
public static void main(String[] args) throws Exception {
    // 目标：每次读取一个字符。
    // 1、创建一个字符输入流管道与源文件接通
    Reader fr = new FileReader("file-io-app\\src\\data06.txt");

    // 2、读取一个字符返回，没有可读的字符了返回-1
    // int code = fr.read();
    // System.out.print((char)code);
    //
    // int code1 = fr.read();
    // System.out.print((char)code1);

    // 3、使用循环读取字符
    int code;
    while ((code = fr.read()) != -1){
        System.out.print((char) code);
    }
}
```





2. 一次读取一个字符数组

   文件字符输入流：FileReader

   作用：以内存为基准，把磁盘文件中的数据以字符的形式读取到内存中去。

   | 方法名称                       | 说明                                                         |
   | ------------------------------ | ------------------------------------------------------------ |
   | public int read()              | 每次读取一个字符返回，如果字符已经没有可读的返回-1           |
   | public int read(char[] buffer) | 每次读取一个字符数组，返回读取的字符数，如果字符已经没有可读的返回-1 |

   **每次读取一个字符数组的优势？**

   - 读取的性能得到了提升
   - 读取中文字符输出不会乱码。

```java
public static void main(String[] args) throws Exception {
    // 1、创建一个文件字符输入流与源文件接通
    Reader fr = new FileReader("file-io-app/src/data07.txt");

    // 2、用循环，每次读取一个字符数组的数据。  1024 + 1024 + 8
    char[] buffer = new char[1024]; // 1K字符
    int len;
    while ((len = fr.read(buffer)) != -1) {
        String rs = new String(buffer, 0, len);
        System.out.print(rs);
    }

}
```





## 文件字符输出流

文件字符输出流：FileWriter

作用：以内存为基准，把内存中的数据以字符的形式写出到磁盘文件中去的流。

| 构造器                                             | 说明                                           |
| -------------------------------------------------- | ---------------------------------------------- |
| public FileWriter(File file)                       | 创建字符输出流管道与源文件对象接通             |
| public FileWriter(File file，boolean append)       | 创建字符输出流管道与源文件对象接通，可追加数据 |
| public FileWriter(String filepath)                 | 创建字符输出流管道与源文件路径接通             |
| public FileWriter(String filepath，boolean append) | 创建字符输出流管道与源文件路径接通，可追加数据 |

**文件字符输出流（FileWriter）写数据出去的API**

| 方法名称                                  | 说明                 |
| ----------------------------------------- | -------------------- |
| void write(int c)                         | 写一个字符           |
| void write(char[] cbuf)                   | 写入一个字符数组     |
| void write(char[] cbuf, int off, int len) | 写入字符数组的一部分 |
| void write(String str)                    | 写一个字符串         |
| void write(String str, int off, int len)  | 写一个字符串的一部分 |
| void write(int c)                         | 写一个字符           |

**流的关闭与刷新** 

| 方法    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| flush() | 刷新流，还可以继续写数据                                     |
| close() | 关闭流，释放资源，但是在关闭之前会先刷新流。一旦关闭，就不能再写数据 |

1. 字符输出流如何实现数据追加更多操作

   `public FileWriter(String filepath，boolean append)`

   创建字符输出流管道与源文件路径接通，可追加数据

2. 字符输出流如何实现写出去的数据能换行

   - fw.write(“\r\n”)

3.  字符输出流如何实现写出去的数据能换行

   - flush()刷新数据

   - close()方法是关闭流，关闭包含刷新，关闭后流不可以继续使用了。

4.  字节流、字符流如何选择使用？
   - 字节流适合做一切文件数据的拷贝（音视频，文本）
   - 字节流不适合读取中文内容输出
   - 字符流适合做文本文件的操作（读，写）

```java
public static void main(String[] args) throws Exception {
    // 1、创建一个字符输出流管道与目标文件接通
    // Writer fw = new FileWriter("file-io-app/src/out08.txt"); // 覆盖管道，每次启动都会清空文件之前的数据
    Writer fw = new FileWriter("file-io-app/src/out08.txt", true); // 覆盖管道，每次启动都会清空文件之前的数据

    //a.public void write(int c):写一个字符出去
    fw.write(98);
    fw.write('a');
    fw.write('徐'); // 不会出问题了
    fw.write("\r\n"); // 换行

    //b.public void write(String c)写一个字符串出去
    fw.write("abc我是中国人");
    fw.write("\r\n"); // 换行


    //c.public void write(char[] buffer):写一个字符数组出去
    char[] chars = "abc我是中国人".toCharArray();
    fw.write(chars);
    fw.write("\r\n"); // 换行


    //d.public void write(String c ,int pos ,int len):写字符串的一部分出去
    fw.write("abc我是中国人", 0, 5);
    fw.write("\r\n"); // 换行


    //e.public void write(char[] buffer ,int pos ,int len):写字符数组的一部分出去
    fw.write(chars, 3, 5);
    fw.write("\r\n"); // 换行


    // fw.flush();// 刷新后流可以继续使用
    fw.close(); // 关闭包含刷线，关闭后流不能使用

}
```

