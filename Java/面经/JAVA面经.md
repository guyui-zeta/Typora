# 一、多线程

## 1.Java线程和操作系统线程的区别

https://blog.csdn.net/CringKong/article/details/79994511

- **进程和线程的解释**
  - **进程（Process）：**系统进行资源分配和调度的基本单元
  - **线程（Thread）：**系统进行运算调度（执行）的最小单元【加入线程时候，os直接调度执行线程，进程只是分配资
- **Java线程在JDK1.2前后的区别**
- 【JDK1.2之前，Green Threads-用户及线程】不是由本地OS直接调度的线程，而是JVM虚拟机调度。这种线程的管理调度发生在用户空间而不是内核空间。OS只能看到进程，而看不到线程。
  - 优点：线程的调度只是在用户态，减少了操作系统从内核态到用户态的切换开销。
  - 缺点：操作系统不知道线程的存在，因此当一个线程进行系统调用时，出现缺页中断而导致线程阻塞，测试操作系统会阻塞整个进程，即时这个进程的其他线程还在工作。并且，由于用户空间没有时钟中断机制，一个线程长时间不释放CPU会导致其他线程忙等

![image-20210924094159186](D:/Typora/Typora_Note/Java/%E9%9D%A2%E7%BB%8F/JAVA%E9%9D%A2%E7%BB%8F.assets/image-20210924094159186.png)

- 【JDK1.2之后-同内核级线程】JVM选择了更加稳健且方便使用的操作系统原生的线程模型，通过系统调用，将程序的线程交给了操作系统内核进行调度。

![image-20210924094250610](D:/Typora/Typora_Note/Java/%E9%9D%A2%E7%BB%8F/JAVA%E9%9D%A2%E7%BB%8F.assets/image-20210924094250610.png)

- **Java线程的状态/生命周期与OS线程的区别**
  - **Java线程**
    - 就绪态和运行态合为Runable
    - Waiting态分为Timed Waiting、Waiting、Blocked

![image-20210924095246260](D:/Typora/Typora_Note/Java/%E9%9D%A2%E7%BB%8F/JAVA%E9%9D%A2%E7%BB%8F.assets/image-20210924095246260.png)

- **OS线程：**

![image-20210924095330383](D:/Typora/Typora_Note/Java/%E9%9D%A2%E7%BB%8F/JAVA%E9%9D%A2%E7%BB%8F.assets/image-20210924095330383.png)

## 2.线程的调度

在多道程序系统中，进程的数量往往多于处理器的个数，进程争用处理器的情况在所难免。处理器调度是对处理器进行分配，就是从就绪队列中，按照一定的算法，选择一个进程并将处理器分配给他运行，以实现进程的并发执行。

一个作业从提交开始知道完成，往往要经历一下三级调度：

- **高级调度（作业调度）**

  - 按一定的原则从外存中处于后备队列的作业中挑选一个（或多个）作业，给他们分配内存等必要资源，并建立相应的进程（建立PCB），使他们获得竞争处理机的权利。

- **中级调度（内存调度）**

  引入了虚拟存储技术之后，可将暂时不能运行的进程调到外存等待，等她重新具备了运行条件且内存又稍有空闲时，再重新调入内存——挂起状态。为了提高内存利用率和系统吞吐量

  - 决定将哪个处于挂起状态的进程重新调入内存

- **低级调度（进程调度）**

  - 按照某种方法和策略从就绪队列中选取一个进程，将处理机分配给他，进程调度是OS中最基本的一种调度，在一般的操作系统中都必须配进程调度，且频率很高。

![image-20210924110619593](D:/Typora/Typora_Note/Java/%E9%9D%A2%E7%BB%8F/JAVA%E9%9D%A2%E7%BB%8F.assets/image-20210924110619593.png)



## 3.死锁的四个必要条件、预防和避免【破解】方法

https://blog.csdn.net/zhangpower1993/article/details/89518780

- **死锁的概念**
  - 多个并发进程因争夺系统资源而产生相互等待的现象
  - 各进程相互等待对方手里的资源，导致各进程都阻塞，无法向前推进的现象
  
- **死锁产生的四个必要条件：**
  
  - 互斥【摄像头】：某种资源一次只允许一个进程访问，即该资源一旦分配给某个进程，其他进程就不能再访问，直到该进程访问结束
  - 不可抢占：进程所获得的资源在未使用完之前，不能由其他进程强行夺走，只能主动释放
  - 占有且等待：一个进程本身占有资源（一种或多种），同时还有资源未得到满足，正在等待其他进程释放资源
  - 循环等待：存在一个进程链，使得每个进程都占有下一个进程所需要的的至少一种资源
  
- **死锁的预防**

  可以通过破坏死锁产生的4个必要条件来预防死锁，由于资源互斥是资源使用的固有特性，是无法改变的【为了系统安全性，必须保护互斥性】

  - 破坏“占有且等待”条件
    - 方法1【静态分配方法】：在进程开始之前，一次性申请在整个运行过程中用到的所有资源
    - 方法2：进程只获得初期使用的资源，便开始运行，在运行过程中逐步释放掉分配到的已经使用完毕的资源，然后再去申请新的资源
  - 破坏“不可抢占”条件
    - 方法：当一个已经有了一些资源的进程在提出新的资源请求没有得到满足时，他必须释放已经保持的所有资源，待以后需要使用的时候在重新申请。【短暂的释放】
  - 破坏“循环等待”条件
    - 方法【顺序资源分配法】： 将系统中的所有资源顺序编号，紧缺的、稀少的采用较大的编号，在申请资源时必须按照编号的顺序进行（从小往大申请）

- **死锁的避免【银行家算法】**

  在使用前进行判断，只允许不会产生死锁的进程申请资源

  两种避免方法：

  - 1.如果一个进程的请求会导致死锁，则不启动该进程
  - 2.如果一个进程的增加资源请求会导致死锁，则拒绝该请求

  具体实现通常使用银行家算法：【安全序列 ：分配资源后，进程使用完资源会归还资源】
  
- 银行家算法步骤：

  - 1.检查此次申请是否超过了之前声明的最大需求数
  - 2.检查此时系统剩余饿可用资源是否还能满足这次请求
  - 3.试探着分配，更改各数据结构
  - 4.用安全性算法检查此次分配知否会导致系统进入不安全状态
    - 安全性算法步骤：
    - 检查当前的剩余资源是否能满足某个进程的最大需求，如果可以，就把该进程加入到安全序列，并把该进程所持有的的资源全部回收。不断重复上述过程，看最终是否能让所有进程都进入到安全序列中

