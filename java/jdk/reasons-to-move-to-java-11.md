---
title: 迁移到 Java 11 的原因
description: 本文为摘要级别的文档，适用于那些正权衡从 Java 8 迁移到 Java 11 的优势的决策者。
author: dsgrieve
manager: maverberg
tags: java
ms.topic: article
ms.date: 11/19/2019
ms.author: dagrieve
ms.openlocfilehash: 7daf058c2abebbf2cca85dadc4f9ffe3e8771fa1
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2019
ms.locfileid: "74812218"
---
# <a name="reasons-to-move-to-java-11"></a>迁移到 Java 11 的原因

问题不是是否应迁移到 Java 11，而是何时进行迁移。   再过几年，我们将不再支持 Java 8，用户必须迁移到 Java 11。 我们认为，迁移到 Java 11 有很多好处，因此鼓励各团队尽快这样做。

自 Java 8 发布以来，我们已添加了多项新功能并对原有功能进行了强化。 我们对 API 进行了明显的添加和修改，并通过增强功能改善了启动、性能和内存使用情况。

## <a name="transitioning-to-java-11"></a>过渡到 Java 11

可以逐步过渡到 Java 11。 代码不需使用 Java 模块即可在 Java 11 上运行。  Java 11 可以用来运行通过 JDK 8 开发和生成的代码，
但存在一些潜在的问题，主要涉及弃用的 API、类加载程序和反射。

