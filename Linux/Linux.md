[TOC]

# Linux 之进程管理
## 获取进程的常用属性
**获取进程自身pid**
获取进程本身的进程ID的系统调用函数是getpid，具体的说明如下： 

需要的头文件如下： 
```c
#include <sys/types.h>  
#include <unistd.h>  
```
函数格式如下：
`pid_t getpid(void);`

函数返回值说明：
`返回当前进程的pid值。`

**获取父进程pid**
获取父进程的进程ID的系统调用函数是getppid，具体的说明如下： 

函数格式如下：
`pid_t getppid(void);`

函数返回值说明：
`返回当前进程的父进程的pid值。`

**案例演示1:  **
编写一个程序，打印父进程ID和自身进程ID。详细代码如下所示： 
```
#include <stdio.h>  
#include <sys/types.h>  
#include <unistd.h>
int main()  
{  
    pid_t pid = getpid();  
    printf("当前进程的ID为:%d\n", pid);  
    pid_t ppid = getppid();  
    printf("当前进程的父进程ID为:%d\n", ppid);  
    return 0;  
}  
```
将以上代码保存为getppid.c文件，编译执行。可以看到每次运行都打印出相同的父进程ID，这是因为我们在同一个终端中运行3次程序，所以被运行的程序父进程为终端进程，因为父进程一直都一样。

## 使用fork函数创建进程
fork函数的具体的说明如下： 

需要的头文件如下： 
`#include <unistd.h> `

函数格式如下：
`pid_t fork(void);`

函数返回值说明：
`调用成功，fork函数两个值，分别是0和子进程ID号。当调用失败时，返回-1，并设置错误编号errno。`

注意：fork函数调用将执行两次返回，它将从父进程和子进程中分别返回。从父进程返回时的返回值为子进程的 PID，,而从子进程返回时的返回值为0，并且返回都将执行fork之后的语句。

**案例演示1:**
编写一个程序，使用fork函数创建一个新进程，并在子进程中打印出其进程ID和父进程ID，在父进程中返回进程ID。详细代码如下所示： 
```
#include <stdio.h>  
#include <sys/types.h>  
#include <unistd.h>  
#include <string.h>  
#include <errno.h>
int main()  
{  
    pid_t pid;  
    pid = fork();  
    if(pid == -1)  
    {  
        //创建进程失败  
        printf("创建进程失败(%s)!\n", strerror(errno));  
        return -1;  
    }  
    else if(pid == 0)  
    {  
        //子进程  
        printf("当前进程为子进程：pid(%d)，ppid(%d)\n", getpid(), getppid());  
    }  
    else  
    {  
        //父进程  
        printf("当前进程为父进程：pid(%d)，ppid(%d)\n", getpid(), getppid());  
    }  
    //子进程和父进程分别会执行的内容  
    return 0;  
}  
```
将以上代码保存为forkProcess.c文件，编译执行。可以看到每次执行forkProcess时，子进程和父进程都不是固定的执行顺序，因此由fork函数创建的子进程执行顺序是由操作系统调度器来选择执行的。因此，子进程和父进行在执行的时候顺序不固定。

## 进程终止

常见与退出进程相关的函数有：exit、_exit、atexit、on_exit、abort和assert。

1. exit函数是标准C库中提供的函数，它用来终止正在运行的程序，并且关闭所有I/O标准流。
2. _exit函数也可用于结束一个进程，与exit函数不同的是，_exit不会关闭所有I/O标准流。
3. atexit 函数用于注册一个不带参数也没有返回值的函数以供程序正常退出时被调用。
4. on_exit 函数的作用与atxeit函数十分类似，不同的是它注册的函数具有参数，退出状态和参arg都是传递给该程序使用的。
5. abort 函数其实是用来发送一个SIGABRT信号，这个信号将使当前进程终止。
6. assert是一个宏。调用assert时，它将先计算参数表达式 expression的值,如果为0，则调用abort函数结束进程。

# Linux 文件/目录管理

## Linux之文件创建/删除
**Linux创建文件**
Linux中使用touch命令来创建一个空文件。

具体命令如下：`touch 文件名`

具体说明： 
* 如果一次想创建多个文件，则每个文件名用空格隔开。
* touch命令创建一个指定的新文件，并将当前登录用户作为文件所有者。
* 由于touch命令创建的文件为空，所以文件的大小为0。
* touch命令还可以用于更改文件的访问时间和修改时间，而不改变文件的内容。

在Linux系统命令行下创建一个新的文件，文件名为：newFile，则可以使用如下命令：
`touch newFile`

使用ls命令来查看文件是否创建成功。

在Linux系统命令行下一次创建2个文件，文件名分别为：newFile1和newFile2，则可以使用如下命令：`touch newFile1 newFile2`

**Linux删除文件**
Linux中使用rm命令来删除一个已经存在的文件。

具体命令如下：`rm 参数 文件名`

常用参数如下：

* -f：强制删除文件或目录；
* -i：删除已有文件或目录之前先询问用户；
* -r或-R：递归处理，将指定目录下的所有文件与子目录一并处理；-i：删除已有文件或目录之前先询问用户。
具体说明：  
* 如果一次想删除多个文件，则每个文件名用空格隔开。
* rm命令可以使用通配符来删除文件。

