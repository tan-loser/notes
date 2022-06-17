# 静态关键字：static

## static的作用

static关键字的作用

- static是静态的意思，可以修饰成员变量和成员方法。
- static修饰成员变量表示该成员变量只在内存中只存储一份，可以被共享访问、修改。

## 成员变量

### 成员变量的用法

```java
public class User {
    // 成员变量
    public static int onlineNumber= 161;
    private String name;
    private int age;
    …
}
```

成员变量可以分为2类

1. 静态成员变量（有static修饰，属于类，内存中加载一次）: 常表示如在线人数信息、等需要被共享的信息，可以被共享访问。

```java
public class User {
    // 静态成员变量
    public static String onlineNumber= 161;
}
类名.静态成员变量。（推荐）
对象.静态成员变量。（不推荐）
```

```java
public class User {
    public static String onlineNumber= 161;
    // 实例成员变量
    private String name;
    private int age;
    …
}
对象.实例成员变量。
```

2. 实例成员变量（无static修饰，存在于每个对象中）：常表示姓名name、年龄age、等属于每个对象的信息。



### 内存原理

```java
public class User {
    // 静态成员变量：有static修饰，属于类，加载一次，可以被共享访问
    public static int onLineNumber = 161;
    // 实例成员变量：无static修饰，属于每个对象
    private String name;
    private int age;

    public static void main(String[] args) {
    	// 1、类名.静态成员变量
        System.out.println(User.onLineNumber);
        System.out.println(onLineNumber);
        User.onLineNumber++;

        // System.out.println(name); // 报错！

        // 2、创建用户对象: 对象.实例成员变量
        User u1 = new User();
        u1.name = "孙悟空";
        u1.age = 35;
        System.out.println(u1.name);
        System.out.println(u1.age);
        // 对象名称.静态成员变量( 不推荐 )
        u1.onLineNumber++;

        User u2 = new User();
        u2.name = "猪八戒";
        u2.age = 29;
        System.out.println(u2.name);
        System.out.println(u2.age);
        // 对象名称.静态成员变量( 不推荐 )
        u2.onLineNumber++;

        System.out.println(User.onLineNumber); // 164
    }
}

```

![](D:\桌面文件\笔记\notes\java-base\image\static内存原理.png)



## 成员方法

### 成员方法的基本用法

```java
 public static int getMax(int a , int b){
     return a > b ? a : b;
 }
```

成员方法的分类：

- 静态成员方法（有static修饰，属于类），建议用类名访问，也可以用对象访问。
- 实例成员方法（无static修饰，属于对象），只能用对象触发访问。

使用场景

- 表示对象自己的行为的，且方法中需要访问实例成员的，则该方法必须申明成实例方法。
- 如果该方法是以执行一个共用功能为目的，则可以申明成静态方法。

成员方法的分类和访问分别是什么样的？

1.  静态成员方法（有static修饰，属于类和对象共享）访问格式：

   - 类名.静态成员方法。

   - 对象.静态成员方法。（不推荐）

2. 实例成员方法（无static修饰，属于对象）的访问格式：

   - 对象.实例成员方法。

**定义员工类的实例**

需求：请完成一个标准实体类的设计，并提供如下要求实现。

1. 某公司的员工信息系统中，需要定义一个公司的员工类Employee，包含如下信息（name, age , 所在部门名称dept ） , 定义一个静态的成员变量company记录公司的名称。
2. 需要在Employee类中定义一个方法showInfos()，用于输出当前员工对象的信息。如name, age ，dept 以及公司名称company的信息。
3. 需要在Employee类中定义定义一个通用的静态方法compareByAge，用于传输两个员工对象的年龄进入，并返回比较较大的年龄，例如：2个人中的最大年龄是:45岁。

### 内存原理