## 3.5死锁的java代码实现

- 利用Synchronized（对象锁）的方式，锁住互相占有，互相需求的资源

```java
public class LockTest {
   public static String obj1 = "obj1";
   public static String obj2 = "obj2";
   public static void main(String[] args) {
      LockA la = new LockA();
      //新开一个线程并执行
      new Thread(la).start();
      LockB lb = new LockB();
      //新开一个线程并执行
      new Thread(lb).start();
   }
}
class LockA implements Runnable{
   public void run() {
      try {
         System.out.println(new Date().toString() + " LockA 开始执行");
         while(true){
            synchronized (LockTest.obj1) {
               System.out.println(new Date().toString() + " LockA 锁住 obj1");
               Thread.sleep(3000); // 此处等待是给B能锁住机会
               synchronized (LockTest.obj2) {
                  System.out.println(new Date().toString() + " LockA 锁住 obj2");
                  Thread.sleep(60 * 1000); // 为测试，占用了就不放
               }
            }
         }
      } catch (Exception e) {
         e.printStackTrace();
      }
   }
}
class LockB implements Runnable{
   public void run() {
      try {
         System.out.println(new Date().toString() + " LockB 开始执行");
         while(true){
            synchronized (LockTest.obj2) {
               System.out.println(new Date().toString() + " LockB 锁住 obj2");
               Thread.sleep(3000); // 此处等待是给A能锁住机会
               synchronized (LockTest.obj1) {
                  System.out.println(new Date().toString() + " LockB 锁住 obj1");
                  Thread.sleep(60 * 1000); // 为测试，占用了就不放
               }
            }
         }
      } catch (Exception e) {
         e.printStackTrace();
      }
   }
}
```



## 4.java中的i++是不是一个原子操作？

不是，由于java操作涉及多线程，线程间会互相干扰。

- i++操作分为三步：
  - 1.取值i
  - 2.i++
  - 3.赋值i

任何一步都可能被打断，所以不是原子操作

- 举例：
  - i=0;两个线程分别对i进行++100次,值是多少？取值范围是2-200
  - 当i=99时：此时i=99，i+1，赋值时被b线程打断了，并进行赋值，赋值完后又被a打断了

![image-20211004160444322](D:/Typora/Typora_Note/Java/%E9%9D%A2%E7%BB%8F/JAVA%E9%9D%A2%E7%BB%8F.assets/image-20211004160444322.png)



- 补充：
  - 但是在Redis中的incr key自增操作是一个原子操作
  - 因为redis是单线程+多路IO实现的，单线程中的,可以在**==单条指令==**完成的操作都是原子操作，因为中断只能发生在指令之间



## 5.多线程中wait（）-【notify（）】和sleep（）的区别

- wait（）和sleep（）
  - sleep（）
    - sleep（）方法是Thread类中的静态方法
    - sleep（）方法使当前线程暂停执行指定的时间，让出cpu给其他线程，时间到了会自动恢复运行状态
    - 调用sleep（）方法，不会释放对象锁
    - sleep（）方法会抛出异常
    - 可以在任何地方使用
    - **==会自动恢复运行状态==**
  - wait（）
    - wait（）是Object类中的成员方法
    - wait（）使当前线程暂停**==并让出同步资源锁==**，以便其他正在等待的线程得到资源并运行
    - 调用wait（）方法会放弃对象锁，进入等待此对象锁的等待锁定池
    - wait（）方法不会抛出异常
    - wait（）方法只能在同步方法和同步代码块中使用
    - **==需要notify（）/notifyAll（）唤醒指定的线程或所有线程==**
- notify（）方法
  - 唤醒处于等待状态的线程
  - 只能在同步方法和同步代码块中运行，如果有多线等待唤醒的线程，就随机挑选一个唤醒。
  - notify（）方法调用后，当前线程不会立刻释放对象锁，要等到当前线程执行完毕后再释放锁。

## 5.两个线程交替打印1-100

- 流程：
  - 1.创建一个类，实现Runable接口，并作为对象锁
  - 2.在同步代码块Synchronized中
  - 3.先唤醒notify（），由于后面会进入wait（），所以必须手动唤醒。也是为了唤醒另外一个线程，并进入就绪状态
  - 4.进行i++
  - 5.进入wait（）状态，让出资源并释放锁，此时另外一个唤醒且就绪状态的线程会争夺锁并打印i

```java
public class ThreadTest implements Runnable{
    int i = 1;
    @Override
    public void run() {
        while(true){
            //this只带ThreadTest，对象锁
            synchronized (this){
                //唤醒另外一个正在等待的线程，唤醒之前wait()的线程
                //由于上一次i++完之后，线程进入了wait，所以需要手动notify
                notify();
                try {
                    //暂停当前进程100ms，为了有交替打印的效果
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                if(i <= 100){
                    System.out.println(Thread.currentThread().getName() + ":" + i);
                    i++;
                    try {
                        //放弃资源让出锁，等待
                        wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }

    public static void main(String[] args) {
        //只有一个ThreadTest线程对象
        ThreadTest threadTest = new ThreadTest();
        //创建两个线程，对ThreadTest对象进行多线程操作
        Thread t1 = new Thread(threadTest);
        Thread t2 = new Thread(threadTest);

        t1.setName("线程1");
        t2.setName("线程2");

        t1.start();
        t2.start();
    }
}
```



## 6.多线程中的数据可见性

















# -------------------------------------------

# 二、操作系统

## 1.系统的调用

https://blog.csdn.net/weixin_44478378/article/details/107270951

- 进程在系统上的运行分为两个级别：
  - 用户态（user mode）：用户态运行的进程可以直接读取用户程序的数据
  - 系统态（kernel mode）：系统态运行的程序**可以访问计算机的任何资源**，不受限制

平常我们运行的程序都是用户态的，如果想要将进程运行在系统态，需要利用系统调用

- 系统的调用
  - 在我们运行的用户程序中，**凡是与系统级别的资源有关的操作【设备管理、文件管理、进程控制、进程通信、内存管理】**都必须通过系统调用的方式向OS提出服务请求，由OS代为完成

## 2.虚拟内存