在Linux系统命令行下删除一个文件，文件名为：newFile，则可以使用如下命令：
`rm newFile`

使用ls命令来查看文件是否删除成功。

在Linux系统命令行下一次删除2个文件，文件名分别为：newFile1和newFile2，则可以使用如下命令：
`rm newFile1 newFile2`

在Linux系统命令行下删除一个文件同时删除前询问用户，文件名分别为：newFile，则可以使用如下命令：
`rm -i newFile`

## Linux创建/删除目录

**Linux中使用mkdir命令来创建一个空目录。**

具体命令如下：`mkdir 参数 目录名`

常用参数如下：
* -p或--parents：若所要建立目录的上层目录目前尚未建立，则会一并建立上层目录。
具体说明：  
* 如果一次想创建多个目录，则每个目录名用空格隔开。
* mkdir命令创建一个指定的目录，并将当前登录用户作为目录所有者。
* mkdir命令创建一个空目录后，该目录下只存在两个特殊的目录，分别是.和..。

在Linux系统命令行下创建一个新的目录，目录名为：newDir，则可以使用如下命令：
`mkdir newDir`

在Linux系统命令行下一次创建2个目录，目录名分别为：newDir1和newDir2，则可以使用如下命令：
`mkdir newDir1 newDir2`

在Linux系统命令行下创建一个目录如果上层目录目前尚未建立则一并将其创建，目录名分别为：Dir1/Dir2，则可以使用如下命令：
`mkdir -p Dir1/Dir2`
`ls -a Dir1`
`ls -a Dir2`

**Linux删除目录**
Linux中使用rmdir命令来删除一个已经存在的空目录。

具体命令如下：`rmdir 参数 目录名`

常用参数如下：

* -p或--parents：删除指定目录后，若该目录的上层目录已变成空目录，则将其一并删除；
具体说明：  
* 如果一次想删除多个空目录，则每个目录名用空格隔开。

* rmdir 命令可以使用通配符来删除目录。

如果想删除的目录不为空，则先使用rm命令将目录下的所有文件都清空，然后再使用rmdir将目录删除，或者直接使用rm -r命令直接递归的删除整个目录。

在Linux系统命令行下删除一个空目录，目录名为：newDir，则可以使用如下命令：
`rmdir newDir`

在Linux系统命令行下一次删除2个空目录，目录名分别为：newDir1和newDir2，则可以使用如下命令：
`rmdir newDir1 newDir2`

