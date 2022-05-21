[TOC]

# 创建不可变集合

**为什么要创建不可变集合？**
+ 如果某个数据不可被修改，把它防御性的拷贝到不可变即可中是个很好的实践
+ 或者当集合对象被不可信的库调用时，不可变形式是安全的

## **如何创建不可变集合？**

在List、Set、Map接口中，都存在of方法，可以创建一个不可变集合

|方法名称|说明|
|----|----|
|static <E>  List<E>  of(E…elements)	|创建一个具有指定元素的List集合对象|
|static <E>  Set<E>  of(E…elements)	|创建一个具有指定元素的Set集合对象|
|static <K , V>   Map<K，V>  of(E…elements)	|创建一个具有指定元素的Map集合对象|

这个集合不能添加，不能删除，不能修改

```java
// 1、不可变的List集合
        List<Double> lists = List.of(569.5, 700.5, 523.0,  570.5);
        double score = lists.get(1);
        
// 2、不可变的Set集合
        Set<String> names = Set.of("迪丽热巴", "迪丽热九", "马尔扎哈", "卡尔眨巴" );
        
// 3、不可变的Map集合
        Map<String, Integer> maps = Map.of("huawei",2, "Java开发", 1 , "手表", 1);
```




# Stram流

## Stream流的概述

 + 什么是Stream流？
    在Java 8中，得益于Lambda所带来的函数式编程， 引入了一个全新的Stream流概念。
    目的：用于简化集合和数组操作的API。
  
 + Stream流式思想的核心：
     - 先得到集合或者数组的Stream流（就是一根传送带）
     -  把元素放上去
     -  然后就用这个Stream流简化的API来方便的操作元素。

## stream流的获取

 + Stream流的三类方法
    - 取Stream流
      创建一条流水线，并把数据放到流水线上准备进行操作
    - 中间方法
      流水线上的操作。一次操作完毕之后，还可以继续进行其他操作。
    - 终结方法
     一个Stream流只能有一个终结方法，是流水线上的最后一个操作

 **集合获取Stream流的方式**
 + 可以使用Collection接口中的默认方法stream​()生成流
    |名称|说明|
    |----|----|
    |default Stream<E> stream ()	|获取当前集合对象的Stream流|

- 数组获取Stream流的方式

  | 名称                                          | 说明                            |
  | --------------------------------------------- | ------------------------------- |
  | public static <T> Stream<T> stream(T[] array) | 获取当前数组的Stream流          |
  | public static<T> Stream<T> of(T... values)    | 获取当前数组/可变数据的Stream流 |


```
小结：
	 集合获取Stream流用: stream();
	 数组：Arrays.stream(数组)   /  Stream.of(数组)
```

```java
	/** --------------------Collection集合获取流-------------------------------   */
	Collection<String> list = new ArrayList<>();
	Stream<String> s =  list.stream();
	
	/** --------------------Map集合获取流-------------------------------   */
	Map<String, Integer> maps = new HashMap<>();
	// 键流
	Stream<String> keyStream = maps.keySet().stream();
	// 值流
	Stream<Integer> valueStream = maps.values().stream();
	// 键值对流（拿整体）
	Stream<Map.Entry<String,Integer>> keyAndValueStream =  maps.entrySet().stream();
	
	/** ---------------------数组获取流------------------------------   */
	String[] names = {"赵敏","小昭","灭绝","周芷若"};
	Stream<String> nameStream = Arrays.stream(names);
	Stream<String> nameStream2 = Stream.of(names);

```
## Stream流的常用API

+ Stream流的常用API(中间操作方法)

|名称	|说明|
|----|----|
|Stream<T> filter(Predicate<? super T> predicate)	|用于对流中的数据进行过滤。|
|Stream<T> limit (long maxSize)	|获取前几个元素|
|Stream<T> skip (long n)	|跳过前几个元素|
|Stream<T> distinct ()	|去除流中重复的元素。依赖(hashCode和equals方法)|
|static <T> Stream<T> concat (Stream a, Stream b)	|合并a和b两个流为一个流|
|Stream<T> map ( ...) |加工方法|

**注意：**

1. 中间方法也称为非终结方法，调用完成后返回新的Stream流可以继续使用，支持链式编程。
2. 在Stream流中无法直接修改集合、数组中的数据。



+ Stream流的常见终结操作方法