```java
public class Student {
    private String name;
    // 1. 实例方法: 无static修饰的，属于对象的
    public void study(){
        System.out.println(name +  “在好好学习~~~”);
    }
    // 2. 静态方法：有static修饰，属于类和对象共享的
    public static int getMax(int a , int b){
        return a > b ? a : b;
    }
    public static void main(String[] args) {
        // 1. 类名.静态成员方法
        System.out.println(Student.getMax(10 , 2));
        // 注意：同一个类中访问静态成员类名可以不写
        System.out.println(getMax(2 , 10));
        
        // 2. 对象.实例成员方法
        // study(); // 会报错
        Student s = new  Student();
        s.name = "猪八戒";
        s.study();
        
        // 3. 对象.静态成员方法。（不推荐）
        System.out.println(s.getMax(20 , 10));
    }
}
```

![](D:\桌面文件\笔记\notes\java-base\image\static方法内存原理.png)



## 案例：设计工具类

工具类：

- 工具类中定义的都是一些静态方法，每个方法都是以完成一个共用的功能为目的。

工具类的好处

- 一是调用方便，二是提高了代码复用（一次编写，处处可用）

为什么工具类中的方法不用实例方法做？ 

- 实例方法需要创建对象调用，此时用对象只是为了调用方法，这样只会浪费内存。

**工具类的定义注意**

- 建议将工具类的构造器进行私有，工具类无需创建对象。
- 里面都是静态方法，直接用类名访问即可，工具类不需要创建对象。

**定义数组工具类**

需求：在实际开发中，经常会遇到一些数组使用的工具类。请按照如下要求编写一个数组的工具类：ArraysUtils

1. 我们知道数组对象直接输出的时候是输出对象的地址的，而项目中很多地方都需要返回数组的内容，请在ArraysUtils中提供一个工具类方法toString，用于返回整数数组的内容，返回的字符串格式如：[10, 20, 50, 34, 100]（只考虑整数数组，且只考虑一维数组）
2. 经常需要统计平均值，平均值为去掉最低分和最高分后的分值，请提供这样一个工具方法getAerage，用于返回平均分。（只考虑浮点型数组，且只考虑一维数组）
3. 定义一个测试类TestDemo，调用该工具类的工具方法，并返回结果。

```java
public class ArrayUtils {
    /**
       把它的构造器私有化
     */
    private ArrayUtils(){
    }

    /**
       静态方法，工具方法
     */
    public static String toString(int[] arr){
        if(arr != null ){
            String result = "[";
            for (int i = 0; i < arr.length; i++) {
                result += (i == arr.length - 1 ? arr[i] : arr[i] + ", ");
            }
            result += "]";
            return result;
        }else {
            return null;
        }
    }

    /**
     静态方法，工具方法
     */
    public static double getAverage(int[] arr){
        // 总和  最大值 最小值
        int max = arr[0];
        int min = arr[0];
        int sum = 0;
        for (int i = 0; i < arr.length; i++) {
            if(arr[i] > max){
                max = arr[i];
            }
            if(arr[i] < min){
                min = arr[i];
            }
            sum += arr[i];
        }
        return (sum - max - min)*1.0 / (arr.length - 2);
    }
}
```

```java
public static void main(String[] args) {
    int[] arr = {10, 20, 30};
    System.out.println(arr);
    System.out.println(ArrayUtils.toString(arr));
    System.out.println(ArrayUtils.getAverage(arr));

    int[] arr1 = null;
    System.out.println(ArrayUtils.toString(arr1));
    int[] arr2 = {};
    System.out.println(ArrayUtils.toString(arr2));

}
```





## 注意事项总结[面试热点]

static访问注意实现：

- 静态方法只能访问静态的成员，不可以直接访问实例成员。
- 实例方法可以访问静态的成员，也可以访问实例成员。
- 静态方法中是不可以出现this关键字的。