- 定义：
  - 虚拟内存是一种计算机系统内存管理技术，它使得应用程序认为它拥有连续可用的内存，即一个连续完整的地址空间。而实际上，它通常是被分割成多个物理内存碎片，还有部分暂时存储在外部磁盘存储器上，在需要时进行数据交换
  - 【在操作系统的管理下，在用户看来似乎有一个比实际内存大得多的内存，就是虚拟内存，物理内存没有变，在逻辑上实现了扩充】
- 实现过程
  - 1.在程序装入时，可以将程序中很快会用到的部分装入内存，暂时用不到的部分留在外存中
  - 2.在程序执行过程中，当所访问的信息不在内存时，由操作系统负责将所需信息从外存调入内存
  - 3.若内存空间不够时，由操作系统负责将内存中暂时用不到的信息换到外存中

## 3.死锁、饥饿和死循环的区别

- 死锁：
  - 各进程互相等待对方手里的资源，导致各进程都阻塞，无法向前推进的现象
- 饥饿：
  - 由于长期得不到想要的资源，某进程无法向前推进的现象。
- 死循环：
  - 某进程执行过程中一直跳不出某个循环的现象。

|        | 区别                                                         |
| ------ | ------------------------------------------------------------ |
| 死锁   | 死锁一定是“循环等待对方手里的资源”导致的，因此，如果有死锁现象，那么至少有两个或两个以上的进程同时发送死锁。另外，发生死锁的进程一定处于**==阻塞态==** |
| 饥饿   | 可能只有一个进程发送饥饿。发送饥饿的进程既可能是**==阻塞态==**（长期得不到I/O设备），也可能是**==就绪态==**（长期得不到处理机） |
| 死循环 | 可能只有一个进程发生死循环。死循环的进程可以上处理机运行（可以是**==运行态==**） |



# -------------------------------------------

# 三、计算机网络

## 1.TCP/IP四层模型&OSI七层模型&优化五层模型

![image-20210927100212633](D:/Typora/Typora_Note/Java/%E9%9D%A2%E7%BB%8F/JAVA%E9%9D%A2%E7%BB%8F.assets/image-20210927100212633.png)

### ①**TCP/IP四层模型**

- **应用层：**
  - 应用程序之间通信的层，如简单的电子邮件传输（SMTP）、万维网www（HTTP）、文件传输协议（FTP）
- **传输层**
  - 负责传输数据，提供节点间的数据传送服务，如传输控制协议（TCP）、用户数据报协议（UDP），给数据包加入传输数据并把它传输到下一层
- **网络层**
  - 负责提供基本的数据封包传送功能，让每一块数据包都能够到达目的主机，如网际协议（IP）来传送数据
- **网络接口层**
  - 链路层不是通常意义上的一层，是一个与物理网络的接口，处于网络层和数据链路层之间，没有具体规定细节

### ②**OSI七层参考模型**

【国际标准化组织ISO提出的统一模型】

- **总体模型图：**
  - 端到端：主机的A与B，进程的端口号之间
  - 点到点：中间会设计路由器或者交换机

![image-20211008204339573](JAVA面经.assets/image-20211008204339573.png)



- **应用层**
  - 所有能和用户交互产生网络流量的程序【QQ、邮箱】
  - 典型应用层服务：文件传输（FTP）、万维网www（HTTP）、电子邮件（SMTP）

![image-20211008204030661](JAVA面经.assets/image-20211008204030661.png)

- **表示层**
  - 呈现在设备/屏幕上的东西，用于处理两个通信系统中交换信息的表示方式
  - 功能一：数据格式变换【翻译官】eg：比特流→jpg图片
  - 功能二：数据加密解密【涉及到密码的信息会加密解密】
  - 功能三：数据压缩和恢复【视频会先压缩，传过去在解压】

![image-20211008204016348](JAVA面经.assets/image-20211008204016348.png)

- **会话层**
  - 表示层会用会话层的功能，向表示层实体/用户进程**提供建立连接**并在连接上有序地传输数据。建立同步（SYN），单个会话之间独立不影响
  - 功能一：建立、管理、终止会话
  - 功能二：使用校验点可使会话在通信失效时从校验点/同步点继续恢复通信，实现数据同步【适用于传输大文件】

![image-20211008203949763](JAVA面经.assets/image-20211008203949763.png)

- **传输层**
  - 负责主机中两个进程的通信，即端到端的通信【因为不同的进程，会有**端口号**来区分】传输单位是报文段或用户数据报
  - 功能一：可靠传输、不可靠传输【TCP和UDP】
  - 功能二：差错控制【解决乱序、丢包问题】
  - 功能三：流量控制【发送/接收速度匹配】
  - 功能四：复用分用
    - 复用：各个应用层进程可同时使用下面传输层的服务
    - 分用：运输层把收到的信息分别交付给上面应用层中相应的进程【用端口号区分】

![image-20211008205752765](JAVA面经.assets/image-20211008205752765.png)

- **网络层**
  - 主要任务是把分组从源端传到目的端，为分组交换网上的不同主机提供通信服务，网络层传输单位是数据报。
  - 功能一：路由选择【最佳路径】
  - 功能二：流量控制
  - 功能三：差错控制
  - 功能四：拥塞控制【控制整体速度】

![image-20211008210546447](JAVA面经.assets/image-20211008210546447.png)

- **数据链路层**
  - 主要任务是把网络层传下来的数据组装成帧。数据链路层/链路层的传输单位是帧
  - 功能一：成帧（定义帧的开始和结束）
  - 功能二：差错控制：帧错+位错
  - 功能三：流量控制
  - 功能四：访问（接入）控制，控制对信道的访问

![image-20211008210827374](JAVA面经.assets/image-20211008210827374.png)

- **物理层**
  - 把比特流转成电信号的形式，传到物理媒体上。主要任务是在物理媒体【无线电、双绞线】上实现比特流的透明传输。物理层传输单位是比特。
  - 透明传输：指不管所传数据是什么样的比特组合，都应当能够在链路上传送
  - 功能一：定义接口特性
  - 功能二：定义传输模式：单工、半双工、双工
  - 功能三：定义传输速率
  - 功能四：比特同步
  - 功能五：比特编码

![image-20211008211142541](JAVA面经.assets/image-20211008211142541.png)

### ③优化五层模型

- OSI优点：每一个层次功能很清晰
- TCP/IP优点：层次简单，层之间没有交叉

五层模型综合了OSI和TCP/IP的优点

![image-20211008212123242](JAVA面经.assets/image-20211008212123242.png)





