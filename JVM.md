\# What? 工作原理？

是Java虚拟机，也就是Java语言的运行环境。



<img src="https://cdn.nlark.com/yuque/0/2026/png/63012352/1776758940087-af291263-a909-4b52-9f1c-135af1058fbb.png" width="456" title="" crop="0,0,1,1" id="ucce17b1e" class="ne-image">



作用：



\+ 运⾏并管理Java源码⽂件所⽣成的Class⽂件，

\+ 在不同的操作系统上安装不同的JVM，从⽽实现了跨平台的保证。



\*\*为什么使用JVM？\*\*



主要是为Java语言提供Write Once，Run Anywhere的能力。实际上，一次编写，到处运行这个能力本身是不可能实现的。因为不同的操作系统和硬件最终执行的指令会有较大的差异。



而Java虚拟机就是解决这个问题的，它能根据不同的操作系统和硬件差异，生成符合这个平台机器指令。它就相当于一个翻译工具，在window下，翻译成window可执行的指令，在linux下，翻译成linux下可执行的指令。



除了这个因素，我还认为自动回收垃圾这个功能也是原因之一，它让开发者省去了垃圾回收这个工作，减少了程序开发的复杂性。



\*\*目前主流的JVM HotSpot的架构图\*\*



<img src="https://cdn.nlark.com/yuque/0/2026/png/63012352/1776759025261-be4a68ba-2320-43de-8ea6-83aa65bdc14d.png" width="451.2" title="" crop="0,0,1,1" id="u9f31bce1" class="ne-image">



大致流程是把一个<u>class文件</u>通过类加载器加载进系统，然后放到不同的区域，通过编译器编译。



\+ \*\*Class Files：Class⽂件是由源码⽂件⽣成\*\*

\+ \*\*Class Loader Subsystem即类加载机制：\*\*Class⽂件加载到内存中，需要借助Java中的类加载机制。类加载机制分为装载、链接和初始化，其主要就是对类进⾏查找、验证以及分配相关的内存空间和赋值

\+ \*\*Runtime Data Areas也就是通常所说的运⾏时数据区：\*\*Class⽂件进入内存之后，该如何进⾏存储不同的数据以及数据该如何进⾏扭转。



Method Area通常会储存由Class⽂件常量池所对应的运⾏时常量池、字段和⽅法的元数据信息、类的模板信息等；



Heap是存储各种Java中的对象实例；



Java Threads通过线程以栈的⽅式运⾏加载各个⽅法；



Native Internal Thread可以理解为是加载运⾏native类型的⽅法；



PC Register则是保存每个线程执⾏⽅法的实时地址。



\+ \*\*Garbage Collector也就是通常所说的垃圾回收：\*\*对运⾏时数据区中的数据进⾏管理和回收。回收机制可以基于不同的垃圾收集器，⽐如Serial、Parallel、CMS、G1、ZGC等，可以针对不同的业务场景选择不同的收集器，只需要通过JVM参数设置 即可。

\+ \*\*JIT Compiler和Interpreter：\*\*通俗理解就是翻译器，Class的字节码指令通过JIT Compiler和Interpreter翻译成对应操作系统的CPU指令，只不过可以选择解释执⾏或者编译执⾏，在HotSpot JVM默认采用的是这两种⽅式的混合。

\+ \*\*JNI的技术：\*\*找Java中的某个native⽅法是如何通过C或者C++实现的，那么可以通过Native Method Interface来进⾏查找



\# Java内存区域 (运行时数据区） 的组成

JVM在执行Java程序的过程中会把它管理的内存划分成若干个不同的数据区域。



Java虚拟机规范对于运行时数据区域的规定是相当宽松的。以堆为例：堆可以是连续空间，也可以不连续。堆的大小可以固定，也可以在运行时按需扩展。虚拟机实现者可以使用任何垃圾回收算法管理堆，甚至完全不进行垃圾收集也是可以的。







JDM1.8之前：								JDK1.8之后：



<img src="https://cdn.nlark.com/yuque/0/2026/png/63012352/1776606168991-dec84aa4-3ba3-4134-92ab-ee69d43d06c9.png" width="306" title="" crop="0,0,1,1" id="BK3MV" class="ne-image">  <img src="https://cdn.nlark.com/yuque/0/2026/png/63012352/1776606226379-5e06936b-1387-49ad-85f9-c9e932dc7e5d.png" width="306" title="" crop="0,0,1,1" id="u76c19793" class="ne-image">  

线程私有的：程序计数器、虚拟机栈、本地方法栈



\# OutOfMemoryError

该错误的含义是：JVM 没有足够的内存空间分配给新创建的对象，垃圾回收（GC）也无法回收足够内存，最终导致程序崩溃。  







<font style="color:rgb(60, 60, 67);">程序计数器是唯一一个不会出现 </font>`<font style="color:rgb(60, 60, 67);">OutOfMemoryError</font>`<font style="color:rgb(60, 60, 67);"> 的内存区域，它的生命周期随着线程的创建而创建，随着线程的结束而死亡。</font>



<font style="color:rgb(60, 60, 67);">程序计数器主要有两个作用：</font>



\+ <font style="color:rgb(60, 60, 67);background-color:#FBDE28;">字节码解释器</font><font style="color:rgb(60, 60, 67);">通过改变程序计数器来</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">依次读取指令</font><font style="color:rgb(60, 60, 67);">，从而实现代码的流程控制，如：顺序执行、选择、循环、异常处理。</font>

\+ <font style="color:rgb(60, 60, 67);">在多线程的情况下，程序计数器用于</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">记录当前线程执行的位置</font><font style="color:rgb(60, 60, 67);">，从而当线程被切换回来的时候能够知道该线程上</font>内存的作用和组成



\# <font style="color:rgb(60, 60, 67);">关于堆</font>

\## 堆溢出

<font style="color:rgb(60, 60, 67);">堆溢出，就是 </font>`<font style="color:rgb(60, 60, 67);">OutOfMemoryError: Java heap space</font>`



<font style="color:rgb(60, 60, 67);">该错误含义是：JVM 在尝试为新对象分配内存时，堆中已经没有足够的连续空间了，并且经过垃圾回收后，也无法腾出足够的空间。</font>



导致堆溢出的场景主要可以分为两类：



1\. \*\*内存泄漏\*\*<font style="color:rgb(60, 60, 67);">：</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">对象用完了但没被释放</font><font style="color:rgb(60, 60, 67);">，比如 </font>`<font style="color:rgb(60, 60, 67);">static</font>`<font style="color:rgb(60, 60, 67);"> 集合无限增长、 </font>`<font style="color:rgb(60, 60, 67);">ThreadLocal</font>`<font style="color:rgb(60, 60, 67);"> 没调用 </font>`<font style="color:rgb(60, 60, 67);">remove()</font>`<font style="color:rgb(60, 60, 67);"> 。</font>

2\. \*\*内存膨胀\*\*<font style="color:rgb(60, 60, 67);">：</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">短时间内创建了太多对象</font><font style="color:rgb(60, 60, 67);">，比如一次性从数据库查了几百万条数据到 List 里，或者直接把一个大文件整个读进内存。</font>



\## 堆内存的作用和组成

堆是JVM所管理的内存最大的一块，Java堆是<font style="color:rgb(60, 60, 67);">所有线程共享的一块内存区域，在虚拟机启动时创建。</font>\*\*<font style="color:rgb(60, 60, 67);">此内存区域的唯一目的就是存放对象实例，几乎所有的对象实例以及数组都在这里分配内存。</font>\*\*



随着JIT编译器的发展与逃逸分析技术逐渐成熟，栈上分配、标量替换优化技术将会导致一些变化，所有的对象都分配到堆上变得不那么“绝对”，从JDK1.7开始默认开启逃逸分析，<font style="color:rgb(60, 60, 67);">如果某些方法中的</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">对象引用没有被返回或者未被外面使用</font><font style="color:rgb(60, 60, 67);">（也就是</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">未逃逸出去</font><font style="color:rgb(60, 60, 67);">），那么对象可以直接在栈上分配内存。</font>



<font style="color:rgb(60, 60, 67);">Java 堆是</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">垃圾收集器管理的主要区域</font><font style="color:rgb(60, 60, 67);">，因此也被称作 </font>\*\*GC 堆（Garbage Collected Heap）\*\*<font style="color:rgb(60, 60, 67);">。从垃圾回收的角度，由于现在收集器基本都采用</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">分代垃圾收集算法</font><font style="color:rgb(60, 60, 67);">，所以 Java 堆还可以细分为：新生代（ 新生代包含 Eden 和两个 Survivor 区  ）和老年代（也就是Old区）。进一步划分的目的是更好地回收内存，或者更快地分配内存。</font>



JDK7版本及以前，堆内存被分为三部分：新生代内存（Young Generation）、老生代(Old Generation)、永久代(Permanent Generation)。JDK 1.8 版本之后 PermGen(永久代) 已被 Metaspace(元空间) 取代，元空间使用的是本地内存。



<img src="https://cdn.nlark.com/yuque/0/2026/png/63012352/1776685284090-7c8bb5a4-a804-4a56-8aca-7b1a37167172.png" width="456" title="" crop="0,0,1,1" id="ud90eb673" class="ne-image">