|名称	|说明|
|----|----|
|void forEach (Consumer action)	|对此流的每个元素执行遍历操作|
|long count ()	|返回此流中的元素数|

**注意：**终结操作方法，调用完成后流就无法继续使用了，原因是不会返回Stream了。

```java
List<String> list = new ArrayList<>();
list.add("张无忌");
list.add("周芷若");
list.add("赵敏");
list.add("张强");
list.add("张三丰");
list.add("张三丰");

list.stream().filter(s -> s.startsWith("张")).forEach(s -> System.out.println(s));

long size = list.stream().filter(s -> s.length() == 3).count();
System.out.println(size);

list.stream().filter(s -> s.startsWith("张")).limit(2).forEach(s -> System.out.println(s));
//下面的这行代码与上一行代码的功能相同
list.stream().filter(s -> s.startsWith("张")).limit(2).forEach(System.out::println);

list.stream().filter(s -> s.startsWith("张")).skip(2).forEach(System.out::println);

// map加工方法: 第一个参数原材料  -> 第二个参数是加工后的结果。
// 给集合元素的前面都加上一个：黑马的：
list.stream().map(s -> "黑马的：" + s).forEach(a -> System.out.println(a));

// 需求：把所有的名称 都加工成一个学生对象。
list.stream().map(s -> new Student(s)).forEach(s -> System.out.println(s));
//list.stream().map(Student::new).forEach(System.out::println); // 构造器引用  方法引用

// 合并流。
Stream<String> s1 = list.stream().filter(s -> s.startsWith("张"));
Stream<String> s2 = Stream.of("java1", "java2");

Stream<String> s3 = Stream.concat(s1 , s2);
s3.distinct().forEach(s -> System.out.println(s));
```



## 收集Stream流

+ Stream流的收集操作
    - 收集Stream流的含义：就是把Stream流操作后的结果数据转回到集合或者数组中去。
    - Stream流：方便操作集合/数组的手段。
    - 集合/数组：才是开发中的目的。
+ Stream流的收集方法
|名称	|说明|
|----|---|
|R collect (Collector collector)	|开始收集Stream流，指定收集器|

+ Collectors工具类提供了具体的收集方式
|名称	|说明|
|----|----|
|public static <T> Collector toList ()	|把元素收集到List集合中|
|public static <T> Collector toSet ()	|把元素收集到Set集合中|
|public static  Collector toMap (Function keyMapper , Function valueMapper)	|把元素收集到Map集合中|

```java
List<String> list = new ArrayList<>();
list.add("张无忌");
list.add("周芷若");
list.add("赵敏");
list.add("张强");
list.add("张三丰");
list.add("张三丰");

Stream<String> s1 = list.stream().filter(s -> s.startsWith("张"));
List<String> zhangList = s1.collect(Collectors.toList()); // 可变集合
zhangList.add("java1");
System.out.println(zhangList);

//       List<String> list1 = s1.toList(); // 得到不可变集合
//       list1.add("java");
//       System.out.println(list1);

// 注意注意注意：“流只能使用一次”
Stream<String> s2 = list.stream().filter(s -> s.startsWith("张"));
Set<String> zhangSet = s2.collect(Collectors.toSet());
System.out.println(zhangSet);

Stream<String> s3 = list.stream().filter(s -> s.startsWith("张"));
//         Object[] arrs = s3.toArray();
String[] arrs = s3.toArray(String[]::new); // 可以不管，拓展一下思维！！
System.out.println("Arrays数组内容：" + Arrays.toString(arrs));

```



# 异常处理



## 异常概述、体系

**什么是异常？**

- 异常是程序在“编译”或者“执行”的过程中可能出现的问题，注意：语法错误不算在异常体系中。 
- 比如:数组索引越界、空指针异常、 日期格式化异常，等…

**为什么要学习异常?**

- 异常一旦出现了，如果没有提前处理，程序就会退出JVM虚拟机而终止.
- 研究异常并且避免异常，然后提前处理异常，体现的是程序的安全, 健壮性。

<img src="image\exception.png" alt="异常体系" style="zoom:67%;" />