在Linux系统命令行下删除一个目录如果上层目录为空则一并将其删除，目录名分别为：Dir1/Dir2，则可以使用如下命令：
`ls -a Dir1 Dir1/Dir2'
`rmdir -p Dir1/Dir2`

在Linux系统命令行下删除一个不为空的目录，则可以使用如下命令：
`rm -r Dir`

## Linux 之文件复制/重命名

**Linux拷贝文件**
Linux中使用cp命令将一个或多个源文件复制到指定的目的目录下。它可以将单个源文件复制成一个指定文件名的具体的文件或一个已经存在的目录下。

具体命令如下：`scp 参数 源文件 目的目录`

常用参数如下：
* -f：强行复制文件或目录，不论目标文件或目录是否已存在；
* -i：覆盖既有文件之前先询问用户；
* -p：保留源文件或目录的属性。
具体说明：  
* cp命令支持同时复制多个文件，当一次复制多个文件时，目的目录参数必须是一个已经存在的目录，否则将出现错误；
* cp命令支持同时复制多个文件，当一次复制多个文件时，多个文件用空格分隔。

将当前目录下的一个文件拷贝到一个指定的目录下，文件名为：newFile，目录名为：newDir，则可以使用如下命令：
`cp newFile newDir`

将当前目录下的一个文件拷贝到一个指定的目录下并重命名为newFileCpy，文件名为：newFile，目录名为：newDir，则可以使用如下命令：
`cp newFile newDir/newFileCpy`

将当前目录下的两个文件拷贝到一个指定的目录下，文件名为：newFile1和newFile2，目录名为：newDir1，则可以使用如下命令：
`cp newFile1 newFile2 newDir1`

**Linux重命名文件**
Linux中使用mv命令来重命名一个文件名。

mv命令还可以用来移动文件，类似于Windows上的剪切功能。

具体命令如下：`mv 参数 目录名`

常用参数如下：
* -f：若目标文件与现有的文件重复，则直接覆盖现有的文件；
* -i：交互式操作，覆盖前先行询问用户，如果源文件与目标文件或目标目录中的文件同名，则询问用户是否覆盖目标文件。用户输入”y”，表示将覆盖目标文件；输入”n”，表示取消对源文件的移动。这样可以避免误将文件覆盖。
具体说明：  
* 如果一次想移动多个文件，则每个文件名用空格隔开；
* mv命令可以使用通配符来移动目录。

将当前目录下的文件newFile重命名为newFileRename，则可以使用如下命令:
`mv newFile newFileRename`

将当前目录下的文件newFileRename移动到一个指定的目录下，目录名为：Dir，则可以使用如下命令：
`mv newFileRename Dir`

将当前目录下的文件newFileRename移动到一个指定的目录下并重命名为newFile，目录名为：Dir，则可以使用如下命令：
`mv newFileRename Dir/newFile`

## Linux 之目录复制/重命名

**Linux拷贝目录**
Linux中使用cp -r命令将一个或多个源目录复制到指定的目录下。它可以将单个源目录复制成一个指定目录名的具体的目录或一个已经存在的目录下。

具体命令如下：`cp 参数 源目录 目的目录`

常用参数如下：
* -r 是递归把源目录下的目录递归进行移动；
* -f：强行复制文件或目录，不论目标文件或目录是否已存在；
* -i：覆盖既有文件之前先询问用户；
* -p：保留源文件或目录的属性；
具体说明：  
* cp命令支持同时复制多个目录，当一次复制多个目录时，目的目录参数必须是一个已经存在的目录，否则将出现错误；
* cp命令支持同时复制多个目录，当一次复制多个目录时，多个目录名用空格分隔；
* cp命令拷贝目录和拷贝文件大致用法相似，最大差别就是拷贝目录的时候必须加上-r参数，而拷贝文件的时候不需要加-r参数。

将当前目录下的一个目录拷贝到指定的目录下，被拷贝目录名为：Dir1，指定目录名为：Dir2，则可以使用如下命令：
`cp -r Dir1 Dir2`

将当前目录下的一个目录拷贝到一个指定的目录下并重命名为Dir1Cpy，被拷贝目录名为：Dir1，指定目录名为：Dir2，则可以使用如下命令：
`cp -r Dir1 Dir2/Dir1Cpy`

将当前目录下的两个目录拷贝到一个指定的目录下，被拷贝目录名为：Dir1和Dir2，指定目录名为：Dir3，则可以使用如下命令：
`cp -r Dir1 Dir2 Dir3`

**Linux重命名目录**
Linux中使用mv命令来重命名一个目录名。

mv命令还可以用来移动目录，类似于Windows上的剪切功能。

具体命令如下：mv 参数 源目录 目的目录

常用参数如下：
* -f：若目标目录与现有的目录重复，则直接合并现有的目录；
* -i：交互式操作，覆盖前先行询问用户，如果源目录与目标目录同名，则询问用户是否合并目标目录。用户输入”y”，表示将合并目标目录；输入”n”，表示取消对源目录的移动。这样可以避免误将目录覆盖。
具体说明： 
* 如果一次想移动多个目录，则每个目录名用空格隔开。
* mv命令可以使用通配符来移动目录。
* mv命令移动目录和移动文件的使用大致一样，唯一不同的是如果出现源目录和目标目录重名，则采用合并的方式，而对于文件则是直接覆盖。

将当前目录下的目录newDir重命名为newDirCpy，则可以使用如下命令:
`mv newDir newDirCpy`

将当前目录下的目录newDirCpy移动到一个指定的目录下，指定目录名为：Dir，则可以使用如下命令：
`mv newDirCpy Dir`

将当前目录下的目录newDirCpy移动到一个指定的目录下并重命名为newDir，指定目录名为：Dir，则可以使用如下命令：
`mv newDirCpy Dir/newDir`

## Linux之文件/目录内容查看

### Linux查看文件内容
Linux中查看文件内容的命令有很多，接下来我们介绍常用的几个命令。

** cat命令**
具体命令如下：`cat 参数 文件名`

常用参数如下：
1. -n 或 --number：由1开始对所有输出的行数编号；
2. -b 或 --number-nonblank：和-n相似，只不过对于空白行不编号。
具体说明：  
1. cat命令支持同时查看多个文件，当一次查看多个文件时，每个文件的内容都会被打印到屏幕上；
2. cat命令不能对文件进行编辑操作，只能查看文件内容。

查看文件/etc/passwd内容，则可以使用如下命令：
`cat /etc/passwd`

**head命令**
具体命令如下：`head 参数 文件名`

常用参数如下：
1. -n number：显示文件的前number行内容；
具体说明：  
1. head命令是从文件的开头显示内容，默认情况下只显示前10行的内容；
2. head命令不能对文件进行编辑操作，只能查看文件内容。

查看文件/etc/passwd的前8行内容，则可以使用如下命令：
`head -n 8 /etc/passwd`

**tail命令**
具体命令如下：`tail 参数 文件名`

常用参数如下：
1. -n number：显示文件的最后number行内容；
具体说明：  
1. tail命令是从文件的结尾显示内容，默认情况下只显示最后10行的内容；
2. tail命令不能对文件进行编辑操作，只能查看文件内容。

查看文件/etc/passwd末尾5行内容，则可以使用如下命令：
`tail -n 5 /etc/passwd`

### Linux查看目录内容
Linux中使用ls命令来查看一个目录下的内容。

具体命令如下：`ls 参数 目录`

常用参数如下：
1. -a：显示所有档案及目录（ls内定将档案名或目录名称为“.”的视为影藏，不会列出）；
2. -A：显示除影藏文件“.”和“..”以外的所有文件列表；
3. -l：列出内容的详细信息；
4. -r：以文件名反序排列并输出目录内容列表；
5. -s：显示文件和目录的大小，以区块为单位；
6. -i：显示文件索引节点号（inode）。一个索引节点代表一个文件；
7. -R：递归处理，将指定目录下的所有文件及子目录一并处理。
具体说明：  
1. 如果一次查看多个目录，则每个目录名用空格隔开。

查看目录/mnt下的所有信息(包括隐藏内容)，则可以使用如下命令:
`ls -a /mnt`

查看目录/mnt下的所有信息(包括隐藏内容)，同时显示每个文件的详细信息，则可以使用如下命令:
`ls -al /mnt`



# Linux之线程同步一

## 互斥锁

对于多线程的程序，访问冲突的问题是很普遍的，Linux系统中为了解决线程同步问题引入了互斥锁(mutex)。互斥锁操作主要包括**加锁**、**解锁**和**测试加锁**三个，不论哪种类型的锁，都不可能被两个不同的线程同时得到，而必须等待解锁，因此能够保证同一时刻只运行一个线程执行一个关键部分的代码。

Linux系统中提供了如下几个函数来操作互斥锁：

|函数	|功能|
|--|--|
|pthread_mutex_init	|初始化一个互斥锁|
|pthread_mutex_destroy	|注销一个互斥锁|
|pthread_mutex_lock	|加锁操作，如果不成功，则阻塞等待|
|pthread_mutex_unlock	|解锁操作|
|pthread_mutex_trylock	|测试加锁，如果不成功则立刻返回|

以上函数我们可以使用man命令来查询该函数的使用方法。具体的查询命令为：man 3 函数名。



### 初始化互斥锁

使用互斥锁前必须先进行初始化操作。在Linux中初始化互斥锁有两种方式，分别是：(1) 静态赋值法；(2)使用初始化函数。

1. 静态赋值法
   静态赋值法是将宏结构常量直接赋值给互斥锁，常见的宏结构常量有如下几个：  
```
   PTHREAD_MUTEX_INITIALIZER: (普通锁)当一个线程加锁后，其余请求锁的线程形成等待队列，解锁后按照优先级获取锁;  PTHREAD_RECURSIVE_MUTEX_INITIALIZER_NP: (嵌套锁)允许一个线程对同一个锁进行多次加锁操作，并通过多次解锁来释放锁;  PTHREAD_ERRORCHECK_MUTEX_INITIALIZER_NP: (检错锁)在同一个线程请求同一个锁的情况下，返回EDEADLK，否则执行的动作与普通锁相同;  
