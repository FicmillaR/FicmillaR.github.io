> 这是一篇正在编辑中的文章
> 作为一个学生熟练工，如果一个老师的introduce让我听得很舒服，就大概率是宝藏课程

# 操作系统：设计与实践
* 本文属与工作后本科课程补课系列，操作系统部分来自于b站《南大22年操作系统》课程
  

# 为什么要学？

1. 《微积分》这门课其实把几个时代的知识串到了一起
* Newton时代：学习牛顿时代的微积分求解技巧是没有意义的，有了基本的微积分概念就可以用python做应用
* Cauchy时代：非常难，可以卡出bug（Weierstrass函数、Peano曲线）>> 不需要记技巧，也可以用python包呀~
* 现代应用：优化、有限元、PID... 真正有用的部分：用来实现神经元凸优化、现代工业，机械臂稳定这些有用的

2. 为什么要学操作系统？
* 每天都在用的东西，还没搞明白：窗口创建/[Ctrl+C退出程序](https://stackoverflow.blog/2017/05/23/stack-overflow-helping-one-million-developers-exit-vim/)？
* 128个处理器，程序只用1个
* 每天都在用的东西，实现不出来：浏览器、编译器...

3. 操作系统给你有关编程的全部
* 学会后内力大增 >> special/advance/paper 才能满足你
* 摸到可以**卖钱**的软件在刚刚被创造出来时的样子 >> 概念映射


# what is？
* making it easy to run programs,allowing programs to share memory, programs to interact with devices, other fun stuff.
> 一个定义滴水不漏的同时也味同嚼蜡

* 操作系统是如何一路编程这样滴呢？
* 三个重要线索：
  * 硬件
  * 程序
  * 操作系统（管理软件的软件）


1. 1940s的计算机
   电子计算机的实现：    
   * 程序直接从操作硬件
   * 不需要画蛇添足的程序来管理它

2. 1950s的计算机
   * I/O速度已经严重低于处理器速度，中断机制出现
   
   * 更复杂的任务，希望调用API，Fortran诞生（使用打孔板编程）:
   
     ![Punch-card--fortran (1)](os_1.assets/Punch-card--fortran%20(1).jpg)
   
   * 好多个卡片，一个CPU >> os诞生：
     * “批处理系统”= 程序自动切换（换卡）+库函数API
     * Disk Operation Systems（DOS）：操作系统开始出现设备，文件，任务等对象和API
  
3. 1960s的计算机
   * 大内存：可以把好几个程序同时放到内存里，but只有一个cpu
   * 运行一个程序，cpu+mem 30s，打印机 5分钟，这五分钟浪费了
   * 有了进程的概念
   * 内存隔离：A程序bug不会影响B程序
   * os控制程序分时切片执行：A 100ms后os直接介入 换B上 100ms 再换A >> Multics(1965,MIT)

4. 1970s+
   * 与现今的计算机已无巨大差异

5. 今天
   * “虚拟化”硬件资源
   * 更复杂的处理器（非对称多处理器/更多的硬件机制/...）和内存
   * 更多的设备和资源


# 理解操作系统
1. 操作系统服务谁？
   * 程序 = 状态机 >> 多线程linux
2. (设计/应用视角)操作系统为程序提供什么服务?
   * 操作系统=对象+API >> POSIX + 部分Linux特性
3. (实现/硬件)如何实现操作系统提供的服务?
   * 操作系统=C程序 >> 完成初始化后操作系统就成为 interrupt/trap/fault handler
   * 涉及xv6,mini操作系统
  >　关于从按下开机键到操作系统开始接管的步骤,可以看[这篇文章](https://www.ruanyifeng.com/blog/2013/02/booting.html)


# 程序员的核心能力
1. STFW/RTFM
2. 命令行工具
3. 不惧怕写代码
4. 出bug时机器永远是对的