## 2.Http的版本区别

https://www.cnblogs.com/surui/p/11669346.html

- HTTP

  是浏览器与服务器之间以明文的方式传送内容的一种互联网通信协议

  - HTTP0.9
    - 仅支持GET请求，不支持请求头
  - HTTP1.0
    - 默认短连接（一次请求建立一次TCP连接，请求完就断开），支持GET、POST、HEAD（与GET相似，不返回请求体）请求
  - HTTP1.1
    - 默认长连接（一次TCP连接可以多次请求）；支持POST、GET、PUT、DELETE、PATCH等六种请求方式
    - 增加host头、支持虚拟主机、支持断点续传功能【host头：请求服务器的域名/IP地址和端口号】
  - HTTP2.0
    - **多路复用，降低开销**（一次TCP连接可以处理多个请求）
    - 服务器主动推送（相关资源一个请求全部推送）
    - 解析基于二进制，解析错误少，更加高效（HTTP1.x解析基于文本）
    - 报头压缩，降低开销

- HTTPS

  - 在HTTP的基础上主要基于SPDF协议结合SSL/TLS加密协议，客户端依靠证书验证服务器的身份传递加密信息的通信协议

# -------------------------------------------

# 四、计算机组成原理



# -------------------------------------------

# 五、数据库

https://blog.csdn.net/qq_22222499/article/details/79060495

## 0.Mysql语句大全

- 一、几个基本函数的执行顺序
  - 先从这个表里查询（from），然后经过条件的过滤（where）后，分组（group by）之后才能使用分组函数，如果不满意可以再进行过滤（having），查完后做一个排序（order by），最后查找输出（select）

```mysql
select    6
from      1
where     2
group by  3
having    4
order by  5
```

- 二、多表联合查询

```mysql
1.内连接：能匹配的信息都显示出来
(inner) join 
on
2.外连接：有主副之分，把主表信息都匹配并显示出，没有就显示null
left join  //左边是主表
on
right join //右边是主表
on
3.结果合并
select ...
union
select ...
```

- 三、分组函数&聚合函数

```mysql
1.分组函数（只能在分组之后使用，不能在where中使用）
count()计数
sum()求和
avg()平均值
max()最大值
min（）最小值
2.聚合函数
通常要和group by一起使用的，”分组+聚合“
count(字段):会统计该字段在表中出现的次数，不统计字段为null的记录
count(1):会统计所有的记录数，包含字段为null的记录
count(*):包含了所有的列，相当于行数，包含字段为null的记录
```

```mysql
1.去重关键字
select distinct salary 
```



- 四、分页查询

```mysql
1.0代表其实位置，1代表取几条数据
limit 0,1
2.0代表其实位置，offset代表往后第几位
limt 1 offset 2//代表第一位往后两位，即第三位
```

- 五、增、删、改语句

```mysql
1.增加数据(字段可以省略不写)
insert into t_student(no,name,sex,classno,birth) values(1,'zhangsan','1','gaosan1ban','1950-10-12');
2.删除数据
delete from t_studen where id = 1
3.修改数据
update dept1 set loc='SHANGHAI', dname='HR' where deptno = 20
```

- 六、建表语句

```mysql
1.创建表
create table 表名(
	no bigint,
    name varchar(255),
    sex char(1),
    classno varchar(255),
    //添加非空约束
    birth char(10) not null
    //添加唯一性约束
    birth char(10) unique
    //添加主键约束
    birth char(10) primary key
);
2.删除表
drop table if exists t_student;
3.表的复制(查出来的临时表复制，持久化)
create table 表名 as select语句：
```

- 七、索引语句

索引其实就是一种数据结构，来加快查询效率（B+树）

```mysql
1.创建索引对象
create index 索引名称 on 表名（字段名）
create index emp_sal_index on emp(sal)
2.删除索引对象
drop index 索引名称 on 表名;
drop index emp_sal_index on emp
```

- 八、事务语句

```mysql
1.开启事务
start transaction
2.提交事务
commit;
3.回滚事务
rollback;
```





- 详细版本：
- 1.查询语句

```mysql
select
	date as d
from
	table;
```

- 2.条件查询

```mysql
select 
	date as d
from
	table
where
	ename = 'SMITH';
```

- 查询的几种情况

  - 范围查询/范围外查询

  ```mysql
  select
  	name
  from
  	emp
  where
  	//范围内查询
  	sal >= 1000 and sal <= 3000;
  	//范围外查询
  	sal not in (1000,3000)
  ```

  - 首字母范围查询

  ```mysql
  select
  	name
  from
  	emp
  where
  	name between 'A' and 'C';
  ```

  - 查询不为空/为空

  ```mysql
  select
  	name
  from
  	emp
  where
  	//不为空
  	comm is not null
  	//为空
  	comm is null
  ```

  - 与查询/或操作

  ```mysql
  select
  	ename as name, job
  from
  	emp
  where
  	//或操作
  	job = 'saleman' or job = 'manager';
  	//与操作
  	job = 'saleman' and sal > 2000;与查询/或操作
  ```

  - 模糊查询

  适配符%和_

  %：表示多个字符

  _：表示任意1个字符

  ```mysql
  select
  	name
  from
  	emp
  where 
  	//%多字符模糊查询
  	name like '%O%'
  	//_单字符模糊查询
  	name like '_O_'
  ```

- 2.排序

  - 升序/降序排列

  ```mysql
  select 
  	name,sal
  from
  	emp
  order by 
  	sal asc //升序ascending
  order by 
  	sal desc //降序descending
  ```

- 3.分组函数

  - 分组函数都是针对“某一组”数据进行操作，且不能在where子句中使用，因为group by是在where执行之后才会执行的，分组函数必须分完组之后才能用。
    - count计数
      - count(*)和count()的区别：前者表示统计总记录条数，后者表示count(name)统计name不为null的数据总数量
    - sum求和
    - avg平均值
    - max最大值
    - min最小值

- 4.单行处理函数

  - ifnull（comm，0）空处理函数
    - 由于数据库运算中，只要有null参与，结果一定是null，所以需要换算成0

- 5.group by分组

  - 按照某个字段过着某些字段进行分组，例如：group by job，按照job的类型进行分组

  ```mysql
  select
  	max(sal)
  from
  	emp
  //每个岗位的最高薪资
  group by 
  	job
  //每个部门的每个岗位的最高薪资
  group by 
  	deptno,job
  ```