```

例如使用静态赋值法来初始化一个互斥锁变量：
`pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;`

2. 函数赋值法
   Linux系统提供一个`pthread_mutex_init`库函数来对互斥锁进行初始化。
   `pthread_mutex_init`函数的具体的说明如下：  

- 需要的头文件如下：

```
  #include <pthread.h>  
```

- 函数格式如下：

```
  int pthread_mutex_init(pthread_mutex_t *mutex, const pthread_mutex attr_t *mutexattr);
```

  参数说明：

```
  mutex：互斥锁变量；  mutexattr：互斥锁属性，如果为NULL则使用默认属性，其他常见属性见下表；  
```

| 属性值  | 意义      |
| ---- | ------------------- |
| PTHREAD_MUTEX_TIMED_NP      | 普通锁: 当一个线程加锁后，其余请求锁的线程形成等待队列，解锁后按照优先级获取锁 |
| PTHREAD_MUTEX_RECURSIVE_NP  | 嵌套锁: 允许一个线程对同一个锁进行多次加锁操作，并通过多次解锁来释放锁 |
| PTHREAD_MUTEX_ERRORCHECK_NP | 检错锁: 在同一个线程请求同一个锁的情况下，返回`EDEADLK`，否则执行的动作与普通锁相同 |

- 函数返回值说明：
  `pthread_mutex_init`返回值总为`0`。

### 加锁操作

对互斥锁初始化后，就可以给互斥锁进行加锁操作。Linux提供了两个库函数来对加锁，分别是：`pthread_mutex_lock`和`pthread_mutex_trylock`，这些函数的具体的说明如下：

- 需要的头文件如下：  

```
  #include <pthread.h>  
```

- 函数格式如下：  
```
  int pthread_mutex_lock(pthread_mutex_t *mutex);  int pthread_mutex_trylock(pthread_mutex_t *mutex);  
```

参数说明：
`mutex：要被执行加锁操作的锁变量`

- 函数返回值说明：
  调用成功，返回值为`0`，否则返回一个非零的错误码。

- `pthread_mutex_lock`和`pthread_mutex_trylock`区别：
  用`pthread_mutex_lock`加锁时，如果`mutex`已经被锁住，当前尝试加锁的线程就会被阻塞，直到互斥锁被其他线程释放。而`pthread_mutex_trylock`函数则不同，如果`mutex`已经被锁住，它将立即返回，返回的错误码为`EBUSY`，而不是阻塞等待。

### 解锁操作

有加锁操作就相对应的有解锁操作。Linux提供了一个`pthread_mutex_unlock`函数来解锁操作，这个函数的具体的说明如下：

- 需要的头文件如下：  
```
  #include <pthread.h>  