```java
public class Test {
    // 静态成员变量
    public static int onLineNumber;
    // 实例成员变量
    private String name;

    public static void getMax(){
        // 1、静态方法可以直接访问静态成员,不能访问实例成员。
        System.out.println(Test.onLineNumber);
        System.out.println(onLineNumber);
        inAddr();

        // System.out.println(name);

        // 3、静态方法中不能出现this关键字
        // System.out.println(this);
    }

    public void run(){
        // 2、实例方法可以直接访问静态成员，也可以访问实例成员
        System.out.println(Test.onLineNumber);
        System.out.println(onLineNumber);
        Test.getMax();
        getMax();

        System.out.println(name);
        sing();

        System.out.println(this);
    }

    public void sing(){
        System.out.println(this);
    }

    // 静态成员方法
    public static void inAddr(){
        System.out.println("我们在黑马程序员~~");
    }

    public static void main(String[] args) {

    }
}
```



# static应用知识：

## 代码块

### 代码块的分类、作用

代码块概述

- 代码块是类的5大成分之一（成员变量、构造器，方法，代码块，内部类），定义在类中方法外。
- 在Java类下，使用 { } 括起来的代码被称为代码块 。

代码块分为

- 静态代码块: 
  - 格式：static{}
  - 特点：需要通过static关键字修饰，随着类的加载而加载，并且自动触发、只执行一次
  - 使用场景：在类加载的时候做一些静态数据初始化的操作，以便后续使用。
-  构造代码块（了解，用的少）：
  - 格式：{}
  - 特点：每次创建对象，调用构造器执行时，都会执行该代码块中的代码，并且在构造器执行前执行
  - 使用场景：初始化实例资源。



### 静态代码块的应用案例

**斗地主游戏**

**需求：**

- 在启动游戏房间的时候，应该提前准备好54张牌，后续才可以直接使用这些牌数据。

**分析：**

1. 该房间只需要一副牌。
2. 定义一个静态的ArrayList集合存储54张牌对象，静态的集合只会加载一份。
3. 在启动游戏房间前，应该将54张牌初始化好
4. 当系统启动的同时需要准备好54张牌数据，此时可以用静态代码块完成。

静态代码块的作用是什么?

- 如果要在启动系统时对数据进行初始化。
- 建议使用静态代码块完成数据的初始化操作，代码优雅。

```java
public class StaticCodeTest3 {
    /**
       模拟初始化牌操作
         点数: "3","4","5","6","7","8","9","10","J","Q","K","A","2"
         花色: "♠", "♥", "♣", "♦"
       1、准备一个容器，存储54张牌对象，这个容器建议使用静态的集合。静态的集合只加载一次。
     */
    // int age = 12;
    public static ArrayList<String> cards = new ArrayList<>();

    /**
       2、在游戏启动之前需要准备好54张牌放进去，使用静态代码块进行初始化
     */
    static{
        // 3、加载54张牌进去。
        // 4、准备4种花色：类型确定，个数确定了
        String[] colors = {"♠", "♥", "♣", "♦"};
        // 5、定义点数
        String[] sizes = {"3","4","5","6","7","8","9","10","J","Q","K","A","2"};
        // 6、先遍历点数、再组合花色
        for (int i = 0; i < sizes.length; i++) {
            // sizes[i]
            for (int j = 0; j < colors.length; j++) {
                cards.add(sizes[i] + colors[j]);
            }
        }
        // 7、添加大小王
        cards.add("小🃏");
        cards.add("大🃏");
    }

    public static void main(String[] args) {
        System.out.println("新牌：" +  cards);
    }
}
```

```java
//静态代码块
public class TestDemo1 {

    public static String schoolName;

    public static void main(String[] args) {
        // 目标：学习静态代码块的特点、基本作用
        System.out.println("=========main方法被执行输出===========");
        System.out.println(schoolName);
    }

    /**
     特点：与类一起加载，自动触发一次，优先执行
     作用：可以在程序加载时进行静态数据的初始化操作（准备内容）
     */
    static{
        System.out.println("==静态代码块被触发执行==");
        schoolName = "黑马程序员";
    }
}
```