- 6.having

  - 对分组之后的数据进行再次过滤

  where搞不定的情况：例如where后不能根分组函数，但是having后面可以跟

  ```mysql
  select
  	deptno, avg(sal)
  from
  	emp
  group by 
  	deptno 
  having 
  	avg(sal) > 2000;
  ```

- 多表联合查询

  - 内连接：等值连接（on条件是=）、非等值连接（on条件是范围）、自连接（连自己表），两表没有主副之分，能匹配的都查出来

  ```mysql
  select
  	e.ename, d.dname
  from
  	emp e
  (inner) join
  	dept d
  on
  	e.deptno = d.deptno
  ```

  - 外连接：把主表查询出来，左外连接&右外连接

  ```mysql
  select
  	a.ename as '员工', b.ename as '领导'
  from
  	emp a
  left join
  	emp b
  on
  	a.mgr = b.empno
  ```


## 1.Mysql的四个特性

ACID（原子性、一致性、隔离性、永久性）

## 2.MySQL索引有哪些分类？

https://segmentfault.com/a/1190000037683781

- 添加索引的SQL语句

  ```mysql
  mysql>ALTER TABLE `table_name` ADD PRIMARY KEY ( `column` ) 
  ```

- 按数据结构分类：
  - B+ tree索引
    - 用B+树结构，实现IO次数更少的查询
  - Hash索引
    - 优点：检索效率非常高，可以一次定位，而不需要像树一样从根节点到支节点
    - 缺点：无法实现范围查询
  - Full-text索引
- 按物理存储分类：
  - 聚簇索引
    - 每个叶子节点存储了一行完整的表数据
  - 二级索引（辅助索引、非聚簇索引）
    - 叶子节点存储的是表数据的地址，数据另存一块空间
- 按字段特性分类：
  - 主键索引
    - 建立在主键上的索引，一张数据表只能有一个
  - 唯一索引
    - 建立在UNIQUE字段上的索引，一张表可以有多个唯一索引，列值可为空，不会发生重复冲突
  - 普通索引
    - 建立在普通字段上的索引
  - 前缀索引
    - 对字符类型字段的前几个字符或对二进制类型字段的前几个bytes建立的索引

## 3.数据库设计-三大范式

https://blog.csdn.net/weixin_43971764/article/details/88677688

- **什么是设计泛式？**

  - 设计表的依据，按照这三个范式设计的表**不会出现数据冗余**

- **三大范式**

  - **第一范式【列不可再分】**：任何一张表都应该有主键，并且每一个字段都有原子性，不可再分【不能是数组、集合这样的非原子数据】

  

  - **第二范式【属性完全依赖主键】**：建立在第一范式的基础上，所有非主键字段完全依赖主键，不能产生部分依赖【存在多个主键时，会出现部分依赖的情况】

  ![image-20210929084849282](D:/Typora/Typora_Note/Java/%E9%9D%A2%E7%BB%8F/JAVA%E9%9D%A2%E7%BB%8F.assets/image-20210929084849282.png)

  - **第三范式【属性直接依赖于主键】**：建立在第二范式之上，不能存在传递依赖。即不能存在：非主键列m既依赖于全部主键，又依赖于非主键列n的情况。
  
  ![image-20211003101844894](D:/Typora/Typora_Note/Java/%E9%9D%A2%E7%BB%8F/JAVA%E9%9D%A2%E7%BB%8F.assets/image-20211003101844894.png)

## 4.char和varchar的区别

- char（m-个数）：定长不可变字符串

  - 定长：当前字段可以存储的字符个数是固定的

  - 不可变：字段在硬盘上的存储空间是固定的

    -  sex char(3)

       insert into test1 values('abc')  【a】 【b】 【c】

       insert into test1 values('ef')  【e】 【f】 【空格】

- varchar（m）：定长可变字符串

  - 定长：当前字段可以存储的字符个数是固定的

  - 可变：字段在硬盘上存储字符空间可以根据实际情况进行【缩小】

    -  ename varchar(3)

        insert into test1 values('abc') #硬盘 【a】【b】【c】

        insert into test1 values('ef')  #硬盘 【e】【f】-会缩小成2

## 5.索引的本质

https://blog.csdn.net/woshimeilinda/article/details/104532214

- 数据库在查询一张表时，有两种检索方式：

  - 全表扫描——很慢
  - 索引检索——效率很高

- 索引的作用

  - 如果不添加索引，select * from where...即使扫描到该值，还是会继续扫描下去，直到全盘扫描，所以查询会很慢。——全表扫描
  - 使用索引，会把这一列以二叉树（B+树）的数据结构存储，可以快速找到，效率快一半

  ![image-20211026222547452](JAVA面经.assets/image-20211026222547452.png)

- 索引的本质：

  索引其实是一种数据结构

  - 就是把一列单独拿出来，用一些特殊的数据结构（二叉树、B+树）来存储，以达到快速检索的效果

- 索引的实现原理

  - 通过B+ tree缩小扫描范围，底层索引进行了排序、分区，索引会携带数据在表中的“物理地址”，最终通过索引检索到数据之后，可以直接获取相关的物理地址，然后定位到数据

```mysql
select * from emp where ename = "SMITH"
通过索引转换后就变成了:【底层原理】
select * from emp where 物理地址 = 0x123
```



- 

# -------------------------------------------

# 六、JAVASE

## 1.接口和抽象类的区别

​	

|               | 抽象类                                                       | 接口                                    |
| ------------- | ------------------------------------------------------------ | --------------------------------------- |
| 修饰          | abstract修饰                                                 | interface修饰                           |
| 继承/实现     | 不能实例化，只能继承                                         | 只能实现，可以接口extends接口实现多继承 |
| 类中属性/方法 | 含有抽象方法一定是抽象类，抽象类不一定有抽象方法（被重写掉了）；变量可以是所有类型 | 只能是抽象方法和静态变量                |
| 构造方法      | 抽象类有，但是不能用，只能子类调用                           | 没有构造方法                            |
| 作用          | 过滤接口的不想要实现的抽象方法                               |                                         |



## 2.String和StringBuffer的区别

String:

- 0.String是final修饰的，不能被继承，且不是静态类，有String.方法的
- 1.是不可变对象，修改时会重新建立对象，并指向
- 2.为不可变对象,一旦被创建,就不能修改它的值。
- 3.对于已经存在的String对象的修改都是重新创建一个新的对象,然后把新的值保存进去。
- 4.String是final类,即不能被继承。