```

- 函数格式如下：
  `int pthread_mutex_unlock(pthread_mutex_t *mutex);`
  参数说明：
  `mutex：要被执行解锁操作的锁变量`

- 函数返回值说明：
  调用成功，返回值为`0`，否则返回一个非零的错误码。

### 注销锁操作

当一个互斥锁使用完毕后，必须进行清除。Linux提供了一个`pthread_mutex_destroy`函数来注销一个互斥锁，这个函数的具体的说明如下：

- 需要的头文件如下：  

```
  #include <pthread.h>  
```

- 函数格式如下：
`int pthread_mutex_destroy(pthread_mutex_t *mutex);`
  参数说明：
  `mutex：要被执行注销操作的锁变量`

- 函数返回值说明：
  调用成功，返回值为`0`，否则返回一个非零的错误码。

**注意：**如果使用静态初始化来初始化一个互斥锁，则无需使用`pthread_mutex_destroy`对其注销。

*案例演示1:*
编写一个程序，使用静态初始化方法来初始化一个互斥锁，并对一个全局变量进行加锁。详细代码如下所示： 
```c
#include <stdio.h> 
#include <pthread.h>
int globalNumber = 1; 
//初始化一个普通互斥锁number_mutex 
pthread_mutex_t number_mutex = PTHREAD_MUTEX_INITIALIZER;
void *addNumer(void *arg) 
{ 
    pthread_mutex_lock(&number_mutex);  
    globalNumber++;  
    pthread_mutex_unlock(&number_mutex);  
    return NULL;  
}
int main() 
{ 
    pthread_t thread1, thread2;  
    pthread_create(&thread1, NULL, addNumer, NULL);  
    pthread_create(&thread2, NULL, addNumer, NULL);  
    pthread_join(thread1, NULL);  
    pthread_join(thread2, NULL);  
    printf("globalNumber = %d\n", globalNumber);  
    return 0;  
} 
```
将以上代码保存为mutexThread1.c文件，编译执行。可以看到globalNumber变量变为了3，尽管我们不对globalNumber变量进行加锁，结果可能也是3，但是有可能出现结果为2的情况。

案例演示2:
编写一个程序，使用pthread_mutex_init函数来初始化一个普通的互斥锁，并对一个全局变量进行加锁。详细代码如下所示： 
```c
#include <stdio.h> 
#include <pthread.h>
int globalNumber = 1; 
//初始化一个普通互斥锁number_mutex 
pthread_mutex_t number_mutex;
void *addNumer(void *arg) 
{ 
    pthread_mutex_lock(&number_mutex);  
    globalNumber++;  
    pthread_mutex_unlock(&number_mutex);  
    return NULL;  
}
int main() 
{ 
    pthread_mutex_init(&number_mutex, PTHREAD_MUTEX_TIMED_NP);  
    pthread_t thread1, thread2;  
    pthread_create(&thread1, NULL, addNumer, NULL);  
    pthread_create(&thread2, NULL, addNumer, NULL);  
    pthread_join(thread1, NULL);  
    pthread_join(thread2, NULL);  
    printf("globalNumber = %d\n", globalNumber);  
    pthread_mutex_destroy(&number_mutex);  
    return 0;  
} 
```
将以上代码保存为mutexThread2.c文件，编译执行。

## 自旋锁

自旋锁的功能以及使用方法和互斥锁极为相似。自旋锁与互斥锁的主要区别在于，当执行加锁操作时，如果当前锁不可用，对于自旋锁来说，则阻塞后不会让出cpu，会一直忙等待，直到得到锁。而对于互斥锁来说，阻塞后休眠让出cpu。

由于自旋锁不会睡眠，自旋锁一直占用cpu，在未获得锁的情况下，一直运行，所以占用着cpu，如果不能在很短的时间内获得锁，这无疑会使CPU效率降低。因此，自旋锁通常用于驱动程序开发中，适合对短暂处理的资源进行加锁。

**注意**：在单处理器环境下，我们是不需要自旋锁，尽管调用了自旋锁的函数，里面也不是自旋锁的实现。

Linux系统中提供了如下几个函数来操作自旋锁：

|函数	|功能|
|----|----|
|pthread_spin_init	|初始化一个自旋锁|
|pthread_spin_destroy	|注销一个自旋锁|
|pthread_spin_lock	|加锁操作，如果不成功，则阻塞等待|
|pthread_spin_trylock	|测试加锁，如果不成功则立刻返回|
|pthread_spin_unlock	|解锁操作|

以上函数我们可以使用man命令来查询该函数的使用方法。具体的查询命令为：man 3 函数名。

1. **初始化自旋锁**
使用自旋锁前必须先进行初始化操作。初始化自旋锁的库函数是pthread_spin_init。 
pthread_spin_init函数的具体的说明如下： 

需要的头文件如下：
`#include <pthread.h> `
函数格式如下：
`int pthread_spin_init(pthread_spinlock_t *lock, int pshared);`
参数说明：
* lock：自旋锁变量； 
* pshared：自旋锁属性，常见属性见下表； 
属性值	意义
* PTHREAD_PROCESS_SHARED	该自旋锁可以在多个进程中的线程之间共享
* PTHREAD_PROCESS_PRIVATE	仅初始化本自旋锁的线程所在的进程内的线程才能够使用该自旋锁
函数返回值说明：
调用成功，返回值总为0，否则返回一个非零的错误码。