```
常的体系:
     Java中异常继承的根类是：Throwable。

         Throwable(根类，不是异常类)
      /              \
     Error           Exception（异常，需要研究和处理）
                    /            \
                   编译时异常      RuntimeException(运行时异常)


Error : 系统级别问题、JVM退出等，代码无法控制。
    错误的意思，严重错误Error，无法通过处理的错误，一旦出现，程序员无能为力了，只能重启系统，优化项目。
    比如内存奔溃，JVM本身的奔溃。这个程序员无需理会。

Exception:java.lang包下，称为异常类，它表示程序本身可以处理的问题
    才是异常类，它才是开发中代码在编译或者执行的过程中可能出现的错误，它是需要提前处理的。以便程序更健壮！

```

**Exception异常的分类:**
     1.编译时异常：继承自Exception的异常或者其子类，编译阶段就会报错，必须程序员处理的。否则代码编译就不能通过！！

​    2.运行时异常:  继承自RuntimeException的异常或者其子类，编译阶段是不会出错的，它是在 运行时阶段可能出现，运行时异常可以处理也可以不处理，编译阶段是不会出错的，但是运行阶段可能出现，还是建议提前处理！！

**小结：**
    异常是程序在编译或者运行的过程中可能出现的错误！！
    异常分为2类：编译时异常，运行时异常。
        -- 编译时异常：继承了Exception,编译阶段就报错，必须处理，否则代码不通过。
        -- 运行时异常：继承了RuntimeException,编译阶段不会报错，运行时才可能出现。
    异常一旦真的出现，程序会终止，所以要研究异常，避免异常，处理异常，程序更健壮!!



## 常见运行时异常

运行时异常：一般是程序员业务没有考虑好或者是编程逻辑不严谨引起的程序错误，
自己的水平有问题！

1. 数组索引越界异常: ArrayIndexOutOfBoundsException
2. 空指针异常 : NullPointerException，直接输出没有问题，但是调用空指针的变量的功能就会报错。
3. 数学操作异常：ArithmeticException
4. 类型转换异常：ClassCastException
5. 数字转换异常： NumberFormatException

## 异常的默认处理流程

1. 默认会在出现异常的代码那里自动的创建一个异常对象：ArithmeticException。
2. 异常会从方法中出现的点这里抛出给调用者，调用者最终抛出给JVM虚拟机。
3. 虚拟机接收到异常对象后，先在控制台直接输出异常栈信息数据。
4. 直接从当前执行的异常点干掉当前程序。
5. 后续代码没有机会执行了，因为程序已经死亡。

## 编译时异常的处理机制

编译时异常的处理形式有三种：

- 出现异常直接抛出去给调用者，调用者也继续抛出去。
- 出现异常自己捕获处理，不麻烦别人。
- 前两者结合，出现异常直接抛出去给调用者，调用者捕获处理。

**异常处理方式1 —— throws**

- throws：用在方法上，可以将方法内部出现的异常抛出去给本方法的调用者处理。
- 这种方式并不好，发生异常的方法自己不处理异常，如果异常最终抛出去给虚拟机将引起程序死亡。

抛出异常格式：

```
方法 throws 异常1 ，异常2 ，异常3 ..{
}
```

规范做法： 

```
方法 throws Exception{
}
```



**异常处理方式2 —— try…catch…**

- 监视捕获异常，用在方法内部，可以将方法内部出现的异常直接捕获处理。
- 这种方式还可以，发生异常的方法自己独立完成异常的处理，程序可以继续往下执行。

格式：

```
try{
// 监视可能出现异常的代码！
}catch(异常类型1 变量){
// 处理异常
}catch(异常类型2 变量){
// 处理异常
}...

```

建议格式：

```
try{
// 可能出现异常的代码！
}catch (Exception e){
e.printStackTrace(); // 直接打印异常栈信息
}

Exception可以捕获处理一切异常类型！
```



**异常处理方式3 —— 前两者结合**

- 方法直接将异通过throws抛出去给调用者
- 调用者收到异常后直接捕获处理。



## 自定义异常



**自定义异常的必要？**

- Java无法为这个世界上全部的问题提供异常类。
- 如果企业想通过异常的方式来管理自己的某个业务问题，就需要自定义异常类了。

**自定义异常的好处：**

- 可以使用异常的机制管理业务问题，如提醒程序员注意。
- 同时一旦出现bug，可以用异常的形式清晰的指出出错的地方。



**自定义异常的分类**

1. 自定义编译时异常       
   -  定义一个异常类继承Exception.
   -  重写构造器。
   -  在出现异常的地方用throw new 自定义对象抛出，
   - 作用：编译时异常是编译阶段就报错，提醒更加强烈，一定需要处理！！