```java
//构造代码块
public class TestDemo2 {

    private String name;

    /**
       属于对象的，与对象一起加载，自动触发执行。
     */
    {
        System.out.println("==构造代码块被触发执行一次==");
        name = "张麻子";
    }

    public TestDemo2(){
        System.out.println("==构造器被触发执行==");
    }

    public static void main(String[] args) {
        // 目标：学习构造代码块的特点、基本作用
        TestDemo2 t = new TestDemo2();
        System.out.println(t.name);

        TestDemo2 t1 = new TestDemo2();
        System.out.println(t1.name);
    }

}
```





## 单例 

### 设计模式

什么是设计模式（Design pattern）

- 开发中经常遇到一些问题，一个问题通常有n种解法的，但其中肯定有一种解法是最优的，这个最优的解法被人总结出来了，称之为设计模式。
- 设计模式有20多种，对应20多种软件开发中会遇到的问题，学设计模式主要是学2点：
  - 第一：这种模式用来解决什么问题。
  - 第二：遇到这种问题了，该模式是怎么写的，他是如何解决这个问题的。

### 单例模式介绍

单例模式

- 可以保证系统中，应用该模式的这个类永远只有一个实例，即一个类永远只能创建一个对象。
- 例如任务管理器对象我们只需要一个就可以解决问题了，这样可以节省内存空间。

单例的实现方式很多

- 饿汉单例模式。
- 懒汉单例模式。

### 饿汉单例模式

饿汉单例设计模式

- 在用类获取对象的时候，对象已经提前为你创建好了。

设计步骤：

- 定义一个类，把构造器私有。
- 定义一个静态变量存储一个对象。

```java
/** a、定义一个单例类 */
\public class SingleInstance {
    /** c.定义一个静态变量存储一个对象即可 :属于类，与类一起加载一次 */
    public static SingleInstance instance = new SingleInstance ();
    
    /** b.单例必须私有构造器*/
    private SingleInstance (){
        System.out.println("创建了一个对象");
    }
}
```

饿汉单例的实现步骤?

- 定义一个类，把构造器私有。
- 定义一个静态变量存储一个对象



### 懒汉单例模式

汉单例设计模式

- 在真正需要该对象的时候，才去创建一个对象(延迟加载对象)。

设计步骤：

1. 定义一个类，把构造器私有。
2. 定义一个静态变量存储一个对象。
3. 提供一个返回单例对象的方法

```java
/** 定义一个单例类 */
class SingleInstance{
    /** 定义一个静态变量存储一个对象即可 :属于类，与类一起加载一次 */
    public static SingleInstance instance ; // null
    
    /** 单例必须私有构造器*/
    private SingleInstance(){}
    
    /** 必须提供一个方法返回一个单例对象  */
    public static SingleInstance getInstance(){
        if(instance == null){
            // 第一次来拿对象，为他做一个对象
            instance = new SingleInstance2();
        }
        return instance;
    }
}

```



# 面向对象三大特征之二：继承

## 概述、继承的好处

**什么是继承？**

- Java中提供一个关键字extends，用这个关键字，我们可以让一个类和另一个类建立起父子关系。
- `public class Student extends People {}`

Student称为子类（派生类），People称为父类(基类 或超类)。

**使用继承的好处**

- 当子类继承父类后，就可以直接使用父类公共的属性和方法了。因此，用好这个技术可以很好的我们提高代码的复用性

继承的格式

- 子类 extends父类

继承后子类的特点？

- 子类 继承父类，子类可以得到父类的属性和行为，子类可以使用。
- Java中子类更强大

```java
public class Student{
    private String name;
    private int age;

    public void study(){
        System.out.println("努力学习");
    }

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
}
```

```java
public class Teacher {
    private String name;
    private int age;
    
    public void teach(){
        System.out.println("教书育人");
    }
    
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
}
```

**解决方案：把相同的属性和行为抽离出来，可以降低重复代码的书写，抽取出来放到何处呢？**

```java
public class People {
    private String name;
    private int age;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
}
```

```java
ublic class Student extends People{
	public void study(){
        System.out.println("努力学习");
    }
}
```