Microsoft Java 工程组很快会提供一个综合性指南，介绍如何从 Java 8 过渡到 Java 11。 另外还有许多介绍如何从 Java 8 过渡到 Java 9 的入门指南。 例如，[Java Platform, Standard Edition Oracle JDK 9 Migration Guide](https://docs.oracle.com/javase/9/migrate/toc.htm)（Java 平台标准版 Oracle JDK 9 迁移指南）和 [The State of the Module System:Compatibility and Migration](http://openjdk.java.net/projects/jigsaw/spec/sotms/#compatibility--migration)（模块系统状态：兼容性和迁移）。

## <a name="high-level-changes-between-java-8-and-11"></a>概述从 Java 8 到 11 的更改

此部分不会将 Java 版本 9 \[[1](#ref1)\]、10 \[[2](#ref2)\] 和 11 \[[3](#ref3)\] 中所做的全部更改都枚举出来， 但会重点介绍对性能、诊断和工作效率有影响的更改。

### <a name="modules-4ref4"></a>模块 \[[4](#ref4)\]

模块解决在大型应用程序（在 *classpath* 上运行）中难以管理的配置和封装问题。 模块是 Java 类和接口以及相关资源的自述性集合。 

有了模块，即可自定义那些仅包含应用程序所需组件的运行时配置。 此自定义产生的内存占用量较小，因此可以使用 [jlink](https://docs.oracle.com/en/java/javase/11/tools/jlink.html) 将应用程序静态链接到用于部署的自定义运行时中。 这个较小的内存占用量可能特别适用于微服务体系结构。

在内部，JVM 可以通过让类加载更有效的方式利用模块。 结果就是，运行时更小、更轻便且启动速度更快。 JVM 用来改善应用程序性能的优化技术可以更有效，因为模块可以对某个类需要哪些组件进行编码。

对程序员来说，模块可以要求显式声明一个模块可以导出哪些包以及它需要哪些组件，并且可以限制反射访问，因此有助于强制实施强封装。
这种级别的封装使应用程序更安全，维护起来更容易。

应用程序可以继续使用 *classpath*，不需转换为作为必备组件的模块即可在 Java 11 上运行。

### <a name="profiling-and-diagnostics"></a>分析和诊断

#### <a name="java-flight-recorder-5ref5"></a>Java Flight Recorder \[[5](#ref5)\]

Java Flight Recorder (JFR) 从正在运行的 Java 应用程序中收集诊断和分析数据。 JFR 对正在运行的 Java 应用程序几乎没有影响。 收集的数据随后可以使用 Java Mission Control (JMC) 和其他工具进行分析。 虽然 JFR 和 JMC 在 Java 8 中都是商业功能，但二者在 Java 11 中都是开放源代码。

#### <a name="java-mission-control-6ref6"></a>Java Mission Control \[[6](#ref6)\]

Java Mission Control (JMC) 可以通过图形方式显示 Java Flight Recorder (JFR) 收集的数据，在 Java 11 中是开放源代码。
11. 除了收集正在运行的应用程序的常规信息，JMC 还允许用户向下钻取到数据中。 JFR 和 JMC 可以用来诊断运行时问题，例如内存泄露、GC 开销、热方法、线程瓶颈、阻塞 I/O。

#### <a name="unified-logging-7ref7"></a>统一日志记录 \[[7](#ref7)\]

Java 11 有一个通用日志记录系统，适合 JVM 的所有组件。
用户可以使用此统一日志记录系统来定义哪些组件需要记录，以及记录到何种级别。 这种精细的日志记录适用于对 JVM 崩溃进行根本原因分析，以及在生产环境中诊断性能问题。

#### <a name="low-overhead-heap-profiling-8ref8"></a>低开销堆分析 \[[8](#ref8)\]

已经向 Java 虚拟机工具接口 (JVMTI) 添加了新的 API，用于对 Java 堆分配采样。 采样的开销低，可以持续启用。 虽然可以使用 Java Flight Recorder (JFR) 监视堆分配，但 JFR 中的采样方法只能用于分配。 JFR 实现也可能未命中分配。 与之形成对比的是，Java 11 中的堆采样可以同时提供活对象和死对象的相关信息。

应用程序性能监视 (APM) 供应商开始利用此新功能，Java 工程组正在研究是否有可能将它与 Azure 性能监视工具配合使用。

#### <a name="stackwalker-9ref9"></a>StackWalker \[[9](#ref9)\]

进行日志记录时，通常会获取当前线程的堆栈的快照。 问题在于要记录多少堆栈跟踪，以及是否有必要记录堆栈跟踪。 例如，用户可能只想在某个方法出现特定异常时查看堆栈跟踪。 StackWalker 类（在 Java 9 中添加）提供堆栈的快照，并提供方便程序员对堆栈跟踪使用方式进行精细控制的方法。

### <a name="garbage-collection-10ref10"></a>垃圾回收 \[[10](#ref10)\]

Java 11 提供以下垃圾回收器：串行、并行、Garbage-First 和 Epsilon。 Java 11 中的默认垃圾回收器是 Garbage First 垃圾回收器 (G1GC)。

此处提到其他三个回收器为了保持内容完整。 Z 垃圾回收器 (ZGC) 是一个并发、低延迟回收器，它会尝试将暂停时间保持在 10 毫秒以下。 ZGC 在 Java 11 中作为实验性功能提供。 Shenandoah 回收器是一个暂停时间短的回收器，它可以通过正在运行的 Java 程序以并发方式进行更多的垃圾回收，因此缩短了 GC 暂停时间。
Shenandoah 是 Java 12 中的一项实验性功能，但可以后向移植到 Java 11。 Concurrent Mark and Sweep (CMS) 回收器已发布，但自 Java 9 发布后已弃用。

对于一般性使用，JVM 会将 GC 用作默认设置。 通常情况下，需根据应用程序的要求对这些设置和其他 GC 设置进行调整，以便优化吞吐量或延迟。
正确调整 GC 需要深入了解 GC，需要 [Microsoft Java 工程组](mailto:javaplatformgroup@microsoft.com)提供的专业知识。

#### <a name="g1gc"></a>G1GC

Java 11 中的默认垃圾回收器是 G1 垃圾回收器 (G1GC)。 G1GC 的目标是在延迟和吞吐量之间取得平衡。 G1 垃圾回收器尝试在大概率满足暂停时间目标的情况下实现高吞吐量目标。 G1GC 旨在避免完全回收，但当并发回收不能以足够快的速度回收内存时，则会进行回退性的完全 GC。 完全 GC 使用与初期混合性回收相同的并行工作线程数。

#### <a name="parallel-gc"></a>并行 GC

并行回收器是 Java 8 中的默认回收器。 并行 GC 是一个吞吐量回收器，使用多个线程来加速垃圾回收。

#### <a name="epsilon-11ref11"></a>Epsilon \[[11](#ref11)\]

Epsilon 垃圾回收器负责处理分配，但不回收任何内存。 当堆耗尽时，JVM 会关闭。
Epsilon 适用于生存期短的服务和已知没有垃圾的应用程序。

#### <a name="improvements-for-docker-containers-12ref12"></a>Docker 容器改进 \[[12](#ref12)\]

在 Java 10 之前，JVM 无法识别在容器上设置的内存和 CPU 约束。 例如，在 Java 8 中，JVM 会将最大堆大小默认设置为基础主机物理内存的 ¼。 从 Java 10 开始，JVM 会使用容器控制组 (cgroups) 设置的约束来设置内存和 CPU 限制（参见下面的说明）。
例如，默认的最大堆大小为容器的内存限制的 ¼（例如，如果内存限制为 2G，则最大堆大小为 500MB）。

另外还添加了“JVM 选项”，使 Docker 容器用户可以精细地控制用于 Java 堆的系统内存量。

此支持默认启用，仅在基于 Linux 的平台上提供。

> [!NOTE]
> 大多数 cgroup 支持工作可以后向移植到 Java 8（直至 jdk8u191）。 进一步的改进不一定能够后向移植到 8。

#### <a name="multi-release-jar-files-13ref13"></a>多发布版 jar 文件 \[[13](#ref13)\]

在 Java 11 中，可以创建一个 jar 文件，其中包含多个特定于 Java 发布版的类文件版本。 有了多发布版 jar 文件，库开发人员就可以支持多个 Java 版本，不需交付多个版本的 jar 文件。 对于这些库的使用者来说，多发布版 jar 文件解决了必须将特定 jar 文件与特定运行时目标匹配的问题。

## <a name="miscellaneous-performance-improvements"></a>其他性能改进

对 JVM 进行以下更改会直接影响性能。

-   **JEP 197：分段代码缓存** \[[14](#ref14)\] - 将代码缓存分成不同的段。 这种分段可以更好地控制 JVM 内存占用、缩短已编译方法的扫描时间、显著减轻代码缓存的碎片化，从而改进性能。

-   **JEP 254：压缩字符串** \[[15](#ref15)\] - 将字符串的内部表示形式从每个字符两个字节更改为每个字符一到两个字节，具体取决于字符编码。 由于大多数字符串包含 ISO-8859-1/拉丁语-1 字符，此更改可以有效地将存储字符串所需的空间量减半。

-   **JEP 310：应用程序类-数据共享** \[[16](#ref16)\] - 类-数据共享允许在运行时对存档的类进行内存映射，从而缩短启动时间。 应用程序类-数据共享允许将应用程序类置于 CDS 存档中，从而扩展了类-数据共享。 当多个 JVM 共享同一存档文件时，可以节省内存并缩短总体的系统响应时间。

-   **JEP 312：线程本地握手** \[[17](#ref17)\] - 可以在线程上执行回调而不需执行全局 VM 安全点，这样就可以减少全局安全点的数目，有助于 VM 降低延迟。

-   **延迟分配编译器线程** \[[18](#ref18)\] - 在分层编译模式下，VM 会启动大量的编译器线程。
    在有许多 CPU 的系统上，这是默认模式。 不管可用内存为多少，也不管编译请求有多少个，都会创建这些线程。 线程即使在空闲（几乎所有时间都是如此）的情况下也会耗用内存，这导致资源使用效率不高。 为了解决此问题，我们对实现进行了更改，在启动时每种类型只启动一个编译器线程。 系统会动态处理启动其他线程和关闭未使用线程的操作。 

对核心库进行以下更改会影响新代码或已修改代码的性能。

-   **JEP 193：变量句柄** \[[19](#ref19)\] - 定义一种标准的方式，在对象字段和数组元素上调用各种 java.util.concurrent.atomic 操作和 sun.misc.Unsafe 操作的等效项；定义一种标准的围栏操作集，以便精细控制内存排序；定义一种标准的可访问性围栏操作，确保引用的对象始终保持高的可访问性。

-   **JEP 269：用于集合的便利工厂方法** \[[20](#ref20)\] - 定义库 API，这样就可以方便地创建包含少量元素的集合和映射的实例。 这是集合接口上的静态工厂方法，用于创建精简且不可修改的集合实例。 这些实例本质上更高效。 这些 API 创建的集合以简洁方式表示，没有包装器类。

-   **JEP 285：自旋等待提示** \[[21](#ref21)\] - 提供的 API 允许 Java 向运行时系统提示：它处于自旋循环中。 某些硬件平台可以利用表明线程正处于“繁忙-等待”状态的软件指示。

-   **JEP 321：HTTP 客户端（标准）** \[[22](#ref22)\]- 提供的新 HTTP 客户端 API 可实现 HTTP/2 和 WebSocket，并可替换旧版 HttpURLConnection API。

## <a name="references"></a>参考

<a id="ref1">\[1\]</a> Oracle Corporation：\"Java Development Kit 9 Release Notes\"（Java 开发工具包 9 发行说明）（在线）。 请访问： https://www.oracle.com/technetwork/java/javase/9u-relnotes-3704429.html 。
（在 2019 年 11 月 13 日访问）。

<a id="ref2">\[2\]</a> Oracle Corporation：\"Java Development Kit 10 Release Notes\"（Java 开发工具包 10 发行说明）（在线）。 请访问： https://www.oracle.com/technetwork/java/javase/10u-relnotes-4108739.html 。
（在 2019 年 11 月 13 日访问）。

<a id="ref3">\[3\]</a> Oracle Corporation：\"Java Development Kit 11 Release Notes\"（Java 开发工具包 11 发行说明）（在线）。 请访问： https://www.oracle.com/technetwork/java/javase/11u-relnotes-5093844.html 。
（在 2019 年 11 月 13 日访问）。

<a id="ref4">\[4\]</a> Oracle Corporation：\"Project Jigsaw\"（Jigsaw 项目），9 月 22 日， 
2017. （在线）。 请访问： http://openjdk.java.net/projects/jigsaw/ 。
（在 2019 年 11 月 13 日访问）。

<a id="ref5">\[5\]</a> Oracle Corporation：\"JEP 328:Flight Recorder\"（JEP 328：Flight Recorder），2018 年 9 月9日。 （在线）。 请访问： http://openjdk.java.net/jeps/328 。 （在 2019 年 11 月 13 日访问）。

<a id="ref6">\[6\]</a> Oracle Corporation：\"Mission Control\"，2019 年 4 月 25 日。 （在线）。 请访问： https://wiki.openjdk.java.net/display/jmc/Main 。 （在 2019 年 11 月 13 日访问）。

<a id="ref7">\[7\]</a> Oracle Corporation：\"JEP 158:Unified JVM Logging\"（JEP 158：统一 JVM 日志记录），2019 年 2 月 14 日。 （在线）。 请访问： http://openjdk.java.net/jeps/158 。
（在 2019 年 11 月 13 日访问）。

<a id="ref8">\[8\]</a> Oracle Corporation：\"JEP 331:Low-Overhead Heap Profiling\"（JEP 331：低开销堆分析），2018 年 9 月 5 日。 （在线）。 请访问： http://openjdk.java.net/jeps/331 。 （在 2019 年 11 月 13 日访问）。

<a id="ref9">\[9\]</a> Oracle Corporation：\"JEP 259:Stack-Walking API\"（JEP 259：Stack-Walking API），2017 年 7 月 18 日。 （在线）。
请访问： http://openjdk.java.net/jeps/259 。 （在 2019 年 11 月 13 日访问）。

<a id="ref10">\[10\]</a> Oracle Corporation：\"JEP 248:Make G1 the Default Garbage Collector\"（JEP 248：将 G1 设置为默认垃圾回收器），2017 年 9 月 12 日。 （在线）。 请访问： http://openjdk.java.net/jeps/248 。 （在 2019 年 11 月 13 日访问）。

<a id="ref11">\[11\]</a> Oracle Corporation：\"JEP 318:Epsilon:A No-Op Garbage Collector,\"（JEP 318：Epsilon：一种无操作垃圾回收器），2018 年 9 月 24 日。
（在线）。 请访问： http://openjdk.java.net/jeps/318 。 （在 2019 年 11 月 13 日访问）。

<a id="ref12">\[12\]</a> Oracle Corporation：\"JDK-8146115:Improve docker container detection and resource configuration usage\"（JDK-8146115：改进 Docker 容器检测和资源配置使用情况），2019 年 9 月 16 日。
（在线）。 请访问： https://bugs.java.com/bugdatabase/view\_bug.do?bug\_id=JDK-8146115 。
（在 2019 年 11 月 13 日访问）。

<a id="ref13">\[13\]</a> Oracle Corporation：\"JEP 238:Multi-Release JAR Files\"（JEP 238：多发行版 JAR 文件），2017 年 6 月 22 日。 （在线）。 请访问： http://openjdk.java.net/jeps/238 。 （在 2019 年 11 月 13 日访问）。

<a id="ref14">\[14\]</a> Oracle Corporation：\"JEP 197:Segmented Code Cache\"（JEP 197：分段代码缓存），2017 年 4 月 28 日。 （在线）。
请访问： http://openjdk.java.net/jeps/197 。 （在 2019 年 11 月 13 日访问）。

<a id="ref15">\[15\]</a> Oracle Corporation：\"JEP 254:Compact Strings\"（JEP 254：压缩字符串），2019 年 5 月 18 日。 （在线）。 请访问： http://openjdk.java.net/jeps/254 。
（在 2019 年 11 月 13 日访问）。

<a id="ref16">\[16\]</a> Oracle Corporation：\"JEP 310:Application Class-Data Sharing\"（JEP 310：应用程序类-数据共享），2018 年 8 月 17 日。 （在线）。 请访问： https://openjdk.java.net/jeps/310 。 （在 2019 年 11 月 13 日访问）。

<a id="ref17">\[17\]</a> Oracle Corporation：\"JEP 312:Thread-Local Handshakes\"（JEP 312：线程本地握手），2019 年 8 月 21 日。
（在线）。 请访问： https://openjdk.java.net/jeps/312 。 （在 2019 年 11 月 13 日访问）。

<a id="ref18">\[18\]</a> Oracle Corporation：\"JDK-8198756:Lazy allocation of compiler threads\"（JDK-8198756：延迟分配编译器线程），2018 年 10 月 29 日。 （在线）。 请访问： https://bugs.java.com/bugdatabase/view\_bug.do?bug\_id=8198756 。
（在 2019 年 11 月 13 日访问）。

<a id="ref19">\[19\]</a> Oracle Corporation：\"JEP 193:Variable Handles\"（JEP 193：变量句柄），2017 年 8 月 17 日。 （在线）。 请访问： https://openjdk.java.net/jeps/193 。 （在 2019 年 11 月 13 日访问）。

<a id="ref20">\[20\]</a> Oracle Corporation：\"JEP 269:Convenience Factory Methods for Collections\"（JEP 269：用于集合的便利工厂方法），2017 年 6 月 26 日。 （在线）。 请访问： https://openjdk.java.net/jeps/269 。
（在 2019 年 11 月 13 日访问）。

<a id="ref21">\[21\]</a> Oracle Corporation：\"JEP 285:Spin-Wait Hints\"（JEP 285：自旋等待提示），2017 年 8 月 20 日。 （在线）。 请访问： https://openjdk.java.net/jeps/285 。 （在 2019 年 11 月 13 日访问）。

<a id="ref22">\[22\]</a> Oracle Corporation：\"JEP 321:HTTP Client (Standard)\"（JEP 321：HTTP 客户端（标准）），2018 年 9 月 27 日。 （在线）。
请访问： https://openjdk.java.net/jeps/321 。 （在 2019 年 11 月 13 日访问）。