StringBuffer:

- 1.是一个可变对象,当对它进行修改的时候不会像String那样重新建立对象。
- 2.它只能通过构造函数来建立，StringBuffer subffer=new StringBuffer();
- 3.对象被建立以后,在内存中就会分配内存空间,并初始保存一个null,通过它的append方法向其赋值subffer.append(“hello word”);

字符串连接操作中**StringBuffer**的效率要明显比**String**高;
**String**对象是不可变对象,每次操作String都会建立新的对象来保存新的值。
**StringBuffer**对象实例化后,只对这一个对象操作。

## 3.静态类【内部静态类】和非静态类的区别

https://blog.csdn.net/weixin_45104211/article/details/98640693

- 本意是想区分静态方法和实例方法的区别，没想到引出了下列知识点
- 注意：
  - 一般是不用static修饰类的，static修饰类是匿名内部类。【静态类=静态内部类】，所以静态类是以成员变量的形式，存在于外部类的里面



- 静态的特点：

  - 1.全局唯一，任何一次修改都是全局性的影响
  - 2.只加载一次，优先于非静态
  - ==3.使用方法上不依赖于实例对象==
  - 4.生命周期属于类级别，从JVM加载开始到JVM加载结束

- 静态类和非静态类之间的区别
  - （1）内部静态类不需要有指向外部类的引用。但非静态内部类需要持有对外部类的引用。【指创建实例时】
  - （2）非静态内部类能够访问外部类的静态和非静态成员。静态类不能访问外部类的非静态成员。他只能访问外部类的静态成员。
  - （3）一个非静态内部类不能脱离外部类实体被创建，一个非静态内部类可以访问外部类的数据和方法，因为他就在外部类里面。

- 静态（内部）类和非静态内部类的创建

  - 静态（内部）类：不需要创建外部类的实例对象

  ```java
  // 创建静态内部类的实例
  OuterClass.NestedStaticClass printer = new OuterClass.NestedStaticClass();
  // 创建静态内部类的非静态方法
  printer.printMessage();  
  ```

  

  - 非静态类：需要外部类的实例对象

  ```JAVA
  // 为了创建非静态内部类，我们需要外部类的实例【指向外部类的引用】
  OuterClass outer = new OuterClass();    
  OuterClass.InnerClass inner = outer.new InnerClass();／／这样new出来的
  // 调用非静态内部类的非静态方法
  inner.display();
  ```

  

# -------------------------------------------

# 七、JVM

## 1.双亲委派

- **什么是双亲委派？**

  - JDK1.2期间被引入，目前成为最常用的**类加载器实现方式**

- **双亲委派的作用：**

  - 避免原始类被覆盖：
    - 当用户编写一个Object类，放入程序中加载，如果没有双亲委派机制，就会出现重复的Object类。有了双亲委派，每次加载类，都会去问加载器父类是否能加载，如果父类找到了JDK中的原类，就不会再次加载了。

- **双亲委派的加载流程：【针对开发者自己写的Test类】**

  - 1.首先会发送给App ClassLoader程序加载器
  - 2.AppClassLoader不会直接去加载，而是会委派给父类加载器，即ExtClassLoader扩展类加载器
  - 3.ExtClassLoader也不会直接去架子啊，而是会委派给父类加载器，即BootStrapClassLoader启动类加载器
  - 4.BootStrapClassLoader此时已经没有父类，会默认在jdk/lib目录下搜索是否存在该类，此处不存在【如果存在就不会再重复加载】，所以这时会给子类加载器一个反馈【老子做不了】
  - 5.ExtClassLoader收到父类加载器的反馈，会同样在默认的jdk/lib/ext下搜索是否存在该类，此处也不存在，所以往下反馈
  - 6.AppClassLoader最后收到后，明白jdk里不存在该类，所以会自己去加载

- **java中的类加载器**

  - BootStrap ClassLoader（启动类加载器）
    - 默认加载的是jdk\lib目录下jar中诸多类
  - Extension ClassLoader（扩展类加载器）
    - 默认加载jdk\lib\ext\目录下jar中诸多类
  - App ClassLoader（应用程序类加载器）
    - 负责加载开发人员所编写的诸多类
  - User ClassLoader（自定义类加载器）
    - 当上述类加载器解决不了的特殊情况，或存在特殊要求时，可以自定实现类加载逻辑
  - 关系图：

  ![image-20211008191847354](JAVA面经.assets/image-20211008191847354.png)

# -------------------------------------------

#	八、JavaEE和WEB



# -------------------------------------------

# 九、MyBatis框架



# -------------------------------------------

# 十、Spring框架

https://www.cnblogs.com/yanggb/p/11004887.html

## 1.IOC

- **IOC简介**
  - IOC（Inversion Of Controll，控制反转）是将原本在程序中手动创建对象的控制权，交给Spring容器来管理。将对象之间的相互依赖【对象中的对象】的关系交给IOC容器来管理，并由IOC容器完成对象的注入，简化开发。
- **IOC的使用**
  - 当我们需要创建一个对象时，只需要配置好配置文件/注解即可，不用考虑对象是如何被创建出来的
  - Spring时代一般通过XML文件配置Bean，后到SpringBoot流行注解配置@Bean【SpringBoot中使用配置类+@Bean实现IOC】

| 区别           | 配置文件                                  | 配置类                                                       |
| -------------- | ----------------------------------------- | ------------------------------------------------------------ |
| 添加组件的方式 | 在配置文件汇中用<bean></bean>标签添加组件 | 在配置类中用@Bean来添加组件                                  |
| 实现           | <bean></bean>                             | @Bean【@Bean作用：将方法的返回值添加到容器中，容器中这个组件的默认Id就是方法名】 |

![image-20211008215353982](JAVA面经.assets/image-20211008215353982.png)

### 2.依赖注入DI（Dependency Injection）

- 依赖注入简介：

  - 依赖注入是一种**消除类之间依赖关系**的设计模式。例如：A类要依赖B类，A类不再直接创建B类，而是把这种依赖关系配置在外部xml文件（或config文件），然后由Spring容器根据配置信息创建、管理Bean类