大部分情况，对象都会首先在 Eden 区域分配，在一次<font style="background-color:#FBDE28;">新生代垃圾回收后</font>，<font style="background-color:#FBDE28;">如果对象还存活</font>，则会进入 S0 或者 S1，并且对象的<font style="background-color:#FBDE28;">年龄还会加 1</font>(Eden 区->Survivor 区后对象的初始年龄变为 1)，当它的年<font style="background-color:#FBDE28;">龄增加到一定程度（默认为 15 岁</font>），就会被<font style="background-color:#FBDE28;">晋升到老年代</font>中。对象晋升到老年代的年龄阈值，可以通过参数 `-XX:MaxTenuringThreshold` 来设置。不过，设置的值应该在 0-15，否则会爆出以下错误：



`MaxTenuringThreshold of 20 is invalid; must be between 0 and 15`



\## JVM分代年龄为什么是15次？ 可以25次吗？

首先，在JVM的heap内存里面，分为Eden Space、Survivor Space、Old Generation；当使用new关键字创建一个对象时，JVM会在Eden Space 分配一块内存空间来存储这个对象，当Eden Space空间不足时，触发Young GC进行对象回收，那些无法回收的，JVM就将他们转移到Survivor Space ；Survivor Space内部又分为From区和To区，刚从Eden区转移过来的对象会分配到From区，每经历一次Young GC，这些没有办法被回收的对象就会在From区和To区来回移动，每移动一次，这个对象的GC年龄就加1。默认情况下GC年龄达到15的时候，JVM就会把这个对象移动到Old Generation。



其次，一个对象的GC年龄，是存储在对象头里面的，一个Java对象在JVM内存中的布局由三个部分组成，分别是对象头、实例数据、对齐填充。而对象头里面有4个bit位来存储GC年龄，而4个bit位能够存储的最大数值是15。



\## 为什么要将永久代（PermGen）替换为元空间（MetaSpace）呢?

&#x20;1、永久代有一个 JVM 本身设置的固定大小上限，无法进行调整（也就是受到 JVM 内存的限制），而元空间使用的是本地内存，受本机可用内存的限制，虽然元空间仍旧可能溢出，但是比原来出现的几率会更小。



`当元空间溢出时会得到如下错误： java.lang.OutOfMemoryError:MetaSpace`



可以使用 `-XX：MaxMetaspaceSize` 标志设置最大元空间大小，默认值为 unlimited，这意味着它只受系统内存的限制。



`-XX：MetaspaceSize` 调整标志定义元空间的初始大小如果未指定此标志，则 Metaspace 将根据运行时的应用程序需求动态地重新调整大小。



2、元空间里面<font style="background-color:#FBDE28;">存放的是类的元数据</font>，这样加载多少类的元数据就<font style="background-color:#FBDE28;">不由 </font>`<font style="background-color:#FBDE28;">MaxPermSize</font>`<font style="background-color:#FBDE28;"> 控制</font>了, 而由系统的实际可用空间来控制，这样能加载的类就更多了。



3、在 JDK1.8，合并 HotSpot 和 JRockit 的代码时, JRockit 从来没有一个叫永久代的东西, 合并之后就没有必要额外的设置这么一个永久代的地方了。



4、永久代的对象是通过FullGC进行垃圾收集，也就是和老年代同时实现垃圾收集。



替换成元空间以后，简化了Full GC。可以在不进行暂停的情况下并发地释放类数据，同时也提升了GC的性能



\## 程序运行中堆可能会出现什么错误？

<font style="color:rgb(60, 60, 67);">最容易出现的就是 </font>`<font style="color:rgb(60, 60, 67);">OutOfMemoryError</font>`<font style="color:rgb(60, 60, 67);"> 错误，并且出现这种错误之后的表现形式还会有几种，比如：</font>



\+ `\*\*java.lang.OutOfMemoryError: GC Overhead Limit Exceeded\*\*`<font style="color:rgb(60, 60, 67);">：当 JVM 花太多时间执行垃圾回收并且只能回收很少的堆空间时，就会发生此错误。</font>

