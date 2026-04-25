# 编程指北

https://csguide.cn/roadmap/cpp/linux_cpp.html

## 二、后台开发的考察方向

一般来说 Linux C/C++ 后台开发方向涉及以下这些基础知识：

### Linux C++后台常考点

- **C/C++ 语言特性和实现原理**
- **计算机网络**
- **网络编程 和 Linux 系统编程**
- **操作系统原理**
- **部分 Linux 内核原理，如内存管理、文件系统、虚拟内存等**
- **Linux 常见命令使用**
- **算法与数据结构**
- **数据库使用及原理**
- **常见 NoSQL组件，如 Redis、Memcached**
- **版本控制 Git & GitHub**
- `Nginx`
- C++ 主流开发框架有 Boost、gRPC、crow 等
- 设计模式

2.2 非必选加分项：

- **分布式相关，如一致性协议比如 Raft 算法、分布式存储等**
- **Docker、K8S 等虚拟化和云计算相关的**
- **系统设计能力，如短链服务、评论服务、Feed流系统、抢红包、秒杀等**

---

## 三、C/C++ 语言基础

### 3.1 基础语言特性(必备)

首先是语言的基础知识，一些关键字和实现原理等：

- **指针、引用、数组、内存**
- **引用与指针区别**
- **C 和 C++ 的一些区别，比如 new、delete 和 malloc、free 的区别**
- **虚机制：虚函数、虚函数表、纯虚函数**
- **继承、虚继承、菱形继承等**
- **多态： 动态绑定，静态多态**
- **重写、重载**
- **智能指针原理：引用计数、RAII（资源获取即初始化）思想**
- **智能指针使用：shared_ptr、weak_ptr、unique_ptr等**
- **一些关键字的作用：static、const、volatile、extern**
- **四种类型转换：static_cast, dynamic_cast, const_cast, reinterpret_cast**
- **STL部分容器的实现原理，如 vector、deque、map、hashmap**

### 3.2 进阶语言特性(推荐

- **模板特化、偏特化，萃取 traits 技巧**
- **编译链接机制、内存布局（memory layout）、对象模型**
- **C++11 部分新特性，比如右值引用、完美转发等**

### 3.3 书籍&视频

#### 	《C++ Primer》

这本书基本包括了 C++ 11 的全部特性，最好把前面三部分：C++基础、C++标准库、类设计者的工具都看一遍，我当时花了一两个月断断续续看到了第16章模板那里。

#### 	Effective 系列

包含 **《Effective C++》、《More Effective C++》、《Effective STL》**
第一本是重点，光看《C++ Primer》缺少实践的话，大概率还写不出合格的 C++ 代码，而《Effective C++》就是通过 55 条非常具体的做法告诉你什么样才是符合 C++ 编码规范的，可以缩短你写出合格 C++ 代码的时间，减少踩坑，强烈推荐必读，后面两本优先级稍低，可以有时间再读。

#### 	《STL 源码剖析》和《深度探索 C++ 对象模型》

看完 Primer 和 Effective，你应该已经能够比较熟练的使用C++了，但是还缺少对 C++ 底层实现机制的认识。比如虚函数表、成员变量布局等，同时对于 STL 库可能也仅仅停留在使用上。
推荐的这两本可以分别完善你在 C++ 底层实现和 STL 源码、原理上的认识。

而面试如果问 C++ 的比较深入的话，就会涉及 STL 源码和 C++ 一些底层机制。

#### 	侯捷老师视频

以上书籍同时建议和侯捷老师的视频配合服用，效果更佳。
直接在 B 站搜索「候捷 C++」即可，主要有以下几个系列（最新消息：目前公开视频网站平台几乎都已经下架了）：

- **《C++内存管理》**
- **《`STL`源码分析》**
- **《C++ `STL`与泛型编程高级》**
- **《C++11 新特性》**

### 3.4 C++ 进阶提高

### 3.5 C++ 系统学习路线

## 四、操作系统

### [4.3 操作系统必备知识点](https://csguide.cn/roadmap/cpp/linux_cpp.html#_4-3-操作系统必备知识点)

这部分看完你应该对下面这些话题有一个清晰认知了：

- **操作系统由哪些构成**
- **进程的状态、切换、调度**
- **进程间通信方式（共享内存、管道、消息）**
- **进程和线程的区别**
- **线程的实现方式（一对一、多对一等）**
- **互斥与同步（信号量、管程、锁）**
- **死锁检测与避免**
- **并发经典的问题：读者写者、哲学家就餐问题**
- **为什么需要虚拟内存，MMU 具体如何做地址转换的**
- **内存为什么分段、分页**
- **页面置换算法**
- **文件系统是如何组织的**
- **虚拟文件系统（VFS）是如何抽象的**
- ...