2. **加锁操作**
对自旋锁初始化后，就可以给自旋锁进行加锁操作。Linux提供了两个库函数来对加锁，分别是：pthread_spin_lock和pthread_spin_trylock，这些函数的具体的说明如下：

需要的头文件如下： 
`#include <pthread.h> `
函数格式如下： 
```
int pthread_spin_lock(pthread_spinlock_t *lock); 
int pthread_spin_trylock(pthread_spinlock_t *lock); 
```
参数说明：
* lock：要被执行加锁操作的锁变量

函数返回值说明：
* 调用成功，返回值为0，否则，返回一个非零的错误码。

pthread_spin_lock和pthread_spin_trylock区别：
用pthread_spin_lock加锁时，如果lock已经被锁住，当前尝试加锁的线程就会被阻塞，直到自旋锁被其他线程释放。而pthread_spin_trylock函数则不同，如果lock已经被锁住，它将立即返回，返回的错误码为EBUSY，而不是阻塞等待。

3. **解锁操作**
有加锁操作就相对应的有解锁操作。Linux提供了一个pthread_spin_unlock函数来解锁操作，这个函数的具体的说明如下：

需要的头文件如下： 
`#include <pthread.h> `
函数格式如下：
`int pthread_spin_unlock(pthread_spinlock_t *lock);`
参数说明：
* lock：要被执行解锁操作的锁变量

函数返回值说明：
* 调用成功，返回值为0，否则返回一个非零的错误码。

注销锁操作
* 当一个自旋锁使用完毕后，必须进行清除。Linux提供了一个pthread_spin_destroy函数来注销一个自旋锁，这个函数的具体的说明如下：

需要的头文件如下： 
`#include <pthread.h> `
函数格式如下：
`int pthread_spin_destroy(pthread_spinlock_t *lock);`
参数说明：
* lock：要被执行注销操作的锁变量

函数返回值说明：
* 调用成功，返回值为0，否则返回一个非零的错误码。

*案例演示1:*
编写一个程序，使用自旋锁对一个全局变量进行加锁。详细代码如下所示：  
```c
#include <stdio.h> 
#include <pthread.h>
int globalNumber = 1; 
//初始化一个普通自旋锁number_lock 
pthread_spinlock_t number_lock;
void *addNumer(void *arg) 
{ 
    pthread_spin_lock(&number_lock);  
    globalNumber++;  
    pthread_spin_unlock(&number_lock);  
    return NULL;  
}
int main() 
{ 
    pthread_spin_init(&number_lock, PTHREAD_PROCESS_PRIVATE);  
    pthread_t thread1, thread2;  
    pthread_create(&thread1, NULL, addNumer, NULL);  
    pthread_create(&thread2, NULL, addNumer, NULL);  
    pthread_join(thread1, NULL);  
    pthread_join(thread2, NULL);  
    printf("globalNumber = %d\n", globalNumber);  
    pthread_spin_destroy(&number_lock);  
    return 0;  
} 
```
将以上代码保存为lockThread.c文件，编译执行。可以看到globalNumber变量变为了3，尽管我们不对globalNumber变量进行加锁，结果可能也是3，但是有可能出现结果为2的情况。

## 条件变量

条件变量是利用线程间共享的全局变量进行同步的一种机制。条件变量宏观上类似if语句，符合条件就能执行某段代码，否则，只能等待条件成立。使用条件变量主要包括两个动作，分别为：(1)一个等待使用资源的线程等待"条件变量被设置为真"；(2)一个正在使用资源的线程在使用完资源后"设置条件为真"；这样就可以保证线程间同步。但是，这样存在一个关键的问题，那就是要保证条件变量能够被正确的修改，因此，条件变量要受到特殊的保护才行。实际上使用互斥锁来扮演着这样的一个保护者的角色。注意：使用的互斥锁必须是普通的互斥锁

与互斥锁不同，条件变量是用来等待而不是用来上锁的。条件变量用来自动阻塞一个线程，直到某特殊情况发生为止。通常条件变量和互斥锁同时使用。

条件的检测是在互斥锁的保护下进行的。线程在改变条件状态之前必须首先锁住互斥量。如果一个条件为假，一个线程自动阻塞，并释放等待状态改变的互斥锁。如果另一个线程改变了条件，它发信号给关联的条件变量，唤醒一个或多个等待它的线程，这些线程将重新锁定互斥锁并重新测试条件是否满足。

Linux 系统中提供了如下几个函数来操作条件变量：