```java
ublic class Teacher extends People{
    public void teach(){
        System.out.println("教书育人");
    }
}
```



## 设计规范、内存运行原理

继承设计规范：

- 子类们相同特征（共性属性，共性方法）放在父类中定义，子类独有的的属性和行为应该定义在子类自己里面。

为什么？ 

- 如果子类的独有属性、行为定义在父类中，会导致其它子类也会得到这些属性和行为，这不符合面向对象逻辑。

**继承的设计规范**

**需求：**

- 在传智教育的tlias教学资源管理系统中，存在学生、老师角色会进入系统。

**分析：**

1. 学生信息和行为（名称，年龄，所在班级，查看课表，填写听课反馈）
2. 老师信息和行为（名称，年龄，部门名称，查看课表，发布问题）
3. 定义角色类作为父类包含属性（名称，年龄），行为（查看课表）
4. 定义子类：学生类包含属性（所在班级），行为（填写听课反馈）
5. 定义子类：老师类包含属性（部门名称），行为（发布问题）

```java
/**
   角色类 父类
 */
public class Role {
    private String name;
    private int age;

    /**
        共同行为
     */
    public void queryCourse(){
        System.out.println(name + "开始查看课程信息~~");
    }
...
    public void setAge(int age) {
        this.age = age;
    }
}
```

```java
public class Student extends Role{
    // 独有属性
    private String className;

    // 独有行为
    public void writeInfo(){
        System.out.println(getName()
                + "说：今天学习感觉美美的,老师也是666~~~");
    }

    public String getClassName() {
        return className;
    }

    public void setClassName(String className) {
        this.className = className;
    }
}
```

```java
public static void main(String[] args) {
    // 1、创建学生对象
    Student s = new Student();
    s.setName("张松松"); // 父类的
    s.setAge(25); // 父类的
    s.setClassName("Java999期"); // 子类的
    System.out.println(s.getName());
    System.out.println(s.getAge());
    System.out.println(s.getClassName());
    s.queryCourse(); // 父类的
    s.writeInfo(); // 子类的

}
```





继承需要满足什么样的设计规范？

- 子类们相同特征（共性属性，共性方法）放在父类中定义。
- 子类独有的的属性和行为应该定义在子类自己里面。

![](D:\桌面文件\笔记\notes\java-base\image\extends内存原理.png)



## 继承的特点

**继承的特点**

1. 子类可以继承父类的属性和行为，但是子类不能继承父类的构造器。
2. Java是单继承模式：**一个类只能继承一个直接父类。**
3. Java不支持多继承、**但是支持多层继承。**
4. Java中所有的类都是Object类的子类。

**子类是否可以继承父类的构造器？**

- 不可以的，子类有自己的构造器，父类构造器用于初始化父类对象。

**子类是否可以继承父类的私有成员？**

- 可以的，只是不能直接访问。

**子类是否可以继承父类的静态成员？**

- 有争议的知识点。
- 子类可以直接使用父类的静态成员（共享）
- 但个人认为：子类不能继承父类的静态成员。（共享并非继承）

Object特点：

- Java中所有类，要么直接继承了Object , 要么默认继承了Object , 要么间接继承了Object, Object是祖宗类。



## 继承后：

### 成员变量、成员方法的访问特点

子类方法中访问成员（成员变量、成员方法）满足：**就近原则**

- 先子类局部范围找
- 然后子类成员范围找
- 然后父类成员范围找，如果父类范围还没有找到则报错。

如果子父类中，出现了重名的成员，会优先使用子类的，此时如果一定要在子类中使用父类的怎么办？

- 可以通过super关键字，指定访问父类的成员。
- 格式：super.父类成员变量/父类成员方法



### 方法重写

什么是方法重写？

- 在继承体系中，子类出现了和父类中一模一样的方法声明，我们就称子类这个方法是重写的方法。

方法重写的应用场景

- 当子类需要父类的功能，但父类的该功能不完全满足自己的需求时。

- 子类可以重写父类中的方法。

案例演示：