- 依赖注入的实现方法：

  DI是根据构造方法进行赋值的，所以必须要有构造方法

  - 基于XML的DI

  ```xml
  1.简单类型的set注入
  <bean id = "xx" class="xxx">
      <property name = "属性名字" value = "此属性的值"/>
      一个property只能给一个属性赋值
  </bean>
  2.引用类型的set注入
  <bean id="xxx" class="yyy">
      <property name="属性名称" ref="bean的id（对象的名称）">
  </bean>
  
  ```

  - 基于注解的DI【用的多】

    在类中加入Spring的注解

  ```java
  @Component(value = "mystudent")
  public class Studen{
      
  }
  ```

  ​		在Spring的配置文件中，加入一个【组件扫描器】的标签吗，说明注解的位置

  ```xml
  <context:component-scan base-package="com.zeta.entity"></context:component-scan>
  ```

## 3.AOP

https://blog.csdn.net/Guyui233/article/details/118207519?spm=1001.2014.3001.5501

- **AOP简介**

  - AOP（Aspect-Oriented Programming，面向切面编程），能将那些与业务无关的功能（事务处理、日志管理、**权限控制**）封装起来，减少重复代码，降低模块间的耦合性，提升可扩展性和可维护性。

- **AOP原理【动态代理】：**

  - **动态代理简介：**

    - 动态代理在创建代理对象时更加灵活，动态代理类的字节码在程序运行时，由java反射机制动态产生，他会根据需要，在程序运行期间，动态的为目标对象创建代理对象。

  - **动态代理的实现：**

    JDK动态代理需要公共的接口；CGLIUB不需要公共接口，只需要一个非抽象类就可以实现

    ![image-20211009100900234](JAVA面经.assets/image-20211009100900234.png)

    - **JDK**

      - 必须借助一个公共接口，才能产生代理对象，代理类需要实现InvocationHandler接口，通过里面的invoke方法，实现增强功能

      ```java
      public class JdkProxyExample implements InvocationHandler{
      	//真实对象
      	private Object target = null;
      	/**
           * 建立代理对象和真实对象的代理关系，并返回代理对象
           * newProxyInstance方法有三个参数：
           * loader:目标对象的类加载器
           * interfaces：动态代理类需要实现的接口数组，
           * handler：代理对象
           */
      	public Object getProxy(Object target){
      		this.target = target;
      		return Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(),this);
      	}
      	/**
           * 代理方法逻辑【增强】
           */
           @Override
           public Object invoke(Object proxy,Method method,Object[] args) throws Throwable{
           	System.out.println("进入代理逻辑方法");
           	System.out.println("在调度真实对象之前的服务");
           	Object obj = method.invoke(target,args);//相当于调用sayHelloWold方法【方法反射】
           	System.out.println("在调度真实对象之后的服务");
           	return obj;//方法返回值
           }
      }
      
      ```

      

    - **Cglib**

      - 不需要提供公共接口，只需要一个非抽象类即可实现动态代理

      ```java
      public  void testCGLIBProxy(){
      	CglibProxyExample cpe = new CglibProxyExample();
      	ReflextServiceImpl obj = (ReflextServiceImpl)cpe.getProxy(ReflextServiceImpl.class);
      	obj.sayHello("张三");
      }
      //测试结果
      调用真实对象前
      Hello 张三
      调用真实对象后
      
      ```

      

- **AOP的实现**

  - Spring AOP实现：属于运行时增强
  - AspectJ AOP实现：编译时增强，SpringAOP已经集成了AspectJ

- **AOP的使用流程：**

  - 1.加入aspectj依赖

  - 2.创建目标类：接口和他的实现类

  - 3.创建切面类

    - 在类的上面加入@Aspect
    - 在类中定义方法，即增强的功能代码，在方法上加入aspectj的通知注解，例如@Before，参数为指定切入点的表达式![image-20211009101838386](JAVA面经.assets/image-20211009101838386.png)

  - 4.创建spring的配置文件：

    - 声明对象，把对象交给容器统一管理
    - aspectj中的自动代理生成器标签会自动生成代理对象，不需要手动声明proxy

    ![image-20211009102231476](JAVA面经.assets/image-20211009102231476.png)

  - 5.测试：

    - 直接使用即可

    ![image-20211009102438877](JAVA面经.assets/image-20211009102438877.png)

    





## 4.常用的注解

| @Component                  | 创建对象，等同于<bean>的功能，value是对象的名称              |
| --------------------------- | ------------------------------------------------------------ |
| @Repository【仓库，存储库】 | 用在持久层类上面，放在dao的实现类上，表示创建dao对象         |
| @Service                    | 用在业务层上面，放在Service的实现类上，创建Service对象       |
| @Controller                 | 用在控制器上面，接收用户提交的参数，显示请求的处理结果       |
| @Value                      | 简单类型的属性赋值，放在属性定义的上面                       |
| @Autowired                  | Spring框架提供的注解，实现引用类型的赋值，通过自动注入原理，自动注入实例 |
| @Resource                   | JDK的注解，同上                                              |

# 5.Spring中的bean的生命周期

完整版：https://www.cnblogs.com/javazhiyin/p/10905294.html

简单版：https://www.jianshu.com/p/1dec08d290c1

- 传统Java中的Bean的生命周期

  - 以后java关键字new进行Bean的实例化，然后就可以使用，不用时，则由java自动进行垃圾回收

- Spring中的Bean的生命周期

  - 1.实例化：1和2对应构造方法和setter方法的注入
  - 2.属性赋值：
  - 3.初始化
  - 4.销毁

  ![image-20211009104832355](JAVA面经.assets/image-20211009104832355.png)

# -------------------------------------------

# 十一、SpringMVC框架



# -------------------------------------------

# 十二、SpringBoot

## 1.SpringBoot的优点

- 一、独立运行
  - SpringBoot内嵌了各种Servlet容器，Tomcat等，不需要打成war包部署到Tomcat容器中，只要打包成一个可执行的**jar包**就能独立运行，所有的依赖包都在一个jar包中。
- 二、简化配置
  - spring-boot-starter-web启动器自动依赖其他组件，简化了maven的配置
- 三、自动配置
  - SpringBoot能根据当前类路径下的类、jar包来自动配置bean，例如：添加一个spring-boot-starter-web启动器就能拥有web的功能，无需其他配置
- 四、无代码生成和XML配置
  - 配置过程中无代码生成，也无需XML配置文件就能完成所有的配置工作
- 五、应用监控
  - 提供一系列端点可以监控服务及应用，做健康检查

## 2.SpringBoot的核心注解是哪几个？