### [4.4 Linux 内核原理](https://csguide.cn/roadmap/cpp/linux_cpp.html#_4-4-linux-内核原理)

但是这还不够，看完偏理论的书，当面试官问「进程和线程的区别」时。

大概只能回答出「进程是资源分配的最小单位，线程是CPU调度的最小单位，balabala...」这样正确却普通的答案。

但是如果你了解 Linux 内核的实现，就可以实际出发，讲讲 Linux 中进程和线程是如何创建的，区别在哪里。

比如在 Linux 中进程和线程实际上都是用一个结构体 `task_struct`来表示一个执行任务的实体。进程创建调用`fork` 系统调用，而线程创建则是 `pthread_create` 方法，但是这两个方法最终都会调用到 `do_fork` 来做具体的创建操作 ，区别就在于传入的参数不同。

深究下去，你会发现 Linux 实现线程的方式简直太巧妙了，实际上根本没有**线程**，它创建的就是进程，只不过通过参数指定多个进程之间共享某些资源（如虚拟内存、页表、文件描述符等），函数调用栈、寄存器等线程私有数据则独立。

这样是不是非常符合理论书上的定义：同一进程内的多个线程共享该进程的资源，但线程并不拥有资源，只是使用他们。

这也算符合 Unix 的哲学了— KISS（Keep It Simple, Stupid）。

但是在其它提供了专门线程支持的系统中，则会在进程控制块（PCB）中增加一个包含指向该进程所有线程的指针，然后再每个线程中再去包含自己独占的资源。

这算是非常正统的实现方式了，比如 Windows 就是这样干的。

但是相比之下 Linux 就显得取巧很多，也很简洁。

对于进程、线程这块你还可以把 fork、vfork、clone 、pthread_create 这些模块关系彻底搞清楚，对你理解 Linux 下的进程实现有非常大的帮助。

说了这么多，就是想强调一下理论联系实际的重要性。

特别是操作系统，最好的实践就是看下 Linux 内核是怎么实现的，当然不是叫你直接去啃 Linux 源码，那不是一般人能掌握的。

最好的方式是看书，书的脉络给你理得很清晰。

### 4.5 Linux 原理的书

#### 	《Linux内核设计与实现》

推荐看下这本书，讲得恰到好处，既讲清了内核实现的要点，又不会通篇源码。
这本书重点关注

- **第 3 章进程管理**
- **第 5 章系统调用**
- **第12章内存管理**
- **第13章虚拟文件系统**
- **第 15 章进程地址空间**

这些章节属于操作系统核心部分，其它如中断处理、块 IO、设备管理根据你自己兴趣选择看下就可以了。
基本上做到这里，操作系统就没什么大问题了。

### 4.6 系统学习操作系统