```java
/**
    自定义的编译时异常
      1、继承Exception
      2、重写构造器
 */
public class ItheimaAgeIlleagalException extends Exception{
    public ItheimaAgeIlleagalException() {
    }

    public ItheimaAgeIlleagalException(String message) {
        super(message);
    }
}
```

2. 自定义运行时异常
   - 定义一个异常类继承RuntimeException.
   - 重写构造器。
   - 在出现异常的地方用throw new 自定义对象抛出!
   - 作用：提醒不强烈，编译阶段不报错！！运行时才可能出现！！

```java
/**
    自定义的编译时异常
      1、继承RuntimeException
      2、重写构造器
 */
public class ItheimaAgeIlleagalRuntimeException extends RuntimeException{
    public ItheimaAgeIlleagalRuntimeException() {
    }

    public ItheimaAgeIlleagalRuntimeException(String message) {
        super(message);
    }
}
```

```java
public static void main(String[] args) {
    //编译时错误，编译都通不过
//        try {
//            checkAge(-34);
//        } catch (ItheimaAgeIlleagalException e) {
//            e.printStackTrace();
//        }
	//运行时错误，编译时通过，运行通不过
        try {
            checkAge2(-23);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void checkAge2(int age)  {
        if(age < 0 || age > 200){
            // 抛出去一个异常对象给调用者
            // throw ：在方法内部直接创建一个异常对象，并从此点抛出
            // throws : 用在方法申明上的，抛出方法内部的异常
            throw new ItheimaAgeIlleagalRuntimeException(age + " is illeagal!");
        }else {
            System.out.println("年龄合法：推荐商品给其购买~~");
        }
    }

    public static void checkAge(int age) throws ItheimaAgeIlleagalException {
        if(age < 0 || age > 200){
            // 抛出去一个异常对象给调用者
            // throw ：在方法内部直接创建一个异常对象，并从此点抛出
            // throws : 用在方法申明上的，抛出方法内部的异常
            throw new ItheimaAgeIlleagalException(age + " is illeagal!");
        }else {
            System.out.println("年龄合法：推荐商品给其购买~~");
        }
    }
```



# 日志框架

## 日志技术的概述

**输出语句的弊端**

- 信息只能展示在控制台
- 不能将其记录到其他的位置（文件，数据库）
- 想取消记录的信息需要修改代码才可以完成

**日志技术具备的优势**

- 可以将系统执行的信息选择性的记录到指定的位置（控制台、文件中、数据库中）。
- 可以随时以开关的形式控制是否记录日志，无需修改源代码。

**日志技术的具体优势？**

|          | **输出语句**               | **日志技术**                         |
| -------- | -------------------------- | ------------------------------------ |
| 输出位置 | 只能是控制台               | 可以将日志信息写入到文件或者数据库中 |
| 取消日志 | 需要修改代码，灵活性比较差 | 不需要修改代码，灵活性比较好         |
| 多线程   | 性能较差                   | 性能较好                             |



## 日志技术体系结构

<img src="image\日志框架.png" alt="日志体系结构" style="zoom:67%;" />



**体系结构**                                                                        **日志实现框架**

   /日志规范接口 Commons Logging(简称：JCL)  /          Log4j

/                                                                             /               JUL (java.util.logging)  

\                                                                             \               Logback

   \Simple Logging facade for Java(简称：slf4j)  \            其它实现



- 日志规范：一些接口，提供给日志的实现框架设计的标准。
- 日志框架：牛人或者第三方公司已经做好的日志记录实现代码，后来者直接可以拿去使用。
- 因为对Commons Logging的接口不满意，有人就搞了SLF4J。因为对Log4j的性能不满意，有人就搞了Logback。

**日志的规范是什么，常见的有几种形式。**

- 日志规范大多是一些接口，提供给实现框架去设计的。
- 常见的规范是：
  - Commons Logging
  - Simple Logging Facade for Java

**日志的实现框架有哪些常见的？**

- Log4J
- Logback(我们重点学习的，其他的都大同小异)



## Logback概述

**Logback日志框架**

- Logback是由log4j创始人设计的另一个开源日志组件，性能比log4j要好
-  官方网站：https://logback.qos.ch/index.html 
- Logback是基于slf4j的日志规范实现的框架。