\+ `\*\*java.lang.OutOfMemoryError: Java heap space\*\*`<font style="color:rgb(60, 60, 67);"> :假如在创建新的对象时, 堆内存中的空间不足以存放新创建的对象, 就会引发此错误。(和配置的最大堆内存有关，且受制于物理内存大小。最大堆内存可通过</font>`<font style="color:rgb(60, 60, 67);">-Xmx</font>`<font style="color:rgb(60, 60, 67);">参数配置，若没有特别配置，将会使用默认值）</font>



\## 堆内存相关的JVM参数有哪些?

\### 堆内存大小控制

\+ `\*\*<font style="color:rgb(60, 60, 67);">-Xms</font>\*\*`<font style="color:rgb(60, 60, 67);"> ：设置 JVM 初始堆内存大小（如</font>`<font style="color:rgb(60, 60, 67);">-Xms512m</font>`<font style="color:rgb(60, 60, 67);">表示初始堆为 512MB）。</font>

\+ `\*\*<font style="color:rgb(60, 60, 67);">-Xmx</font>\*\*`<font style="color:rgb(60, 60, 67);"> ：设置 JVM 最大堆内存大小（如</font>`<font style="color:rgb(60, 60, 67);">-Xmx1g</font>`<font style="color:rgb(60, 60, 67);">表示最大堆为 1GB）。</font>



&#x20;在生产环境中，强烈建议<font style="background-color:#FBDE28;">将 </font>`<font style="background-color:#FBDE28;">-Xms</font>`<font style="background-color:#FBDE28;"> 和 </font>`<font style="background-color:#FBDE28;">-Xmx</font>`<font style="background-color:#FBDE28;"> 设置为相同的值</font>。这样做可以避免 JVM <font style="background-color:#FBDE28;">在运行时根据负载情况动态地收缩和扩展堆内存</font>，这个过程会<font style="background-color:#FBDE28;">引发不必要的 Full GC 和性能抖动</font>，从而提高服务的稳定性和响应速度。



\### 新生代与老年代

\+ `\*\*-Xmn\*\*`：这是最直接控制新生代大小的方式，优先级高于 -`XX:NewRatio`（<img src="https://cdn.nlark.com/yuque/0/2026/png/63012352/1776684951705-50e6148b-fdbd-44b4-aca3-2917b9f16dca.png" width="60" title="" crop="0,0,1,1" id="u63150eca" class="ne-image">）。设置后，老年代的大小就是 `-Xmx` 减去 `-Xmn`。当我们对应用的对象生命周期有明确的判断时（例如，有大量的短生命周期对象），可以直接给新生代一个合适的大小，以达到更好的 GC 性能。

\+ `\*\*-XX:NewRatio\*\*`：这是另一种调节新生代大小的方式，默认值为 2，表示老年代:新生代 = 2:1。因此，新生代默认占整个堆的 1/3。如果设置为 3，则新生代占堆的 1/4。通常在 `-Xmn` 和 `-XX:NewRatio`中选择一个使用即可。

\+ `\*\*-XX:SurvivorRatio\*\*`：设置新生代中 Eden 区与单个 Survivor 区的比例。<font style="background-color:#FBDE28;">默认值为 8</font>，表示<font style="background-color:#FBDE28;"> Eden : From Survivor : To Survivor = 8:1:1</font>。所以 Eden 区占整个新生代的 8/10。这个比例会影响对象能否在新生代中“存活”足够长的时间。如果 Survivor 区太小（即 `-XX:SurvivorRatio` 值过大），Minor GC 后存活的对象可能因为放不下而被迫提前进入老年代，增加 Full GC 的压力。



\### 堆内存溢出相关参数

\+ `\*\*-XX:+HeapDumpOnOutOfMemoryError\*\*` ：当发生`OutOfMemoryError`（OOM）时，自动生成堆转储文件（`.hprof`），记录堆内存对象状态。

\+ `\*\*-XX:HeapDumpPath\*\*` ：指定 OOM 时堆转储文件的保存路径（如`-XX:HeapDumpPath=/logs/heapdump.hprof`），默认生成在程序运行目录。



\# 方法区的常用参数有哪些

<font style="color:rgb(60, 60, 67);">JDK 1.8 之前永久代还没被彻底移除的时候通常通过下面这些参数来调节方法区大小。</font>



```java

//方法区 (永久代) 初始大小

\-XX:PermSize=N 

//方法区 (永久代) 最大大小,超过这个值将会

//抛出 OutOfMemoryError 异常:java.lang.OutOfMemoryError: PermGen

\-XX:MaxPermSize=N 

```



<font style="color:rgb(60, 60, 67);">JDK 1.8 的时候，方法区（HotSpot 的永久代）被彻底移除了（JDK1.7 就已经开始了），取而代之是元空间，元空间使用的是本地内存。下面是一些常用参数：</font>



```java

\-XX:MetaspaceSize=N //设置 Metaspace 的初始（和最小大小）

\-XX:MaxMetaspaceSize=N //设置 Metaspace 的最大大小

```



<font style="color:rgb(60, 60, 67);">与永久代很大的不同就是，如果不指定大小的话，随着更多类的创建，虚拟机会耗尽所有可用的系统内存。</font>



\# <font style="color:rgb(60, 60, 67);">深入辨析堆和栈</font>

\## <font style="color:rgba(13, 13, 13, 0.9);">功能</font>

1\. <font style="color:rgba(13, 13, 13, 0.9);">以栈帧的方式存储方法调用的过程，并存储</font><font style="color:#DF2A3F;background-color:#FBDE28;">方法调用过程</font><font style="color:rgba(13, 13, 13, 0.9);background-color:#FBDE28;">中基本数据类型的变量</font><font style="color:rgba(13, 13, 13, 0.9);">（int、short、long、byte、float、double、boolean、har等）以及</font><font style="color:rgba(13, 13, 13, 0.9);background-color:#FBDE28;">对象的引用变量</font><font style="color:rgba(13, 13, 13, 0.9);">，其内存分配在栈上，变量出了作用域就会自动释放</font>

2\. <font style="color:rgba(13, 13, 13, 0.9);">堆内存用来存储Java中的对象。无论是成员变量，局部变量，还是类变量，它们指向的对象都存储在堆内存中</font>



\## <font style="color:rgba(13, 13, 13, 0.9);">线程独享还是共享</font>

1\. <font style="color:rgba(13, 13, 13, 0.9);">栈内存归属于单个线程，每个线程都会有一个栈内存，其存储的变量只能在其所属线程中可见，即栈内存可以理解成线程的私有内存。</font>

2\. <font style="color:rgba(13, 13, 13, 0.9);">堆内存中的对象对所有线程可见。堆内存中的对象可以被所有线程访问。</font>



\## <font style="color:rgba(13, 13, 13, 0.9);">空间大小</font>

<font style="color:rgba(13, 13, 13, 0.9);">栈的内存要远远小于堆内存</font>



\# <font style="color:rgb(60, 60, 67);">字符串常量池</font>

\## 作用

\*\*<font style="color:rgb(60, 60, 67);">字符串常量池</font>\*\*<font style="color:rgb(60, 60, 67);"> 是 JVM </font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">为了提升性能和减少内存消耗</font><font style="color:rgb(60, 60, 67);">针对字符串（String 类）专门开辟的一块区域，主要目的是为了</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">避免字符串的重复创建</font><font style="color:rgb(60, 60, 67);">。</font>



<font style="color:rgb(60, 60, 67);"> HotSpot 虚拟机中字符串常量池的实现是 </font>`<font style="color:rgb(60, 60, 67);">src/hotspot/share/classfile/stringTable.cpp</font>`<font style="color:rgb(60, 60, 67);"> ,</font>`<font style="color:rgb(60, 60, 67);">StringTable</font>`<font style="color:rgb(60, 60, 67);"> 可以简单理解为一个固定大小的</font>`<font style="color:rgb(60, 60, 67);">HashTable</font>`<font style="color:rgb(60, 60, 67);"> ，容量为 </font>`<font style="color:rgb(60, 60, 67);">StringTableSize</font>`<font style="color:rgb(60, 60, 67);">（可以通过 </font>`<font style="color:rgb(60, 60, 67);">-XX:StringTableSize</font>`<font style="color:rgb(60, 60, 67);"> 参数来设置），保存的是字符串（key）和 字符串对象的引用（value）的映射关系，字符串对象的引用指向堆中的字符串对象。</font>



<font style="color:rgb(60, 60, 67);">JDK1.7 之前，字符串常量池存放在永久代。JDK1.7 字符串常量池和静态变量从永久代移动到了 Java 堆中。</font>



!\[画板](https://cdn.nlark.com/yuque/0/2026/jpeg/63012352/1776748449860-e2179e70-203c-4f04-82ed-a6c5b3dae182.jpeg)



\## JDK1.7为什么要将字符串常量池移动到堆中？

主要是因为永久代（方法区实现）的 <font style="background-color:#FBDE28;">GC 回收效率太低</font>，<font style="background-color:#FBDE28;">只有在整堆收集</font> (Full GC)的时候<font style="background-color:#FBDE28;">才会被执行 GC</font>。Java 程序中通常会有<font style="background-color:#FBDE28;">大量的被创建的字符串等待回收</font>，将字符串常量池放到堆中，能够更高效及时地回收字符串内存。



\# 程序运行中栈可能会出现什么错误？

\+ `\*\*StackOverFlowError\*\*`\*\*：\*\* 如果栈的内存大小不允许动态扩展，那么当<font style="background-color:#FBDE28;">线程请求栈的深度</font>超过当前 Java 虚拟机<font style="background-color:#FBDE28;">栈的最大深度</font>的时候，就抛出 `StackOverFlowError` 错误。

\+ `\*\*OutOfMemoryError\*\*`\*\*：\*\* 如果栈的内存大小可以动态扩展， 那么当虚拟机在动态扩展栈时无法申请到足够的内存空间，则抛出`OutOfMemoryError`异常。



\# 如何排查OOM问题

What?	OOM是out of memory的简称，表示程序需要的内存空间大于JVM分配的内存空间。



导致OOM的情况：



\*\*内存泄露\*\*：<font style="background-color:#FBDE28;">申请使用完的内存没有释放，导致虚拟机不能再次使用该内存</font>，此时这段内存就泄露了，因为申请者不用了，而又不能被虚拟机分配给别人用。



\*\*内存溢出\*\*：<font style="background-color:#FBDE28;">申请的内存超出</font>了JVM能提供的<font style="background-color:#FBDE28;">内存大小</font>，此时称之为溢出。



常见OOM异常的情况：



Java堆内存溢出：一般由于内存泄露或者堆的大小设置不当引起。`java.lang.OutOfMemoryError: Java heap space`。对于内存泄露，需要通过<font style="background-color:#FBDE28;">内存监控软件查找程序中的泄露代码</font>，而堆大小可以<font style="background-color:#FBDE28;">通过虚拟机参数</font>-Xms,-Xmx来修改。



java方法区溢出：一般出现在大量Class、或者采用cglib等反射机制的情况和过多的常量尤其是字符串。`java.lang.OutOfMemoryError: PermGen space 或 java.lang.OutOfMemoryError：MetaSpace`可以通过更改方法区的大小来解决，使用类似-XX:PermSize=64m或-XX:MaxPermSize=256m的形式修改。对于过多常量导致的方法区溢出解决的方法是：先获取Dump文件，再使用MAT工具分析Dump文件（



\+ 如果是内存泄漏，可进一步通过工具查看泄漏对象到GC Roots的引用链。掌握了泄漏对象的类信息和GC Roots引用链的信息，就可以比较准确地定位泄漏代码的位置。

\+ 如果是普通的内存溢出，确实有很多占用内存的对象，那就只需要提升堆内存空间即可。



）



\## 如果发生内存泄漏怎么排查





\# 直接内存的作用

What?	不属于 JVM 堆，\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">直接向操作系统申请的本地内存</font>\*\*。 



How?	  



1.提高 IO 读写效率，普通堆内存读写流程：Java 堆内存 → 拷贝到\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">直接内存</font>\*\* → 系统调用读写磁盘 / 网络 ， 如果用\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">直接内存就会省去一次内存拷贝，直接和操作系统交互 。</font>\*\*



2\. 减轻 JVM 堆压力： 大缓存、大缓冲区放在堆外，不占用 Heap 堆空间，减少堆内存占用、减少 GC 次数、降低 STW  



3\. 方便操作系统底层交互： 直接内存和系统内核缓冲区\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">无缝对接</font>\*\*JVM 不用层层转发，适合底层框架、网络框架  



<font style="color:rgba(13, 13, 13, 0.9);background-color:rgba(102, 128, 153, 0.05);"></font>



<font style="color:rgb(60, 60, 67);">直接内存是一种特殊的</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">内存缓冲区</font><font style="color:rgb(60, 60, 67);">，并不在 Java 堆或方法区中分配的，而是</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">通过 JNI</font><font style="color:rgb(60, 60, 67);"> （Java本地接口）的方式</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">在本地内存上分配</font><font style="color:rgb(60, 60, 67);">的。</font>



<font style="color:rgb(60, 60, 67);">直接内存并不是虚拟机运行时数据区的一部分，也不是虚拟机规范中定义的内存区域，但是这部分内存也被频繁地使用。而且也可能导致 </font>`<font style="color:rgb(60, 60, 67);">OutOfMemoryError</font>`<font style="color:rgb(60, 60, 67);"> 错误出现。</font>



<font style="color:rgb(60, 60, 67);"> JDK1.4 中新加入的 </font>\*\*NIO（Non-Blocking I/O，也被称为 New I/O）\*\*<font style="color:rgb(60, 60, 67);">，引入了一种基于</font>\*\*通道（Channel）\*\*<font style="color:rgb(60, 60, 67);">与</font>\*\*缓存区（Buffer）\*\*<font style="color:rgb(60, 60, 67);">的 I/O 方式，它可以</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">直接使用 Native 函数库直接分配堆外内存</font><font style="color:rgb(60, 60, 67);">，然后</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">通过</font><font style="color:rgb(60, 60, 67);">一个存储在 </font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">Java 堆中的 DirectByteBuffer 对象作为这块内存的引用</font><font style="color:rgb(60, 60, 67);">进行操作。这样就能在一些场景中显著提高性能，因为</font>\*\*避免了在 Java 堆和 Native 堆之间来回复制数据\*\*<font style="color:rgb(60, 60, 67);">。</font>



<font style="color:rgb(60, 60, 67);">直接内存的分配不会受到 Java 堆的限制，但是会受到本机总内存大小以及处理器寻址空间的限制。</font>



<font style="color:rgb(60, 60, 67);">直接内存不同于堆外内存，堆外内存是把对象分配在堆外的内存，这些内存直接受操作系统管理而不是虚拟机，这样做的结果是能够在一定程度上减少垃圾回收对应用程序造成的影响。</font>



\# <font style="color:rgb(60, 60, 67);">Java对象的创建过程</font>

1.类加载检查：<font style="color:rgb(60, 60, 67);">虚拟机执行 new 指令时，</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">先检查常量池中对应类的符号引用</font><font style="color:rgb(60, 60, 67);">是否已加载、解析和初始化，未完成则先执行类加载过程。</font>



<font style="color:rgb(60, 60, 67);">2.分配内存： 类加载通过后，根据类加载确定的对象大小从 Java 堆划分内存，分配方式有 “指针碰撞”（适用于堆内存规整，如 Serial/ParNew 收集器）和 “空闲列表”（适用于堆内存不规整，如 CMS 收集器）；为保证线程安全，采用 CAS + 失败重试或 TLAB（线程本地分配缓冲）机制。</font>



3.初始化零值：<font style="color:rgb(60, 60, 67);">将分配的内存空间（除对象头外）初始化为零值，确保 Java 代码中未赋初始值的实例字段可直接使用对应类型的零值。</font>



4.设置对象值：<font style="background-color:#FBDE28;">在对象头</font>中记录<font style="color:rgb(60, 60, 67);">类</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">元数据信息、哈希码、GC 分代年龄、锁状态</font><font style="color:rgb(60, 60, 67);">等必要信息，具体设置依虚拟机运行状态（如是否启用偏向锁）而定。</font>



<font style="color:rgb(60, 60, 67);">5.执行init方法：虚拟机视角下对象已创建，但需执行</font>`<font style="color:rgb(60, 60, 67);"><init></font>`<font style="color:rgb(60, 60, 67);">方法按程序员定义完成初始化，最终生成可用对象。</font>



\# <font style="color:rgb(60, 60, 67);">对象访问定位的方式有哪些</font>

<font style="color:rgb(60, 60, 67);">通过</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">栈上的 reference 数据</font><font style="color:rgb(60, 60, 67);">来操作堆上的具体对象。对象的访问方式由虚拟机实现而定，目前主流的访问方式有：</font>\*\*<font style="color:rgb(60, 60, 67);">使用句柄</font>\*\*<font style="color:rgb(60, 60, 67);">、</font>\*\*<font style="color:rgb(60, 60, 67);">直接指针</font>\*\*<font style="color:rgb(60, 60, 67);">。</font>



\## <font style="color:rgb(60, 60, 67);">句柄</font>

<font style="color:rgb(60, 60, 67);"> Java 堆中将会划分出一块内存来作为句柄池，reference 不直接指向对象，而是指向一个句柄池中的句柄，句柄中包含了</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">对象实例数据</font><font style="color:rgb(60, 60, 67);">与对象类型数据各自的</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">具体地址信息</font><font style="color:rgb(60, 60, 67);">。</font>



<img src="https://cdn.nlark.com/yuque/0/2026/png/63012352/1776647652715-be3e15c0-d156-47fa-872f-caa2cc48d632.png" width="672" title="" crop="0,0,1,1" id="u42e3a3b4" class="ne-image">



\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">优点</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">：</font>



\+ \*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">GC 移动成本低</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">：对象被 GC 移动时，</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">只需更新句柄里的地址</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">，所有引用（变量）都不用改。</font>

\+ \*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">安全性高</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">：句柄作为中间层，可控制访问权限。</font>



\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">缺点</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">：</font>



\+ \*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">访问慢</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">：多一次指针跳转（引用→句柄→对象）。</font>

\+ \*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">内存占用高</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">：需要额外维护句柄池。</font>



\## 直接指针

<font style="color:rgb(60, 60, 67);">reference 直接存储对象在堆中的内存地址。</font>



<img src="https://cdn.nlark.com/yuque/0/2026/png/63012352/1776647706764-0027656f-4888-473d-a1f9-801bedb12451.png" width="668.8" title="" crop="0,0,1,1" id="ub5d29de5" class="ne-image">



\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">优点</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">：</font>



\+ \*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">访问极快</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">：</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">一次寻址</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">直接访问对象，性能比句柄快 </font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">12%\~18%</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">。</font>

\+ \*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">内存紧凑</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">：无句柄池开销，内存利用率高。</font>



\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">缺点</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">：</font>



\+ \*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">GC 成本高</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">：对象移动时，</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">必须更新所有引用该对象的指针</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">，增加 STW 时间。</font>

\+ \*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">内存安全弱</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">：直接暴露地址，依赖 JVM 严格管理。</font>



\*\*<font style="color:rgb(60, 60, 67);">HotSpot 虚拟机主要使用的就是这种方式来进行对象访问。</font>\*\*



\# <font style="color:rgb(60, 60, 67);">JVM垃圾回收</font>

\## 🤖判断对象死亡

Why?	<font style="color:rgb(60, 60, 67);">堆垃圾回收前的第一步就是要判断哪些对象已经死亡</font>



<font style="color:rgb(60, 60, 67);">How？	</font>



\*\*<font style="color:rgb(60, 60, 67);">方法一：引用计数法</font>\*\*



<font style="color:rgb(60, 60, 67);">给对象中添加一个</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">引用计数器</font><font style="color:rgb(60, 60, 67);">：</font>



\+ <font style="color:rgb(60, 60, 67);">每当有一个地方引用它，计数器就加 1；</font>

\+ <font style="color:rgb(60, 60, 67);">当引用失效，计数器就减 1；</font>

\+ <font style="color:rgb(60, 60, 67);">任何时候</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">计数器为 0 的对象</font><font style="color:rgb(60, 60, 67);">就是不可能再被使用的。</font>



\*\*<font style="color:rgb(60, 60, 67);">这个方法实现简单，效率高，但是目前主流的虚拟机中并没有选择这个算法来管理内存，</font>\*\*



缺点是：\*\*<font style="color:rgb(60, 60, 67);background-color:#FBDE28;">很难解决对象之间循环引用的问题，导致出现内存泄漏</font>\*\*\*\*<font style="color:rgb(60, 60, 67);">。</font>\*\*



\*\*<font style="color:rgb(60, 60, 67);">比如：</font>\*\*<font style="color:rgb(60, 60, 67);">对象 </font>`<font style="color:rgb(60, 60, 67);">objA</font>`<font style="color:rgb(60, 60, 67);"> 和 </font>`<font style="color:rgb(60, 60, 67);">objB</font>`<font style="color:rgb(60, 60, 67);"> 相互引用着对方之外，这两个对象之间再无任何引用。但是他们因为互相引用对方，导致它们的引用计数器都不为 0，于是引用计数算法无法通知 GC 回收器回收他们。</font>



\*\*方法二：可达性分析算法\*\*



以\*\*“GC Roots”\*\* 的对象作为起点，从这些节点开始向下搜索，节点所走过的路径称为引用链，<font style="background-color:#FBDE28;">当一个对象到 GC Roots 没有任何引用链相连的话</font>，则证明此对象是不可用的，需要被回收。



比如:下图中的`object6\~object10`之间虽有引用关系，但它们到GCRoots不可达，因此为需要被回收的对象。<img src="https://cdn.nlark.com/yuque/0/2026/png/63012352/1776648291416-f502fd3a-7eb0-4524-8226-b7991dfe48b8.png" width="656" title="" crop="0,0,1,1" id="ufecdc706" class="ne-image">



\*\*<font style="color:rgb(60, 60, 67);">哪些对象可以作为 GC Roots 呢？</font>\*\*



\+ <font style="background-color:#FBDE28;">虚拟机栈</font>(栈帧中的局部变量表)中引用的对象

\+ <font style="background-color:#FBDE28;">本地方法栈</font>(Native 方法)中引用的对象

\+ 方法区中<font style="background-color:#FBDE28;">类静态属性</font>引用的对象

\+ 方法区中<font style="background-color:#FBDE28;">常量</font>引用的对象

\+ 所有<font style="background-color:#FBDE28;">被同步锁持有</font>的对象

\+ <font style="background-color:#FBDE28;">JNI</font>（Java Native Interface）引用的对象



\*\*<font style="color:rgb(60, 60, 67);">对象可以被回收，就代表一定会被回收吗？</font>\*\*



<font style="color:rgb(60, 60, 67);">真正宣告对象死亡的时候至少经历两次标记过程：</font>



\*\*<font style="color:rgb(60, 60, 67);">第一次标记的标准是</font>\*\*<font style="color:rgb(60, 60, 67);">： 可达性分析判定</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">不可达</font>\*\*<font style="color:rgb(60, 60, 67);"> → 暂时标记为可回收  </font>



\*\*<font style="color:rgb(60, 60, 67);">第二次标记的标准是：</font>\*\*<font style="color:rgb(60, 60, 67);">此对象是否有必要执行 </font>`<font style="color:rgb(60, 60, 67);">finalize</font>`<font style="color:rgb(60, 60, 67);"> 方法， 只有</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">重写了 finalize 且从未执行过</font>\*\*<font style="color:rgb(60, 60, 67);">的对象不会被回收。</font>



\## <font style="color:rgb(60, 60, 67);">垃圾回收的算法有哪些</font>

\### 标记-清除算法

<font style="color:rgb(60, 60, 67);">标记-清除（Mark-and-Sweep）算法分为“标记（Mark）”和“清除（Sweep）”阶段：首先</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">标记出所有不需要回收的对象</font><font style="color:rgb(60, 60, 67);">，在标记完成后统一回收掉所有没有被标记的对象。（也有的说是标记所有需要回收的对象）</font>



<font style="color:rgb(60, 60, 67);"> 存在的问题：</font>



\+ \*\*<font style="color:rgb(60, 60, 67);">效率问题</font>\*\*<font style="color:rgb(60, 60, 67);">：标记和清除两个过程效率都不高。</font>

\+ \*\*<font style="color:rgb(60, 60, 67);">空间问题</font>\*\*<font style="color:rgb(60, 60, 67);">：标记清除后会产生</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">大量不连续</font><font style="color:rgb(60, 60, 67);">的内存碎片。  

</font><img src="https://cdn.nlark.com/yuque/0/2026/png/63012352/1776666238756-35bb7cfb-8c59-47ad-bee5-091afca97daf.png" width="396" title="" crop="0,0,1,1" id="uacb34314" class="ne-image">



\### 复制算法

<font style="color:rgb(60, 60, 67);">将</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">内存分为大小相同的两块</font><font style="color:rgb(60, 60, 67);">，</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">每次使用</font><font style="color:rgb(60, 60, 67);">其中的</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">一块</font><font style="color:rgb(60, 60, 67);">。当这一块的</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">内存使用完后</font><font style="color:rgb(60, 60, 67);">，就将还</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">存活的对象复制到另一块去</font><font style="color:rgb(60, 60, 67);">，然后再</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">把使用的空间一次清理掉</font><font style="color:rgb(60, 60, 67);">。这样就使每次的内存回收都是对内存区间的一半进行回收。  

</font><img src="https://cdn.nlark.com/yuque/0/2026/png/63012352/1776666252467-33c27cb7-249e-4eb5-89e8-71c90ff8b1c6.png" width="419.2" title="" crop="0,0,1,1" id="uaf900bcb" class="ne-image">



存在的问题：



\+ \*\*<font style="color:rgb(60, 60, 67);">可用内存变小</font>\*\*<font style="color:rgb(60, 60, 67);">：可用内存缩小为原来的一半。</font>

\+ \*\*<font style="color:rgb(60, 60, 67);">不适合老年代</font>\*\*<font style="color:rgb(60, 60, 67);">：如果存活对象数量比较大，复制性能会变得很差。</font>



\### 标记-整理算法

首先<font style="background-color:#FBDE28;">标记出不需要回收的对象</font>，然后将所有存活的对象往内存的一端移动、挤在一起，移动完之后，直接把边界以外的内存全部清空。



<img src="https://cdn.nlark.com/yuque/0/2026/png/63012352/1776666577726-d6dcaa89-e061-4835-a201-db529c5dc5f8.png" width="452.8" title="" crop="0,0,1,1" id="ucb0857a9" class="ne-image">



存在的问题：<font style="color:rgb(60, 60, 67);">多了整理这一步，因此效率也不高，适合老年代这种垃圾回收频率不是很高的场景。</font>



\### <font style="color:rgb(60, 60, 67);">分代收集算法</font>

<font style="background-color:#FBDE28;">当前虚拟机的垃圾收集都采用分代收集算法</font>，这种算法没有什么新的思想，只是<font style="background-color:#FBDE28;">根据对象存活周期的不同将内存分为几块</font>。一般将 Java <font style="background-color:#FBDE28;">堆分为新生代和老年代</font>，这样我们就可以根据各个年代的特点选择合适的垃圾收集算法。



比如在<font style="background-color:#FBDE28;">新生代</font>中，每次收集都会有<font style="background-color:#FBDE28;">大量对象死去</font>，所以可以<font style="background-color:#FBDE28;">选择”标记-复制“算法</font>，只需要复制少量对象就可以完成每次垃圾收集。而<font style="background-color:#FBDE28;">老年代的对象存活几率较高</font>，而且<font style="background-color:#FBDE28;">没有额外的空间对它进行分配</font>担保，所以我们<font style="background-color:#FBDE28;">必须选择“标记-清除”或“标记-整理”</font>算法进行垃圾收集。



\## JDK1.8的默认垃圾回收器是？JDK1.9之后呢?

\*\*JDK 1.8 默认垃圾回收器\*\*：Parallel Scanvenge（新生代）+ Parallel Old（老年代）。 这个组合也被称为 <font style="background-color:#FBDE28;">Parallel GC 或 Throughput GC</font>，侧重于吞吐量。



\*\*JDK 1.9 及以后默认垃圾回收器\*\*：<font style="background-color:#FBDE28;">G1 GC</font> (Garbage-First Garbage Collector)。 G1 GC 是一个更现代化的垃圾回收器，旨在<font style="background-color:#FBDE28;">平衡吞吐量和停顿时间</font>，尤其适用于<font style="background-color:#FBDE28;">堆内存较大的应用</font>。



\## G1垃圾回收的过程

&#x20;G1（Garbage-First）垃圾收集器在<font style="background-color:#FBDE28;"> JDK 7 中首次引入</font>，到了 <font style="background-color:#FBDE28;">JDK 8，功能基本已经完全实</font>现，成为一个稳定、可用于生产环境的垃圾收集器。



\+ 初始标记：短暂停顿（STW：Stop The World）标记GC Roots 直接关联的对象

\+ 并发标记：与应用并发运行，标记所有存活的对象，时间较长，但不影响程序运行

\+ 最终标记：再次短暂停顿，处理并发标记阶段结束后残留的少量未处理的引用变更

\+ 筛选回收：再次短暂停顿，<font style="background-color:#FBDE28;">只回收垃圾最多、回收价值最高</font>的区域，把<font style="background-color:#FBDE28;">存活对象复制到空区域</font>，然后直接清掉整块内存。



<img src="https://cdn.nlark.com/yuque/0/2026/png/63012352/1776668042440-136dd5fe-d4d9-4f53-b16b-36394eba80bc.png" width="680.8" title="" crop="0,0,1,1" id="u2ee81c3a" class="ne-image">



Why?



\*\*G1 收集器在后台维护了一个\*\*\*\*<font style="background-color:#FBDE28;">优先列表</font>\*\*\*\*，每次\*\*\*\*<font style="background-color:#FBDE28;">根据允许的收集时间</font>\*\*\*\*，\*\*\*\*<font style="background-color:#FBDE28;">优先选择回收价值最大</font>\*\*\*\*的 Region\*\*。这种<font style="background-color:#FBDE28;">使用 Region 划分内存空间</font>以及<font style="background-color:#FBDE28;">有优先级的区域回收</font>方式，保证了 G1 收集器在有限时间内可以尽可能高的收集效率（把内存化整为零）。



\## ZGC有哪些改进

\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">ZGC (Z Garbage Collector)</font>\*\* 是 Java 虚拟机 (JVM) 中一款\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">革命性的</font>\*\*\*\*<font style="color:rgb(0, 0, 0);background-color:#FBDE28;">低延迟垃圾收集器</font>\*\*，核心目标是实现亚毫秒级（<1ms）的暂停时间，且几乎不受堆内存大小影响。  



体现在：



\+ 暂停时间控制在几毫秒内，且暂停时间不受堆内存大小的影响。出现STW的情况更少，代价是牺牲一些吞吐量

\+ 超大堆支持，支持TB级内存 



<font style="color:rgb(60, 60, 67);">ZGC 在 Java11 中引入，处于试验阶段；Java15 正式使用了，Java21中，引入了分代ZGC，暂停时间可以缩短到1毫秒以内。</font>



<font style="color:rgb(60, 60, 67);">默认的垃圾回收器依然是 G1。你可以通过下面的参数启用 ZGC：</font>`<font style="color:rgb(60, 60, 67);">java -XX:+UseZGC className</font>`



<font style="color:rgb(60, 60, 67);">启用分代 ZGC：</font>`<font style="color:rgb(60, 60, 67);">java -XX:+UseZGC -XX:+ZGenerational className</font>`



\# <font style="color:rgb(60, 60, 67);">常见的引用类型</font>

<font style="color:rgb(60, 60, 67);">JDK1.2 之前，定义的引用类型是：如果 reference 类型的</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">数据存储的数值</font><font style="color:rgb(60, 60, 67);">代表的</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">是另一块内存的起始地址</font><font style="color:rgb(60, 60, 67);">，就称这块内存代表一个引用。</font>



<font style="color:rgb(60, 60, 67);"> JDK1.2 以后，Java 对引用的概念进行了扩充，将引用分为强引用、软引用、弱引用、虚引用四种（引用强度逐渐减弱），强引用就是 Java 中普通的对象，而软引用、弱引用、虚引用在 JDK 中定义的类分别是 </font>`<font style="color:rgb(60, 60, 67);">SoftReference</font>`<font style="color:rgb(60, 60, 67);">、</font>`<font style="color:rgb(60, 60, 67);">WeakReference</font>`<font style="color:rgb(60, 60, 67);">、</font>`<font style="color:rgb(60, 60, 67);">PhantomReference</font>`



\## <font style="color:rgb(60, 60, 67);">强引用（</font>\*\*<font style="color:rgb(60, 60, 67);">StrongReference</font>\*\*<font style="color:rgb(60, 60, 67);">）</font>

强引用实际上就是程序代码中<font style="background-color:#FBDE28;">普遍存在</font>的引用赋值，这是使用最普遍的引用，其代码如下



```java

String strongReference = new String("abc");

```



如果一个对象具有强引用，那就属于\*\*必不可少\*\*，<font style="background-color:#FBDE28;">垃圾回收器绝不会回收它</font>。当内存空间不足，Java 虚拟机宁愿抛出 OutOfMemoryError 错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足问题。



\## 软引用（SoftReference）

如果一个对象只具有软引用，那就\*\*可有可无\*\*。软引用代码如下



```java

// 软引用

String str = new String("abc");

SoftReference<String> softReference = new SoftReference<String>(str);

```



如果内存空间足够，垃圾回收器就不会回收它，如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。<font style="background-color:#FBDE28;">软引用可用来实现内存敏感的高速缓存。</font>软引用可以和一个引用队列（ReferenceQueue）联合使用，如果软引用所引用的对象被垃圾回收，JAVA 虚拟机就会把这个软引用加入到与之关联的引用队列中。



\## 弱引用（WeakReference）

如果一个对象只具有弱引用，那就类似于\*\*可有可无\*\*。弱引用代码如下：



```java

String str = new String("abc");

WeakReference<String> weakReference = new WeakReference<>(str);

str = null; //str变成软引用，可以被收集

```



弱引用与软引用的区别在于：<font style="background-color:#FBDE28;">只具有弱引用</font>的对象拥有<font style="background-color:#FBDE28;">更短暂的生命周期</font>。在垃圾回收器线程扫描它所管辖的内存区域的过程中，<font style="background-color:#FBDE28;">一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存</font>。不过，由于垃圾回收器是一个优先级很低的线程， 因此不一定会很快发现那些只具有弱引用的对象。也可以与引用队列联合使用，如果弱引用所引用的对象被垃圾回收，<font style="color:rgb(60, 60, 67);">Java 虚拟机就会把这个弱引用加入到与之关联的引用队列中。</font>



\## 虚引用（PhantomReference）

<font style="color:rgb(60, 60, 67);">虚引用不会决定对象的生命周期。如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收。虚引用代码如下：</font>



```java

String str = new String("abc");

ReferenceQueue queue = new ReferenceQueue();

// 创建虚引用，要求必须与一个引用队列关联

PhantomReference pr = new PhantomReference(str, queue);

```



\*\*虚引用主要用来跟踪对象被垃圾回收的活动\*\*<font style="color:rgb(60, 60, 67);">。</font>



\*\*虚引用与软引用和弱引用的一个区别在于：\*\*<font style="color:rgb(60, 60, 67);"> 虚引用</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">必须和引用队列</font><font style="color:rgb(60, 60, 67);">（ReferenceQueue）联合使用。当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之关联的引用队列中。程序通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。程序如果发现</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">某个虚引用已经被加入到引用队列</font><font style="color:rgb(60, 60, 67);">，那么就可以在所引用的对象的内存</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">被回收之前做一些收尾操作</font><font style="color:rgb(60, 60, 67);">。</font>



<font style="color:rgb(60, 60, 67);">特别注意，在程序设计中一般很少使用弱引用与虚引用，使用软引用的情况较多，这是因为</font>\*\*软引用可以维护系统的运行安全，防止内存溢出（OutOfMemory）等问题的产生\*\*<font style="color:rgb(60, 60, 67);">。</font>



\# <font style="color:rgb(60, 60, 67);">如何判断一个类是无用的类</font>

Why?	方法区主要回收的是无用的类



How?



满足三个条件“仅仅”可以满足回收：



\+ 该类<font style="background-color:#FBDE28;">所有的实例</font>都已经被回收，也就是 Java 堆中不存在该类的任何实例。

\+ 加载该类的 `<font style="background-color:#FBDE28;">ClassLoader</font>`<font style="background-color:#FBDE28;"> 已经被回收</font>。

\+ 该类<font style="background-color:#FBDE28;">对应的 </font>`<font style="background-color:#FBDE28;">java.lang.Class</font>`<font style="background-color:#FBDE28;"> 对象</font>没有在任何地方被引用，<font style="background-color:#FBDE28;">无法</font>在任何地方<font style="background-color:#FBDE28;">通过反射访问该类的方法</font>。



\# 双亲委派模型

\## What?

JDK默认的类加载机制规则， 当一个类加载器要加载类时，不会自己先去加载，而是先委托给父类加载器； 父类再委托给上层父类，一直向上到\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">启动类加载器</font>\*\*；上层加载不了，下层才自己加载。  



\## How?

\+ <font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">应用加载器要加载一个类 → </font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">先找父类：扩展加载器</font>\*\*

\+ <font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">扩展加载器不自己加载 → </font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">再找父类：启动类加载器</font>\*\*

\+ <font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">启动类加载器是最顶层，没有父类，</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">尝试自己加载</font>\*\*

\+ <font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">能加载 → 直接加载结束</font>

\+ <font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">不能加载 → 退回下一层</font>

\+ <font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">退回扩展加载器尝试加载，不行再退回应用加载器</font>

\+ <font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">最后</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">最下层自己负责加载</font>\*\*



<img src="https://cdn.nlark.com/yuque/0/2026/png/63012352/1776673437339-27711353-ed69-4375-a953-05f4eba627e9.png" width="448.8" title="" crop="0,0,1,1" id="ueb588d74" class="ne-image">



注意：<font style="color:rgb(60, 60, 67);">类加载器之间的父子关系一般不是以继承的关系来实现的，而是通常使用组合关系来复用父加载器的代码。</font>



```java

public abstract class ClassLoader {

&#x20; ...

&#x20; // 组合

&#x20; private final ClassLoader parent;

&#x20; protected ClassLoader(ClassLoader parent) {

&#x20;      this(checkCreateClassLoader(), parent);

&#x20; }

&#x20; ...

}

```



<font style="color:rgb(60, 60, 67);">在面向对象编程中，有一条非常经典的设计原则：</font>\*\*<font style="color:rgb(60, 60, 67);">组合优于继承，多用组合少用继承。</font>\*\*



\## <font style="color:rgb(60, 60, 67);">好处</font>

\+ 安全：这种层级关系代表的是一种优先级，也就是所有类的加载优先给启动类加载器。

\+ 避免重复加载导致程序混乱的问题，如果父加载器已经加载过了那么子类就没必要加载了。



\## <font style="color:rgb(60, 60, 67);">如何打破打破双亲委派模型？</font>

定义加载器的话，需要继承 `ClassLoader` 。



不想打破双亲委派模型，就重写 `ClassLoader` 类中的 `findClass()` 方法，无法被父类加载器加载的类最终会通过这个方法被加载。



想打破双亲委派模型则需要重写 `loadClass()` 方法。<font style="color:rgb(60, 60, 67);">例如，子类加载器可以在</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">委派给父类加载器之前，先自己尝试加载这个类</font><font style="color:rgb(60, 60, 67);">，或者在</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">父类加载器返回之后，再尝试从其他地方加载这个类</font><font style="color:rgb(60, 60, 67);">。具体的规则由我们自己实现，根据项目需求定制化。</font>



\*\*<font style="color:#DF2A3F;">总结：</font>\*\*好的，面试官。



我知道有两种方式来破坏双亲委派模型



1\. 第一种，集成ClassLoader抽象类，重写<font style="background-color:#FBDE28;">loadClass</font>方法，在这个方法可以自定义要加载的类使用的类加载器。

2\. 第二种，使用线程上下文加载器，可以通过java.lang.Thread类的<font style="background-color:#FBDE28;">setContextClassLoader</font>()方法来设置当前类使用的类加载器类型。



<font style="color:rgb(60, 60, 67);">举例一：Tomcat 服务器为了能够优先加载 Web 应用目录下的类，然后再加载其他目录下的类，就自定义了类加载器 </font>`<font style="color:rgb(60, 60, 67);">WebAppClassLoader</font>`<font style="color:rgb(60, 60, 67);"> 来打破双亲委托机制</font>



<img src="https://cdn.nlark.com/yuque/0/2026/png/63012352/1776673885477-d71c6a60-bc77-46d6-8730-fa5e938b49c1.png" width="276.8" title="" crop="0,0,1,1" id="uac40e695" class="ne-image">



Tomcat 这四个自定义的类加载器对应的目录如下：



\+ `CommonClassLoader`对应`<Tomcat>/common/\*`

\+ `CatalinaClassLoader`对应`<Tomcat >/server/\*`

\+ `SharedClassLoader`对应 `<Tomcat >/shared/\*`

\+ `WebAppClassloader`对应 `<Tomcat >/webapps/<app>/WEB-INF/\*`



解释： 



\+ CommonClassLoader 作为 CatalinaClassLoader 和 SharedClassLoader 的父加载器  ， CommonClassLoader 能加载的类都可以被 CatalinaClassLoader 和 SharedClassLoader 使用。CommonClassLoader 是为了实现公共类库，放一些公共jar包，被所有 Web 应用和Tomcat 内部 一起共用。  

\+ CatalinaClassLoader 和 SharedClassLoader 能加载的类相互隔离。<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">Catalina 管</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">Tomcat 自己的代码；</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">Shared 管</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">各个 web 项目共用的代码，</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">他俩</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">互相看不见对方的类</font>\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">，互不打扰、隔离。</font>

\+ <font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);"> 每个 Web 应用都会创建一个单独的 WebAppClassLoader各个WebAppClassLoader 相互隔离，实现 web 应用之间类隔离  </font>



举例二： <font style="background-color:#FBDE28;">SPI 的接口</font>（如 `java.sql.Driver`）是<font style="background-color:#FBDE28;">由 Java 核心库</font>提供的，由`<font style="background-color:#FBDE28;">BootstrapClassLoader</font>`<font style="background-color:#FBDE28;"> 加载</font>。而<font style="background-color:#FBDE28;"> SPI 的实现</font>（如`com.mysql.cj.jdbc.Driver`）是<font style="background-color:#FBDE28;">由第三方供应商提供的</font>，它们是由应用程序类加载器或者自定义类加载器来加载的。 正常规则是：哪个加载器加载了接口，就还用这个加载器去找实现类。可启动类加载器只认识 JDK 内部的类，\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">不能向下找子类加载器</font>\*\*，就找不到项目中第三方实现类，只有打破双亲委派，利用\*\*线程上下文类加载器（TCCL）\*\* 强行让高层代码，\*\*<font style="color:rgb(0, 0, 0);background-color:rgba(0, 0, 0, 0);">反向使用下层的应用类加载器</font>\*\*去加载第三方实现类。



举例三：<font style="color:rgb(60, 60, 67);">项目中有 Spring 的 jar 包，由于其是 Web 应用之间共享的，因此会由 </font>`<font style="color:rgb(60, 60, 67);">SharedClassLoader</font>`<font style="color:rgb(60, 60, 67);"> 加载，但项目中有一些用到了 Spring 的业务类，比如实现了 Spring 提供的接口、用到了 Spring 提供的注解。所以，加载 Spring 的类加载器（也就是 </font>`<font style="color:rgb(60, 60, 67);">SharedClassLoader</font>`<font style="color:rgb(60, 60, 67);">）也会用来加载这些业务类。但是业务类在 Web 应用目录下，不在 </font>`<font style="color:rgb(60, 60, 67);">SharedClassLoader</font>`<font style="color:rgb(60, 60, 67);"> 的加载路径下，所以 </font>`<font style="color:rgb(60, 60, 67);">SharedClassLoader</font>`<font style="color:rgb(60, 60, 67);"> 无法找到业务类，也就无法加载它们。就需要用到 </font>\*\*<font style="color:rgb(60, 60, 67);">线程上下文类加载器（</font>\*\*`\*\*<font style="color:rgb(60, 60, 67);">ThreadContextClassLoader</font>\*\*`\*\*<font style="color:rgb(60, 60, 67);">）来解决问题，</font>\*\*<font style="color:rgb(60, 60, 67);">当 Spring 需要加载业务类的时候，它不是用自己的类加载器，而是用当前线程的上下文类加载器。 每个 Web 应用都会创建一个单独的 </font>`<font style="color:rgb(60, 60, 67);">WebAppClassLoader</font>`<font style="color:rgb(60, 60, 67);">，并在启动 Web 应用的线程里设置线程线程上下文类加载器为 </font>`<font style="color:rgb(60, 60, 67);">WebAppClassLoader</font>`<font style="color:rgb(60, 60, 67);">。这样就可以让高层的类加载器（</font>`<font style="color:rgb(60, 60, 67);">SharedClassLoader</font>`<font style="color:rgb(60, 60, 67);">）借助子类加载器（ </font>`<font style="color:rgb(60, 60, 67);">WebAppClassLoader</font>`<font style="color:rgb(60, 60, 67);">）来加载业务类。</font>



<font style="color:rgb(60, 60, 67);">线程上下文类加载器的原理是将一个</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">类加载器保存在线程私有数据里，跟线程绑定</font><font style="color:rgb(60, 60, 67);">，然后在需要的时候取出来使用。这个类加载器通常是由应用程序或者容器（如 Tomcat）设置的。</font>



`<font style="color:rgb(60, 60, 67);">Java.lang.Thread</font>`<font style="color:rgb(60, 60, 67);"> 中的</font>`<font style="color:rgb(60, 60, 67);">getContextClassLoader()</font>`<font style="color:rgb(60, 60, 67);">和 </font>`<font style="color:rgb(60, 60, 67);">setContextClassLoader(ClassLoader cl)</font>`<font style="color:rgb(60, 60, 67);">分别用来</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">获取和设置</font><font style="color:rgb(60, 60, 67);">线程的上下文类加载器。如果没有通过</font>`<font style="color:rgb(60, 60, 67);">setContextClassLoader(ClassLoader cl)</font>`<font style="color:rgb(60, 60, 67);">进行设置的话，线程将继承其父线程的上下文类加载器。</font>



<font style="color:rgb(60, 60, 67);">Spring 获取线程线程上下文类加载器的代码如下：</font>



```java

cl = Thread.currentThread().getContextClassLoader();

```



\# 问题排查

\## Java性能优化和问题排查工具

\### JDK自带的可视化分析工具

\+ \*\*JConsole\*\* ：基于 JMX 的可视化监视、管理工具，可以用于查看应用程序的运行概况、内存、线程、类、VM 概括、MBean 等信息。

\+ \*\*VisualVM\*\*：基于 NetBeans 平台开发，具备了插件扩展功能的特性。利用它不仅能够监控服务的 CPU、内存、线程、类等信息，还可以捕获有关 JVM 软件实例的数据，并将该数据保存到本地系统，以供后期查看或与其他用户共享。



> 根据《深入理解 Java 虚拟机》介绍：“VisualVM 的性能分析功能甚至比起 JProfiler、YourKit 等专业且收费的 Profiling 工具都不会逊色多少，而且 VisualVM 还有一个很大的优点：不需要被监视的程序基于特殊 Agent 运行，因此他对应用程序的实际性能的影响很小，使得他可以直接应用在生产环境中。这个优点是 JProfiler、YourKit 等工具无法与之媲美的”。

>



\### <font style="color:rgb(60, 60, 67);">JDK 自带的命令行工具</font>

\+ `\*\*jps\*\*` (JVM Process Status）: 类似 UNIX 的 `ps` 命令。用于查看所有 Java 进程的启动类、传入参数和 Java 虚拟机参数等信息；

\+ `\*\*jstat\*\*`（JVM Statistics Monitoring Tool）: 用于收集 HotSpot 虚拟机各方面的运行数据;

\+ `\*\*jinfo\*\*` (Configuration Info for Java) : Configuration Info for Java,显示虚拟机配置信息;

\+ `\*\*jmap\*\*` (Memory Map for Java) : 生成堆转储快照;

\+ `\*\*jhat\*\*` (JVM Heap Dump Browser) : 用于分析 heapdump 文件，它会建立一个 HTTP/HTML 服务器，让用户可以在浏览器上查看分析结果。JDK9 移除了 jhat；

\+ `\*\*jstack\*\*` (Stack Trace for Java) : 生成虚拟机当前时刻的线程快照，线程快照就是当前虚拟机内每一条线程正在执行的方法堆栈的集合。



\### 第三方工具

\+ \*\*MAT\*\*：一款功能强大的 Java 堆内存分析器，可以用于查找内存泄漏以及查看内存消耗情况，用户可以利用 VisualVM 或者是 `jmap` 命令生产堆文件，然后导入工具中进行分析。

\+ \*\*GCeasy\*\*：一款在线的 GC 日志分析器，使用起来非常方便，用户可以通过它的 Web 网站导入 GC 日志，实时进行内存泄漏检测、GC 暂停原因分析、JVM 配置建议优化等功能。网站地址：\[https://gceasy.io/](https://gceasy.io/) 。

\+ \*\*GCViewer\*\*：一款非常强大的 GC 日志可视化分析工具，功能强大而且完全免费。

\+ \*\*JProfiler\*\*：一款商用的性能分析利器，功能强大，但需要付费使用。 它提供更深入的性能分析功能，例如方法调用分析、内存分配分析等。

\+ \*\*Arthas\*\*：阿里开源的一款线上监控诊断工具，可以查看应用负载、内存、gc、线程等信息。



\## 如何查看服务器上运行的Java进程？

<font style="color:rgb(60, 60, 67);">通过JDK 自带的 </font>`<font style="color:rgb(60, 60, 67);">jps</font>`<font style="color:rgb(60, 60, 67);"> (JVM Process Status) 命令专门用于列出当前用户下所有正在运行的 JVM 实例。</font>



`<font style="color:rgb(60, 60, 67);">jps</font>`<font style="color:rgb(60, 60, 67);"> 的基础用法和几个核心参数如下：</font>



\+ `\*\*jps\*\*`<font style="color:rgb(60, 60, 67);">：这是最基础的用法，它会列出 Java 进程的 </font>\*\*LVMID\*\*<font style="color:rgb(60, 60, 67);">（本地虚拟机唯一 ID，通常就是操作系统的进程号 PID）和</font>\*\*主类名\*\*<font style="color:rgb(60, 60, 67);">（或 Jar 包名）。</font>

\+ `\*\*jps -l\*\*`<font style="color:rgb(60, 60, 67);">：输出主类的</font>\*\*完整包名\*\*<font style="color:rgb(60, 60, 67);">，或者如果应用是通过 Jar 包运行的，会输出 Jar 包的</font>\*\*完整路径\*\*<font style="color:rgb(60, 60, 67);">。这在同一台机器上部署了多个来自不同项目的 Java 应用时，能非常清晰地区分它们。</font>

\+ `\*\*jps -v\*\*`<font style="color:rgb(60, 60, 67);">：显示传递给 JVM 的参数，例如 </font>`<font style="color:rgb(60, 60, 67);">-Xmx</font>`<font style="color:rgb(60, 60, 67);">、</font>`<font style="color:rgb(60, 60, 67);">-Xms</font>`<font style="color:rgb(60, 60, 67);">、</font>`<font style="color:rgb(60, 60, 67);">-XX:+UseG1GC</font>`<font style="color:rgb(60, 60, 67);"> 等。通过它，可以快速确认应用的内存配置、GC 策略等是否符合预期。</font>

\+ `\*\*jps -m\*\*`<font style="color:rgb(60, 60, 67);">：用于查看传递给主函数 </font>`<font style="color:rgb(60, 60, 67);">main()</font>`<font style="color:rgb(60, 60, 67);"> 的参数。当需要确认程序启动时传入的业务参数是否正确时，它非常有用。</font>



<font style="color:rgb(60, 60, 67);">在某些情况下，</font>`<font style="color:rgb(60, 60, 67);">jps</font>`<font style="color:rgb(60, 60, 67);"> 命令可能无法满足需求，这时可以采用标准的操作系统命令：</font>



1\. \*\*权限问题\*\*<font style="color:rgb(60, 60, 67);">：jps 默认只能看到由</font>\*\*当前用户\*\*<font style="color:rgb(60, 60, 67);">启动的 Java 进程。如果需要查看服务器上所有用户（如 root 或其他业务用户）的 Java 进程，jps 就会受限。</font>

2\. \*\*环境问题\*\*<font style="color:rgb(60, 60, 67);">：在一些极简的生产环境或 Docker 容器中，可能只安装了 JRE 而没有完整的 JDK，此时 jps 命令可能不存在。</font>



<font style="color:rgb(60, 60, 67);">在这些情况下，可以使用 ps 命令来查找，例如：</font>



```bash

\# 列出所有进程，然后通过 grep 过滤出包含 "java" 关键字的进程

ps -ef | grep java

```



\## 如何检测死锁

\+ 使用`jmap`、`jstack`等命令查看 JVM 线程栈和堆内存的情况。如果有死锁，`jstack` 的输出中通常会有 `Found one Java-level deadlock:`的字样，后面会跟着死锁相关的线程信息。另外，实际项目中还可以搭配使用`top`、`df`、`free`等命令查看操作系统的基本情况，出现死锁可能会导致 CPU、内存等资源消耗过高。

\+ 采用 VisualVM、JConsole （首先，我们要找到JDK的bin目录，找到jconsole并双击打开；打开jconsole后，连接对应的程序，然后进入线程界面选择检测死锁即



可！）等工具进行排查。



\## 遇到OutOfMemoryError怎么排查解决

<font style="color:rgb(60, 60, 67);">可以通过 MAT、JVisualVM 等工具分析 Heap Dump 找到导致</font>`<font style="color:rgb(60, 60, 67);">OutOfMemoryError</font>`<font style="color:rgb(60, 60, 67);"> 的原因。</font>



比如：MAT 提供泄漏嫌疑（Leak Suspects）报告。它会基于启发式算法自动分析整个堆，直接指出最可疑的内存泄漏点，并给出详细的报告，包括问题组件、累积点（Accumulation Point）和引用链的图示。



<font style="color:rgb(60, 60, 67);">如果“泄漏嫌疑”报告不够明确，或者想要分析的是内存占用过高（而非泄漏）问题，可以切换到</font>\*\*支配树（Dominator Tree）\*\*<font style="color:rgb(60, 60, 67);">视图。这个视图将内存对象关系组织成一棵树，父节点“支配”子节点（即父节点被回收，子节点也必被回收）。</font>



\# 什么是Heap Dump（<img src="https://cdn.nlark.com/yuque/0/2026/png/63012352/1776686565285-a97fb701-b383-4bd9-a4ee-b147fe270732.png" width="49.6" title="" crop="0,0,1,1" id="ucb04e314" class="ne-image">）文件？如何生成Heap Dump文件？

Heap Dump（堆转储文件）是 Java 虚拟机（JVM）在某个特定时间点，对整个 <font style="background-color:#FBDE28;">Java </font>\*\*<font style="background-color:#FBDE28;">堆内存</font>\*\*的<font style="background-color:#FBDE28;">快照</font>。它是一个<font style="background-color:#FBDE28;">二进制文件</font>，包含了快照时刻堆中<font style="background-color:#FBDE28;">所有对象的信息</font>，例如：



\+ \*\*对象实例\*\*：每个对象的数据。

\+ \*\*类信息\*\*：对象的类名、父类、静态字段等。

\+ \*\*引用关系\*\*：对象之间复杂的引用链，即谁持有了谁。

\+ \*\*线程信息\*\*：堆栈信息，特别是与 GC Roots 相关的线程栈。



\## 自动生成

<font style="color:rgb(60, 60, 67);">在 JVM 启动参数中加入配置，这是生产环境排查 OOM 问题的首选方案。</font>



```bash

\# 当发生 OutOfMemoryError 时，自动生成 Heap Dump 文件

\-XX:+HeapDumpOnOutOfMemoryError



\# 指定 Heap Dump 文件的生成路径，例如：/home/app/dumps/

\-XX:HeapDumpPath=<path-to-dump-dir>

```



\## 手动生成

<font style="color:rgb(60, 60, 67);">当应用</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">出现内存疑似异常</font><font style="color:rgb(60, 60, 67);">（如内存持续升高、GC 频繁）</font><font style="color:rgb(60, 60, 67);background-color:#FBDE28;">但未崩溃时</font><font style="color:rgb(60, 60, 67);">，可以手动生成快照进行分析。</font>



1\. \*\*jmap\*\*<font style="color:rgb(60, 60, 67);"> ：JDK 自带的命令行工具，专门用于生成堆快照。使用示例：</font>`<font style="color:rgb(60, 60, 67);">jmap -dump:format=b,file=heapdump.hprof <pid></font>`<font style="color:rgb(60, 60, 67);">（jmap -dump:format=b,file=导出文件名 <Java进程ID>）。在执行时会触发 STW ，导致 Java 进程短暂停顿，对生产环境有一定影响。在高版本 JDK 中已不推荐直接使用。</font>

2\. \*\*jcmd\*\*<font style="color:rgb(60, 60, 67);"> ：JDK 7 之后引入的多功能命令行工具，功能比 jmap 更强大一些，可用来替代 jmap，侵入性更小。使用示例：</font>`<font style="color:rgb(60, 60, 67);">jcmd <pid> GC.heap\_dump /path/to/heapdump.hprof</font>`<font style="color:rgb(60, 60, 67);">。（jcmd <Java进程ID> GC.heap\_dump 文件路径）</font>

3\. \*\*Arthas（\*\*/a:sθs/\*\*）\*\*<font style="color:rgb(60, 60, 67);">：阿里巴巴开源的 Java 诊断神器，对应用无侵入，功能强大，可在不重启服务的情况下动态分析。使用示例：</font>`<font style="color:rgb(60, 60, 67);">heapdump /tmp/heapdump.hprof</font>`<font style="color:rgb(60, 60, 67);">。</font>

4\. \*\*可视化工具\*\*<font style="color:rgb(60, 60, 67);">：如 JVisualVM、JProfiler、YourKit 等，都提供了图形化界面，点击按钮即可生成 Heap Dump 文件，并能直接进行分析，非常方便。</font>