- 旧手机的功能只能是基本的打电话，发信息
- 新手机的功能需要能够：基本的打电话下支持视频通话。基本的发信息下支持发送语音和图片。

**@Override重写注解**

1. @Override是放在重写后的方法上，作为重写是否正确的校验注解。
2. 加上该注解后如果重写错误，编译阶段会出现错误提示。
3. 建议重写方法都加@Override注解，代码安全，优雅！

**方法重写注意事项和要求**

1. 重写方法的**名称、形参列表**必须与被重写方法的名称和参数列表**一致**。
2. 私有方法不能被重写。
3. 子类重写父类方法时，**访问权限必须大于或者等于父类** （暂时了解 ：缺省 < protected < public）
4. 子类不能重写父类的静态方法，如果重写会报错的。



### 子类构造器的特点

子类继承父类后构造器的特点：

- 子类中所有的构造器默认都会先访问父类中无参的构造器，再执行自己。

  ```java
  public class Cat extends Animal{
      public Cat(){
          super(); // 默认的，写不写都有，默认就是找父类无参数构造器
          System.out.println("==子类Cat无参数构造器被执行===");
      }
  
      public Cat(String n){
          super(); // 默认的，写不写都有，默认就是找父类无参数构造器
          System.out.println("==子类Cat有参数构造器被执行===");
      }
  }
  ```

  

为什么？

- 子类在初始化的时候，有可能会使用到父类中的数据，如果父类没有完成初始化，子类将无法使用父类的数据。
- 子类初始化之前，一定要调用父类构造器先完成父类数据空间的初始化。

怎么调用父类构造器的？

- 子类构造器的第一行语句默认都是：super()，不写也存在。



### 子类构造器访问父类有参构造器

super调用父类有参数构造器的作用：

-  初始化继承自父类的数据。

如果父类中没有无参数构造器，只有有参构造器，会出现什么现象呢？

- 会报错。因为子类默认是调用父类无参构造器的。

如何解决？

- 子类构造器中可以通过书写 super(…)，手动调用父类的有参数构造器

**子类的每个构造器都会调用父类的无参构造器，除非在第一行调用父类的有参构造器**

```java
public class Student extends People{
    private String className;

    public Student(){
    }

    public Student(String name, int age, String className) {
        super(name, age);
        this.className = className;
    }

    public String getClassName() {
        return className;
    }

    public void setClassName(String className) {
        this.className = className;
    }
}
```

```java
public class Student {
    private String name;
    private String schoolName;

    public Student() {
    }

    public Student(String name) {
        // 借用兄弟构造器！
        this(name, "黑马培训中心");
    }


    public Student(String name, String schoolName) {
        this.name = name;
        this.schoolName = schoolName;
    }
...
    public void setSchoolName(String schoolName) {
        this.schoolName = schoolName;
    }
}
```



### this、super使用总结

this和super详情

- this：代表本类对象的引用；super：代表父类存储空间的标识。

| **关键字** | **访问成员变量**               | **访问成员方法**                  | **访问构造方法**       |
| ---------- | ------------------------------ | --------------------------------- | ---------------------- |
| **this**   | this.成员变量访问本类成员变量  | this.成员方法(…)访问本类成员方法  | this(…)访问本类构器    |
| **super**  | super.成员变量访问父类成员变量 | super.成员方法(…)访问父类成员方法 | super(…)访问父类构造器 |

实际上，在以上的总结中，唯独只有this调用本类其他构造器我们是没有接触过的。

**案例需求：**

- 在学员信息登记系统中，后台创建对象封装数据的时候如果用户没有输入学校，则默认使用“黑马培训中心”。
- 如果用户输入了学校则使用用户输入的学校信息。

this(...)和super(…)使用注意点：

1. 子类通过 this (...）去调用本类的其他构造器，本类其他构造器会通过 super 去手动调用父类的构造器，最终还是会调用父类构造器的。
2. 注意：this(…) super(…) 都只能放在构造器的第一行，所以二者不能共存在同一个构造器中。