|函数	|功能|
|----|----|
|pthread_cond_init	|初始化一个条件变量|
|pthread_cond_wait	|基于条件变量阻塞，无条件等待|
|pthread_cond_waitdwait	|阻塞知道指定事件发生，计时等待|
|pthread_cond_signal	|解除特定线程的阻塞，存在多个等待线程事按照入队顺序激活其中一个|
|pthread_cond_broadcast	|解除所有线程的阻塞|
|pthread_cond_destroy	|注销一个条件变量|

以上函数我们可以使用man命令来查询该函数的使用方法。具体的查询命令为：man 3 函数名。

1. **初始化条件变量**
使用条件变量前必须先进行初始化操作。在Linux中初始化条件变量有两种方式，分别是：(1)静态赋值法；(2)使用初始化函数。

* 静态赋值法

静态赋值法是直接将宏结构常量直接赋值给条件变量，将宏结构变量PTHREAD_COND_INITIALIZER赋值给条件变量即可，例如：
`pthread_cond_t cond = PTHREAD_COND_INITIALIZER;`

* 函数赋值法

Linux 系统提供一个pthread_cond_init库函数来对条件变量进行初始化。
pthread_cond_init函数的具体的说明如下： 

需要的头文件如下： 
`#include <pthread.h> `
函数格式如下：
`int    pthread_cond_init(pthread_cond_t    *cond,   pthread_condattr_t  *cond_attr);`
参数说明： 
* cond：条件变量； 
* cond_attr：条件变量属性，由于该属性在实际中没有被实现，所以它的值通常是NULL； 
函数返回值说明：
* 调用成功，返回值为0，否则，返回一个非零的错误码。

2. **等待条件成立函数**
Linux 提供了两个库函数用来判断等待条件成立，分别是：pthread_cond_wait和pthread_cond_timedwait，这些函数的具体的说明如下：

需要的头文件如下：
`#include <pthread.h> `
函数格式如下： 
`int pthread_cond_wait(pthread_cond_t *cond, pthread_mutex_t *mutex); `
`int   pthread_cond_timedwait(pthread_cond_t   *cond,    pthread_mutex_t *mutex, const struct timespec *abstime); `
参数说明： 
* cond：等待判断的条件变量； 
* mutex：互斥锁； 
* abstime：等待的时间； 
函数返回值说明：
* 调用成功，返回值为0，否则，返回一个非零的错误码。

pthread_cond_wait和pthread_cond_timedwait区别：
* pthread_cond_timedwait函数将阻塞直到条件变量获取变为真(获得信号)或者经过由abstime指定的时间，也就是说，如果在给定的时间前条件变量没有满足，则返回ETIMEOUT并结束等待。

激活条件变量
* 当线程被条件变量所阻塞后，需要对其激活。Linux 提供了两个函数来激活条件变量，分别是：pthread_cond_signal和pthread_cond_broadcast，这些函数的具体的说明如下：

需要的头文件如下：
`#include <pthread.h>  `
函数格式如下： 
`int pthread_cond_signal(pthread_cond_t *cond);  `
`int pthread_cond_broadcast(pthread_cond_t *cond); ` 
参数说明：
* cond：需要被激活的条件变量

函数返回值说明： 
* 调用成功，返回值为0，否则返回一个非零的错误码。

**注意**：pthread_cond_signal激活一个等待条件变量成立的线程，当存在多个等待线程时，按照入队顺序激活其中的一个；而pthread_cond_broadcast则是激活所有等待的线程。

3. **注销条件变量**
当一个条件变量使用完毕后，必须进行清除。Linux 提供了一个pthread_cond_destroy函数来注销一个条件变量，这个函数的具体的说明如下：

需要的头文件如下：
`#include <pthread.h>  `
函数格式如下：
`int pthread_cond_destroy(pthread_cond_t *cond);`
参数说明：
* cond：需要被注销的条件变量

函数返回值说明：
* 调用成功，返回值为0，否则返回一个非零的错误码。

**注意**：只有在没有线程等待一个条件变量时，该条件变量才能被注销，否则返回EBUSY。

案例演示1:
编写一个程序，使用条件变量对一个全局变量进行加锁。详细代码如下所示：  
```
#include <stdio.h> 
#include <pthread.h>
int globalNumber = 1; 
//初始化一个普通互斥锁number_lock 
pthread_mutex_t number_lock; 
//初始化一个条件变量number_cond 
pthread_cond_t number_cond;
void *addNumer(void *arg) 
{ 
    pthread_mutex_lock(&number_lock);  
    pthread_cond_wait(&number_cond, &number_lock);  
    globalNumber++;  
    pthread_mutex_unlock(&number_lock);  
    return NULL;  
}
int main() 
{ 
    pthread_cond_init(&number_cond, NULL);  
    pthread_t thread1, thread2;  
    pthread_create(&thread1, NULL, addNumer, NULL);  
    pthread_create(&thread2, NULL, addNumer, NULL);  
    //激活所有等待的线程  
    sleep(1);     //保证有线程处于等待状态后再发激活信号  
    pthread_cond_broadcast(&number_cond);  
    pthread_join(thread1, NULL);  
    pthread_join(thread2, NULL);  
    printf("globalNumber = %d\n", globalNumber);  
    pthread_cond_destroy(&number_cond);  
    return 0;  
} 
```
将以上代码保存为condThread.c文件，编译执行。可以看到globalNumber变量变为了3。注意：我们需要在激活条件变量前要等待所有线程处于等待条件变量状态，这样才能激活等待的线程。