- **@SpringBootApplicaiton在启动类上的注解**，其包含：

  - **@SpringBootConfiguration：**

    - 组合了@Configuration，实现配置文件的功能

  - **@EnableAutoConfiguration：**

    ​	开启自动配置功能

    - @AutoConfigurationPackage:自动配置包
    - @Import({Registrar.class})：

- **==@EnableAutoConfiguration作用：【及其重要】==**

  - 1.将主配置类（@SpringBootApplication标注的类）的所在包及下面所有子包的所有组件扫描到Spring容器中【所以能扫描到Controller，Service。在传统SSM中需要在xml中配置扫描包的路径】
  - 2.扫描所有jar包路径下的“META-INF/spring.factories”中配置的所有xxxAutoConfiguration类都加载到容器中。这些组件的属性是从对应的properties类中获取的
    - xxxxAutoConfiguration：自动配置类，给容器中添加组件【通过@Bean返回组件】
    - xxxProperties：封装配置文件中相关属性

## 3.SpringBoot自动装配原理

- 无SpringBoot的ssm时代
  - 需要编写web.xml、springmvc.xml和mybatis.xml，很麻烦
- SpringBoot自动配置装载时代
  - SpringBoot有一个全局配置文件：application.properties或者application.yml，只需要在这个全局文件里面配置各种各样的参数即可。例如：server.port = 88
- **==SpringBoot自动装配原理【面试总结版】==**
  - SpringBoot启动时会通过**@EnableAutoConfiguration注解**找到**META-INF/spring.factories**配置文件中所有的自动配置类，并对其进行加载，这些自动配置类都是以**xxxAutoConfiguration结尾**的，还会将starter类下所有包中，我们自己写的组件【controller、service】记载。他实际上是一个java配置类形式的Spring容器配置类，他能通过该以**xxxProperties结尾**命名的类中获取在全局配置文件中配置的属性，如**server.port**，而xxxProperties类是通过@ConfigurationProperties注解与全局配置文件中的对应属性进行绑定的





# -------------------------------------------

# 十三、分布式

## 	1.分布式和微服务的区别

https://zhuanlan.zhihu.com/p/138645236

- 分布式：
  - 只要将一个项目拆分成了多个模块，并将这些模块分开部署，就是分布式
- 微服务：
  - 一种非常细粒度的垂直拆分【满足原子性】
- 区别：
  - 分布式：拆了就是
  - 微服务：细粒度的垂直拆分【需要满足原子性，拆到不能再拆的微小服务】

![image-20211003101712971](D:/Typora/Typora_Note/Java/%E9%9D%A2%E7%BB%8F/JAVA%E9%9D%A2%E7%BB%8F.assets/image-20211003101712971.png)

## 2.分布式锁

https://www.cnblogs.com/liuqingzheng/p/11080501.html

- 什么是分布式锁？

  - 在开发中，如果需要对某一个共享变量进行多线程同步访问时，可以用锁来进行处理——但是仅限于单机应用。发展到分布式集群时代，一个应用需要部署到几台机器上然后做负载均衡。如下图。

  - 下图中，变量A存在于三个服务器内存中，如果不加任何控制的话，变量A同时都会分配一块内存，三个请求发过来同时对这个变量操作，显然结果是不对的！即使不是同时发过来，变量A之间不存在共享，也不具有可见性，处理的结果也是不对的！

  - 因此需要分布式锁，来实现一种跨机器的互斥机制来控制共享资源的访问

    ![image-20211021105436537](JAVA面经.assets/image-20211021105436537.png)

- 分布式锁应该具备哪些条件

  - 1、在分布式系统环境下，一个方法在同一时间只能被一个机器的一个线程执行；
  - 2、高可用的获取锁与释放锁；
  - 3、高性能的获取锁与释放锁；
  - 4、具备可重入特性；
  - 5、具备锁失效机制，防止死锁；
  - 6、具备非阻塞锁特性，即没有获取到锁将直接返回获取锁失败

- 分布式锁的三种实现方式

  - 基于数据库实现分布式锁

    - 核心思想：

      - 在数据库中创建一个表，表中包含“方法名”等字段，并在方法名字段上创建**==唯一索引==**，想要执行某个方法，就使用这个方法名向表中插入数据，成功插入则获取锁，执行完成后删除对应的行数据来**释放锁**

      ![image-20211021110030221](JAVA面经.assets/image-20211021110030221.png)

  - 基于缓存（Redis）实现分布式锁

    - 实现思想：

      - 获取锁的时候，使用setnx加锁，并用expire命令为锁添加一个超时时间，超过改时间则自动释放锁，锁的value值为随机生成的UUID，通过id在释放锁的时候进行判断。
      -  过这个时间则放弃获取锁
      - 释放所得时候，通过UUID判断是不是该锁，若是该锁，则执行delete进行锁释放

      ![image-20211021110716981](JAVA面经.assets/image-20211021110716981.png)

  - 基于Zookeeper实现分布式锁

    - 待补充







# -------------------------------------------

# 十四、微服务



# -------------------------------------------

# 十五、Redis6

## 0.Redis使用步骤和流程

## 1.Redis和Mysql事务的区别

- Redis事务的三个特性
  - 1.**单独的隔离操作**：事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断；
  - 2.**没有隔离级别**的概念：队列中的命令没有提交之前都不会实际的被执行，因为事务提交前任何指令都不会被实际执行，也就不存在”事务内的查询要看到事务里的更新，在事务外查询不能看到”这个让人万分头痛的问题；
  - 3.**不保证原子性**：redis同一个事务中如果有一条命令执行失败，其后的命令仍然会被执行，没有回滚；

区别：

- 事务命令的区别
  - mysql
    - Start transaction：显示的开始一个事务
    - Commit：提交事务，将对数据库镜像的所有的修改变成永久性的
    - Rollback：结束用户的事务，并撤销现在正在进行的未提交的修改
  - Redis：
    - Multi：标记事务的开始
    - Exec：执行事务的commands队列
    - Discard：结束事务，并清楚commands队列
- 默认状态的区别
  - mysql：
    - mysql会默认开启一个事务，且缺省设置为自动提交，即每成功执行一次sql，一个事务就会马上commit，所以不能rollback
    - 非默认情况下，可以rollback
  - Redis：
    - redis默认不会开启事务，即command会立即执行，而不会排队，并不支持rollback







# 十六、消息中间件RabbitMQ

## 0.RabbitMQ使用步骤和流程