关于系统学习操作系统，操作系统实验等内容可以看这篇文章: [如何系统学习操作系统?](https://csguide.cn/roadmap/basic/os.html)

## 五、计算机网络

需要掌握的网络协议和知识：

- **HTTP、TCP、IP、ICMP、UDP、DNS、ARP**
- **IP地址、MAC地址、OSI七层模型（或者 TCP/IP 五层模型）**
- **`HTTPS`安全相关的：数字签名、数字证书、`TLS`**
- **常见网络攻击：局域网ARP泛洪、`DDoS`、TCP SYN Flood、XSS等**

### 推荐书籍

#### 	《计算机网络：自顶向下方法》

#### 	《网络是怎样连接的》

#### 	《TCP/IP详解卷1：协议》

#### 	《图解HTTP》

### 网络细节

###  从 URL 输入到页面展现到底发生什么

最后别忘了自己回答一遍那被问烂了、写烂了的问题：
**从 URL 输入到页面展现到底发生什么?**

## 六、网络编程

Linux 下网络编程核心的包括系统编程和网络 IO 两个部分：

- **进程间通信方式： 信号量、管道、共享内存、socket 等**
- **多线程编程：互斥锁、条件变量、读写锁、线程池等**
- **五大 IO 模型：同步、异步、阻塞、非阻塞、信号驱动**
- **高性能 IO 两种模式：Reactor 和 Proactor（ 但是 Linux 下由于缺少异步 IO 支持，基本没有 Proactor**
- **IO 复用机制：epoll、select、poll（破解 C10K 问题的利器）**

### 推荐的书

#### 	《Unix网络编程》& 《Unix环境高级编程》

#### 	《Linux高性能服务器编程》

#### 	《Linux多线程服务器编程》

## 七、项目

### 7.1 HTTP服务器（WebServer)

### 7.2  网络库

### 7.3 RPC 

### 7.4 网络聊天室

## 八、系统级编程

## 九、数据库

## 十、算法和数据结构



---

# 程序员鱼皮

## 一、C++语法基础

### **知识点**

- 基础概念
  - 变量
  - 常量
  - 关键字
  - 数据类型
  - 运算符
  - 表达式
  - 控制结构
    - 条件分支
    - 循环



- 开发工具
- 函数
  - 函数重载
  - 默认参数
- 基本数据结构
  - 数组
  - 字符串



- 内存管理
  - 内存模型
  - 动态内存分配
  - 动态内存释放
- 指针
  - 空指针
  - 野指针



- 引用
  - 函数参数引用
  - 返回值引用
- 结构体
  - 定义
  - 访问



- 命名空间
- 面向对象编程
  - 类
  - 对象
  - 封装
    - 成员变量
    - 成员函数
    - this 指针
  - 构造函数
  - 析构函数
  - 拷贝构造函数
  - 静态成员
  - 友元
    - 友元函数
    - 友元类



- 运算符重载
- 继承
  - 对象初始化顺序
  - 同名成员问题
  - 多继承
  - 虚继承
  - 菱形继承
- 多态
  - 虚函数
  - 纯虚函数
  - 抽象类
  - 虚析构函数
  - 纯虚析构函数



- 异常处理
  - 抛出
  - 捕获
  - 异常类型
  - 标准异常
- STL
  - 容器
    - vector
    - string
    - list
    - pair
    - set
    - map
    - deque
    - stack



- 迭代器
- 函数对象
- 谓词
  - 一元谓词
  - 二元谓词
- 内置函数对象
- 算法
  - 排序
  - 查找



- 类型转换
- 模板
  - 函数模板
  - 类模板
- 泛型
- I / O 操作
  - 文件操作

## 二、C++进阶

### **知识点**

- RAII
- C++ 11 新特性
  - 自动类型推导
  - lambda 表达式
  - 智能指针
  - 移动语义
  - 右值引用
  - 标准多线程库
  - nullptr 关键字



- 类型转换
  - static_cast
  - reinterpret_cast
  - dynamic_cast
  - const_cast
- 异常处理
  - 栈解旋
  - 异常接口声明
  - 异常对象生命周期



- 工具
  - 编译工具
    - GCC
  - 构建工具
    - CMake



- 调试工具
  - GDB
- 静态分析工具
  - Clang Static Analyzer



- 编码规范
  - Google C++ Style
- 程序执行原理
  - 编译
  - 链接
    - 静态链接
    - 动态链接



- STL 容器实现原理
  - vector
  - string
  - list
  - pair
  - set
  - map
  - deque
  - stack

## 三、计算机基础

- 计算机导论（计算机基本概念）
- 数据结构和算法
- 操作系统
- 计算机网络

## 四、软件开发通用

### **知识点**

主要包括以下 5 部分：

- 企业项目研发流程
- Git & GitHub
- Linux 系统
- 设计模式
- 软件工程

## 五、后端开发通用

### **知识点**

- 数据库

- 开发框架

- 包管理工具

- Redis

- 消息队列

- Nginx

  https://blog.csdn.net/hyfsbxg/article/details/122322125

- 微服务

- 容器

- 架构设计

## 六、C++项目实战

- 编程语言
  - 用 C 语言实现自己的编程语言：[https://buildyourownlisp.com/](https://link.zhihu.com/?target=https%3A//buildyourownlisp.com/)

- 开发工具
  - 开发自己的文本编辑器：[https://viewsourcecode.org/snaptoken/kilo/](https://link.zhihu.com/?target=https%3A//viewsourcecode.org/snaptoken/kilo/)
- 工具库
  - 手写简易 STL：[https://github.com/Alinshans/MyTinySTL](https://link.zhihu.com/?target=https%3A//github.com/Alinshans/MyTinySTL)
  - 简单 JSON 库：[https://github.com/dropbox/json11](https://link.zhihu.com/?target=https%3A//github.com/dropbox/json11)

- 开发框架
  - 网络编程库 muduo：[https://github.com/chenshuo/muduo](https://link.zhihu.com/?target=https%3A//github.com/chenshuo/muduo)
  - 微信 RPC 框架 phxrpc：[https://github.com/Tencent/phxrpc](https://link.zhihu.com/?target=https%3A//github.com/Tencent/phxrpc) （简化版的微信后台 RPC 框架，冲鹅厂的同学推荐看）
- 服务器
  - 轻量级 Web 服务器学习：[https://github.com/qinguoyi/TinyWebServer](https://link.zhihu.com/?target=https%3A//github.com/qinguoyi/TinyWebServer)
  - 高性能 web 服务器项目：[https://github.com/linyacool/WebServer](https://link.zhihu.com/?target=https%3A//github.com/linyacool/WebServer)

- 分布式系统
  - MIT6.824 中文教程：[https://mit-public-courses-cn-translatio.gitbook.io](https://link.zhihu.com/?target=https%3A//mit-public-courses-cn-translatio.gitbook.io/mit6-824/)

## 七、C++求职备战

C++ 同学的面试重点主要分为 3 个大方向：

1. C++ 语言本身
2. 计算机基础
3. C++ 领域技能（比如后端、嵌入式、游戏开发等）

### **C++ 面试题**

1. 是否关注过 C++ 的版本更新？比如 C++ 11 新增了哪些新特性？
2. 什么是 C++ 的虚函数和纯虚函数？它们分别有什么作用？
3. 如果虚函数是有效的，那为什么不把所有函数设为虚函数？
4. 什么是 C++ 的多态？它是怎么实现的？
5. C++ 函数重载和覆盖有什么区别？
6. 什么是 C++ 的智能指针？它有什么作用？有哪些种类？

### **计算机基础面试题**

数据结构与算法

**算法和数据结构面试题库：**[https://www.mianshiya.com/bank/1824727406021644290](https://link.zhihu.com/?target=https%3A//www.mianshiya.com/bank/1824727406021644290%3FshareCode%3Ddoceh5)

操作系统

1. 什么是进程和线程？二者有什么区别？
2. 操作系统是如何做到进程阻塞的？
3. 有哪些常见的进程通信方式？
4. 进程调度算法有哪些？
5. 线程是如何实现的？

计算机网络

1. 介绍下计算机网络分层结构，各层有哪些常用协议？
2. TCP 和 UDP 协议有什么区别？分别适用于什么场景？
3. TCP 为什么需要三次握手和四次挥手？为什么不是两次握手、四次握手？为什么不是三次挥手？

### **C++ 后端面试题**

1. 你用过哪些 C++ 网络编程库或 web 开发框架？
2. 你用过哪些 C++ 日志框架？
3. 什么是 socket 编程？C++ 中怎么进行 socket 编程？

- GitHub C++ 专区：[https://github.com/topics/cpp](https://link.zhihu.com/?target=https%3A//github.com/topics/cpp)
- GitHub C++ 内容合集：[https://github.com/fffaraz/awes](https://link.zhihu.com/?target=https%3A//github.com/fffaraz/awesome-cpp)

## 鱼皮编程导航

[编程导航 - 程序员一站式编程学习交流社区，做您编程学习路上的导航员](https://www.codefather.cn/)

## 鱼皮面试刷题

[面试鸭 - 2026程序员面试题库大全 | 10000+Java/前端/Python面试题免费刷](https://www.mianshiya.com/)

# 个人C++

## 设计模式

- 单例模式
- 工厂模式

## 数据结构

- 树
- 图
- 排序
- 哈希

## 项目

# 目前在学

- C++设计模式
  - [x] 单例模式
  - [x] 工厂模式
    - 简单工厂模式
    - 工厂模式
    - 抽象工厂模式
  - [ ] 桥接模式
  - [ ] 
- QT入门
  - [x] QT入门.md
  - [ ] QT2.md
- 咕泡云课堂的项目
- 咕泡云课堂的就业指导

具体情况:
27届双非本科,

网络工程专业

期望城市没有,

期望薪资:能进实习就行,想找暑假实习,就是想问问目前学什么,然后实习后再学什么

把C++课程内容就是断断续续学完了,没有做过什么具体项目;上学期就是没事的时候刷了刷牛客上的一些C++选择题

学完我就去学 操作系统,网络编程这方面知识,自学了Linux高性能服务器编程这本书

然后Mysql懂一点,之前学过SQLserver课,

然后现在就在学一点QT,只会一点控件

然后刷题的话,只刷过非常基础的题,现在把好多东西都忘了,之前学数据结构的时候上级自己实现过就是树啊图啊这类的实现,但是目前都忘光了

目前没项目,好多东西都无从下手,就是到底现在该干什么,不想就是一个东西一直学那个