## 生产者消费者模型

生产者消费者模型一个著名的同步问题。它描述的是：多个生产者来生产产品，并将这些产品提供给消费者去消费。为使生产者和消费者能够并发执行。在两者之间设置了一个公共区域，生产者进入公共区域生产产品并放入其中。消费者进入公共区域并取走产品进行消费。

生产者消费者模型需要满足如下规则：当一个生产者进入公共区域生产产品时，其他生产者和消费者不能同时进入公共区域生产产品或消费产品。当一个消费者进入公共区域消费产品的时候，其它消费者和生产者不能同时进入该区域消费产品或生产产品。也就是说，任意时刻，最多只允许一个生产者或一个消费者进入公共区域。即生产者和消费者必须互斥的访问公共区域。

实现生产者消费者模型
一个普通的生产者消费者模型需要满足以下标准：(1)生产者与生产者之间存在竞争即互斥关系；(2)消费者与消费者之间存在竞争即互斥关系；(3)生产者与消费者之间存在互斥与同步关系。

*案例演示1：*
通过互斥锁和条件变量实现生产者消费者模型。我们使用链表来存在数据，生产者向数据区生产随机数，消费者从数据区读取生产好的数据并打印出来。我们设置当生产者与消费者满足特定的生产与消费标准时，则退出程序。

详细的代码设计为： 
```c
#include <stdio.h> 
#include <pthread.h> 
#include <stdlib.h> 
#include <time.h>
struct msg 
{ 
    int data;    //存放生产的数据  
    struct msg *next;  
};
pthread_cond_t cond;   //条件变量 
pthread_mutex_t mutex;   //互斥锁 
const int max_number = 10;   //生产者与消费者的最大生产和消费任务 
int producer_number = 0;    //生产者已经生产的任务数 
int consumer_number = 0;     //消费者已经消费的任务数 
struct msg *begin = NULL;    //定义数据区头 
struct msg *end = NULL;    //定义数据区尾
/* 
 * 生产者线程 
 */ 
void *producer(void *arg) 
{ 
    while(1)  
    {  
        pthread_mutex_lock(&mutex);  
        //判断生产者生产的数据量是否满足需求(一共生产max_number多个数据)  
        if(producer_number == max_number)  
        {  
            pthread_mutex_unlock(&mutex);  
            pthread_exit(NULL);  
        }  
        //生产数据  
        if(begin == NULL)  
            begin = end = malloc(sizeof(struct msg));  
        else  
            end = end->next =  malloc(sizeof(struct msg));  
        end->data = rand()%100;  
        producer_number++;  
        //激活所有消费者(注意：这里不能用pthread_cond_signal函数来激活)  
        //pthread_cond_signal函数只能一次激活一个线程  
        pthread_cond_broadcast(&cond);  
        pthread_mutex_unlock(&mutex);  
    }  
}
/* 
 * 消费者线程 
 */ 
void *consumer(void *arg) 
{ 
    while(1)  
    {  
        pthread_mutex_lock(&mutex);  
        //判断当前是否有数据可用来消费  
        while(begin == NULL)  
        {  
            //判断消费者消费的数据量是否满足需求(一共消费max_number多个数据)  
            if(consumer_number == max_number)  
            {  
                pthread_mutex_unlock(&mutex);  
                pthread_exit(NULL);  
            }  
            //如果没有数据可消费，则睡眠当前线程  
            pthread_cond_wait(&cond, &mutex);  
        }  
        //消费数据  
        printf("Consumer: %d\n", begin->data);  
        consumer_number++;  
        //将消费后的数据释放掉  
        struct msg *tmp = begin;  
        begin = begin->next;  
        free(tmp);  
        pthread_mutex_unlock(&mutex);  
    }  
}
int main(int argc, char *argv[]) 
{ 
    //设置随机数种子  
    srand(time(NULL));  
    //初始化条件变量和互斥锁  
    pthread_cond_init(&cond, NULL);  
    pthread_mutex_init(&mutex, NULL);  
    pthread_t thread[5];  
    int i;  
    //创建2个生产者用来生产数据  
    for(i = 0; i < 2; i++)  
        pthread_create(&thread[i], NULL, producer, NULL);  
    //创建3个消费者用来消费数据  
    for(i = 2; i < 5; i++)  
        pthread_create(&thread[i], NULL, consumer, NULL);  
    //等待生产者和消费者结束  
    for(i = 0; i < 5; i++)  
        pthread_join(thread[i], NULL);  
    pthread_cond_destroy(&cond);  
    pthread_mutex_destroy(&mutex);  
    return 0;  
} 
```
将以上代码保存为ProducerConsumer.c文件中，编译执行。

