**Logback主要分为三个技术模块：**

-  logback-core： logback-core 模块为其他两个模块奠定了基础，必须有。
-  logback-classic：它是log4j的一个改良版本，同时它完整实现了slf4j API。
-  logback-access 模块与 Tomcat 和 Jetty 等 Servlet 容器集成，以提供 HTTP 访问日志功能

**使用Logback需要使用哪几个模块，各自的作用是什么**

- slf4j-api：日志规范
- logback-core：基础模块。
- logback-classic：它是log4j的一个改良版本，同时它完整实现了slf4j API



## Logback快速入门

**需求：**导入Logback日志技术到项目中，用于纪录系统的日志信息
**分析：**

1. 在项目下新建文件夹lib，导入Logback的相关jar包到该文件夹下，并添加到项目依赖库中去。

   - slf4j-api-1.7.26.jar
   - logback-core-1.2.3.jar
   - logback-classic-1.2.3.jar

2. 将Logback的核心配置文件logback.xml直接拷贝到src目录下（必须是src下）。

   - loback.xml

3. 在代码中获取日志的对象

   ```
   public static final Logger LOGGER = LoggerFactory.getLogger("类对象");
   ```

4. 使用日志对象LOGGER调用其方法输出不能的日志信息

[Logback](类库/日志框架)

## Logback配置详解-输出位置、格式设置

Logback日志系统的特性都是通过核心配置文件logback.xml控制的。

**Logback日志输出位置、格式设置：**

- 通过logback.xml 中的<append>标签可以设置输出位置和日志信息的详细格式。
- 通常可以设置2个日志输出位置：一个是控制台、一个是系统文件中

输出到控制台的配置标志

```
<appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
```

输出到系统文件的配置标志

```
<appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
```



## Logback配置详解-日志级别设置



**如果系统上线后只想记录一些错误的日志信息或者不想记录日志了，怎么办？**

- 可以通过设置日志的输出级别来控制哪些日志信息输出或者不输出。

**日志级别**

- 级别程度依次是：TRACE< DEBUG< INFO<WARN<ERROR  ; 默认级别是debug（忽略大小写），对应其方法。
- 作用：用于控制系统中哪些日志级别是可以输出的，只输出级别不低于设定级别的日志信息。
- ALL  和 OFF分别是打开全部日志信息，及关闭全部日志信息。

具体在<root level="INFO">标签的level属性中设置日志级别。

```
<root level=“INFO">
	<appender-ref ref="CONSOLE"/>
	<appender-ref ref="FILE" />
</root>
```

```
public class MovieSystem {
	
	//参数是所在类的全类名
	public static final Logger LOGGER = LoggerFactory.getLogger("MovieSystem.class");
	
	...
	private static void login() {
		...
		LOGGER.info(u.getUserName() +"登录了系统~~~");
		...
		LOGGER.error("时间解析出了毛病");
	}
	
}
```

**loback.xml文件内容：**

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!--
        CONSOLE ：表示当前的日志信息是可以输出到控制台的。
    -->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <!--输出流对象 默认 System.out 改为 System.err-->
        <target>System.out</target>
        <encoder>
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度
                %msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%-5level]  %c [%thread] : %msg%n</pattern>
        </encoder>
    </appender>

    <!-- File是输出的方向通向文件的 -->
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
            <charset>utf-8</charset>
        </encoder>
        <!--日志输出路径-->
        <file>C:/code/itheima-data.log</file>
        <!--指定日志文件拆分和压缩规则-->
        <rollingPolicy
                class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <!--通过指定压缩文件名称，来确定分割文件方式-->
            <fileNamePattern>C:/code/itheima-data2-%d{yyyy-MM-dd}.log%i.gz</fileNamePattern>
            <!--文件拆分大小-->
            <maxFileSize>1MB</maxFileSize>
        </rollingPolicy>
    </appender>

    <!--
    level:用来设置打印级别，大小写无关：TRACE, DEBUG, INFO, WARN, ERROR    |    ALL 和 OFF
   ， 默认debug
    <root>可以包含零个或多个<appender-ref>元素，标识这个输出位置将会被本日志级别控制。
    -->
    <root level="ALL">
        <!-- 注意：如果这里不配置关联打印位置，该位置将不会记录日志-->
        <appender-ref ref="FILE" />
    </root>
</configuration>
```





















