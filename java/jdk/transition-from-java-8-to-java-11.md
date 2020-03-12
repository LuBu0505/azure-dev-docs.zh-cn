---
title: 从 Java 8 转换到 Java 11
titleSuffix: Azure
description: 从 Java 8 到 Java 11 的迁移管理指南。
author: dsgrieve
manager: maverbur
tags: java
ms.service: azure
ms.devlang: java
ms.topic: article
ms.date: 11/19/2019
ms.author: dagrieve
ms.openlocfilehash: 528b111e945bb68bd18c849847522a070259c0f3
ms.sourcegitcommit: f1e3c72c38376b15f5313d4bfe5fefdbfc022dc9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "79022306"
---
# <a name="transition-from-java-8-to-java-11"></a>从 Java 8 转换到 Java 11

将代码从 Java 8 转换到 Java 11 时，并没有一种适用于所有情况的解决方案。
对于不重要的应用程序来说，从 Java 8 迁移到 Java 11 可能意味着很大的工作量。 潜在问题包括：删除的 API、弃用的包、内部 API 的使用、对类加载程序的更改，以及对垃圾回收的更改。 

通常，解决方法是尝试在不重新编译的情况下在 Java 11 上运行，或者先使用 JDK 11 进行编译。 如果目标是尽快启动并运行应用程序，则通常情况下，最佳方法是直接在 Java 11 上运行。 对于库，目标将是发布使用 JDK 11 编译和测试的项目。

迁移到 Java 11 值得付出这样的努力。 自 Java 8 发布以来，我们已添加了多项新功能并对原有功能进行了强化。 这些功能和增强功能可改进启动、性能和内存使用情况，并提供与容器更好的集成。 此外还对 API 进行了添加和修改，这可以提高开发人员的工作效率。 

本文档介绍了用于检查代码的工具。 它还介绍了你可能遇到的问题以及解决这些问题的建议。 你还应参阅其他指南，如 [Oracle JDK Migration Guide](https://docs.oracle.com/en/java/javase/11/migrate/index.html)（Oracle JDK 迁移指南）。 本文不介绍如何将现有代码[模块化](http://openjdk.java.net/projects/jigsaw)。  


## <a name="the-toolbox"></a>工具箱

Java 11 有两个用于探查潜在问题的工具：*jdeprscan* 和 *jdeps*。 可以对现有类或 jar 文件运行这两个工具。 无需重新编译即可评估转换工作量。 

[jdeprscan](https://docs.oracle.com/en/java/javase/11/tools/jdeprscan.html) 可查看是否使用了已弃用或已删除的 API。
使用已弃用的 API 不是阻塞性问题，但值得探讨。 是否有更新的 jar 文件？ 是否需要记录某个问题才能解决已弃用 API 的使用问题？ 使用已删除的 API 是阻塞性问题，必须予以解决，然后才能尝试在 Java 11 上运行应用程序。


[jdeps](https://docs.oracle.com/en/java/javase/11/tools/jdeps.html)，一个 Java 类依赖关系分析器。 与 `--jdk-internals` 选项一起使用时，*jdeps* 会告诉你哪个类依赖于哪个内部 API。 可以继续使用 Java 11 中的内部 API，但应优先考虑改变这种使用情况。 OpenJDK Wiki 页面 [Java Dependency Analysis Tool](https://wiki.openjdk.java.net/display/JDK8/Java+Dependency+Analysis+Tool)（Java 依赖关系分析工具）推荐了某些常用 JDK 内部 API 的替换项。 

Gradle 和 Maven 都有 *jdeps* 和 *jdeprscan* 插件。 建议将以下工具添加到生成脚本中。 

> [!div class="mx-tdBreakAll"]
> |工具|Gradle 插件|Maven 插件|
> |-|-|-|
> |jdeps|[jdeps-gradle-plugin](https://github.com/kordamp/jdeps-gradle-plugin)|[Apache Maven JDeps 插件](https://maven.apache.org/plugins/maven-jdeps-plugin/index.html)|
> |jdeprscan|[jdeprscan-gradle-plugin](https://github.com/kordamp/jdeprscan-gradle-plugin)|[Apache Maven JDeprScan 插件](https://maven.apache.org/plugins/maven-jdeprscan-plugin/index.html)|

Java 编译器本身 *javac* 是工具箱中的另一个工具。 从 *jdeprscan* 和 *jdeps* 获取的警告和错误来自编译器。  使用 *jdeprscan* 和 *jdeps* 的优点是，可以在现有的 jar 和类文件（包括第三方库）上运行这两个工具。

*jdeprscan* 和 *jdeps* 不能做的是对使用反射来访问封装的 API 进行警告。 反射访问在运行时进行检查。 最终必须在 Java 11 上运行代码才能确切地知道。

### <a name="using-jdeprscan"></a>使用 jdeprscan

若要使用 [jdeprscan](https://docs.oracle.com/en/java/javase/11/tools/jdeprscan.html)，最简单的方法是为其提供一个来自现有生成的 jar 文件。 还可以为其指定目录（如编译器输出目录）或单个类名。 使用 `--release 11` 选项可获取已弃用 API 的最完整列表。 若要确定要采用的已弃用 API 的优先级，请将设置回退到 `--release 8`。 在 Java 8 中弃用的 API 的删除时间可能会早于最近弃用的 API。 

```console
jdeprscan --release 11 my-application.jar
```

如果无法解析依赖类，*jdeprscan* 工具会生成错误消息。
例如，`error: cannot find class org/apache/logging/log4j/Logger` 。 建议将依赖类添加到 `--class-path` 或使用应用程序 class-path，但该工具会在没有它的情况下继续扫描。
参数为 *&#8209;&#8209;class&#8209;path*。 class-path 参数的其他变体将不起作用。

```console
jdeprscan --release 11 --class-path log4j-api-2.13.0.jar my-application.jar
error: cannot find class sun/misc/BASE64Encoder
class com/company/Util uses deprecated method java/lang/Double::<init>(D)V
```
此输出告诉我们，`com.company.Util` 类在调用 `java.lang.Double` 类的已弃用构造函数。 javadoc 会建议用来代替已弃用 API 的 API。 无论如何都无法解决“`error: cannot find class sun/misc/BASE64Encoder`”问题，因为它是已删除的 API。 自 Java 8 发布以来，应使用 `java.util.Base64`。 

运行 `jdeprscan --release 11 --list` 即可了解自 Java 8 后弃用的具体 API。 若要获取已删除 API 的列表，请运行 `jdeprscan --release 11 --list --for-removal`。

### <a name="using-jdeps"></a>使用 jdeps

可以使用 [jdeps](https://docs.oracle.com/en/java/javase/11/tools/jdeps.html) 通过 `--jdk-internals` 选项来查找 JDK 内部 API 上的依赖项。 此示例需要 `--multi-release 11` 命令行选项，因为 *log4j-core-2.13.0.jar* 是[多版本 jar 文件](https://docs.oracle.com/en/java/javase/11/docs/specs/jar/jar.html#multi-release-jar-files)。 没有此选项，*jdeps* 会在找到多版本 jar 文件的情况下发出错误消息。 此选项指定要检查的类文件的版本。 

```console
jdeps --jdk-internals --multi-release 11 --class-path log4j-core-2.13.0.jar my-application.jar
Util.class -> JDK removed internal API
Util.class -> jdk.base
Util.class -> jdk.unsupported
   com.company.Util        -> sun.misc.BASE64Encoder        JDK internal API (JDK removed internal API)
   com.company.Util        -> sun.misc.Unsafe               JDK internal API (jdk.unsupported)
   com.company.Util        -> sun.nio.ch.Util               JDK internal API (java.base)

Warning: JDK internal APIs are unsupported and private to JDK implementation that are
subject to be removed or changed incompatibly and could break your application.
Please modify your code to eliminate dependence on any JDK internal APIs.
For the most recent update on JDK internal API replacements, please check:
https://wiki.openjdk.java.net/display/JDK8/Java+Dependency+Analysis+Tool

JDK Internal API                         Suggested Replacement
----------------                         ---------------------
sun.misc.BASE64Encoder                   Use java.util.Base64 @since 1.8
sun.misc.Unsafe                          See http://openjdk.java.net/jeps/260   

```

输出提供了一些关于避免使用 JDK 内部 API 的好建议！ 如果可能，建议使用替换 API。 在括号中提供封装了包的模块的名称。 如果需要显式[中断封装](https://docs.oracle.com/javase/9/migrate/toc.htm#JSMIG-GUID-2F61F3A9-0979-46A4-8B49-325BA0EE8B66)，则可将模块名称与 `--add-exports` 或 `--add-opens` 配合使用。 

使用 *sun.misc.BASE64Encoder* 或 *sun.misc.BASE64Decoder* 会导致 Java 11 中出现 *java.lang.NoClassDefFoundError*。 使用这些 API 的代码必须经过修改才能使用 *java.util.Base64*。 

尝试不使用来自 *jdk.unsupported* 模块的任何 API。 此模块中的 API 将引用 [JDK 增强方案 (JEP) 260](http://openjdk.java.net/jeps/260) 作为建议的替换方案。
简而言之，JEP 260 指出，在替换 API 可用之前，会一直支持使用内部 API。 虽然你的代码使用的是 JDK 内部 API，但至少在一段时间内它是可以正常运行的。 请看看 JEP 260，因为它指出了某些内部 API 的替换项。 
例如，可以使用[变量句柄](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/invoke/VarHandle.html)来代替某个 *sun.misc.Unsafe* API。 

除了扫描 JDK 内部 API 的使用情况，*jdeps* 还可以执行其他操作。 它是一项有用的工具，可以用来分析依赖关系和生成模块信息文件。 有关详细信息，请参阅[文档](https://docs.oracle.com/en/java/javase/11/tools/jdeps.html)。

### <a name="using-javac"></a>使用 javac

如果使用 JDK 11 进行编译，则需要更新才能生成脚本、工具、测试框架和包含的库。 使用 *javac* 的 `-Xlint:unchecked` 选项可获取 JDK 内部 API 的使用详情和其他警告。 可能还需要使用 `--add-opens` 或 `--add-reads` 向编译器公开封装的包（请参阅 [JEP 261](http://openjdk.java.net/jeps/261)）。 

库可以考虑以[多版本 jar 文件](https://docs.oracle.com/en/java/javase/11/docs/specs/jar/jar.html#multi-release-jar-files)形式打包。 多版本 jar 文件允许同时支持同一 jar 文件中的 Java 8 和 Java 11 运行时。 它们增加了生成的复杂性。 如何生成多版本 jar 超出了本文档的讨论范围。 

## <a name="running-on-java-11"></a>在 Java 11 上运行

大多数应用程序在不修改的情况下应该可以在 Java 11 上运行。 首先要尝试的是在不重新编译代码的情况下在 Java 11 上运行。 直接运行的目的是查看执行时会出现哪些警告和错误。 此方法可以  
让应用程序在 Java 11 上更快地运行，因为可以尽量减少那些必须完成的关注事项。 

你可能会遇到的大多数问题都可以得到解决，无需重新编译代码。
如果需要在代码中修复问题，请进行修复，但继续使用 JDK 8 进行编译。 如果可能，请在使用 JDK 11 进行编译  之前，让应用程序使用 `java` 版本 11 运行  。 

### <a name="check-command-line-options"></a>检查命令行选项

在 Java 11 上运行之前，请对命令行选项进行快速扫描。 
[已删除的选项](#unrecognized-options)会导致 Java 虚拟机 (JVM) 退出。 如果使用 GC 日志记录选项，则此检查尤其重要，因为它们已明显不同于 Java 8 中的情况。 [JaCoLine](https://jacoline.dev/about) 工具是一项很好的工具，用于检查命令行选项的问题。 

### <a name="check-third-party-libraries"></a>检查第三方库

你不能控制的第三方库是潜在的问题来源。 可以主动将第三方库更新到较新的版本。 也可查看运行应用程序时哪些库未使用，仅更新那些必需的库。 将所有库更新到最新版本的问题在于，如果应用程序中存在错误，则更难找到根本原因。 发生此错误是因为更新了某个库吗？ 或者，此错误是由运行时中的某些更改引起的吗？ 仅更新所需内容的问题在于，可能需要多次迭代才能解决问题。

此处的建议是尽可能少做更改，将第三方库[单独](#next-steps)进行更新。 如果更新第三方库，则往往需要与 Java 11 兼容的最新且最好的版本。 根据当前版本的落后程度，你可能需要采取更谨慎的方法，升级到第一个与 Java 9+ 兼容的版本。 

除了查看发行说明以外，还可以使用 *jdeps* 和 *jdeprscan* 来评估 jar 文件。 此外，OpenJDK 质量组还维护一个[质量外展](https://wiki.openjdk.java.net/display/quality/Quality+Outreach) Wiki 页面，其中列出了根据 OpenJDK 版本对多个免费开源软件 (FOSS) 项目进行的测试的状态。 

### <a name="explicitly-set-garbage-collection"></a>显式设置垃圾回收

并行垃圾回收器（并行 GC）是 Java 8 中的默认 GC。 如果应用程序使用默认值，则应使用命令行选项 `-XX:+UseParallelGC` 显式设置 GC。
Java 9 中的默认值已更改为 Garbage First 垃圾回收器 (G1GC)。 若要对 Java 8 与 Java 11 上运行的应用程序进行公平比较，GC 设置必须相同。 应推迟使用 GC 设置进行的试验，直到应用程序在 Java 11 上经过验证。 

### <a name="explicitly-set-default-options"></a>显式设置默认选项

如果在作用点 VM 上运行，则设置命令行选项 `-XX:+PrintCommandLineFlags` 会转储由 VM 设置的选项的值，特别是由 GC 设置的默认值。
在 Java 8 上使用此标志运行，在 Java 11 上运行时使用输出的选项。 大多数情况下，Java 8 到 11 中的默认值是相同的。 但是，使用 Java 8 中的设置可确保奇偶校验。

建议设置命令行选项 `--illegal-access=warn`。
在 Java 11 中，使用反射访问 JDK 内部 API 会生成一个[“非法的反射访问”警告](#warning-an-illegal-reflective-access-operation-has-occurred)。
默认情况下，系统仅对第一次非法访问发出警告。 设置 `--illegal-access=warn` 会导致系统对每一次  非法反射访问发出警告。 如果将选项设置为 *warn*，则会发现更多非法访问案例。 但是，你也会收到大量冗余警告。  
在 Java 11 上运行应用程序后，设置 `--illegal-access=deny` 即可模拟 Java 运行时的未来行为。 从 Java 16 开始，默认设置将为 `--illegal-access=deny`。 

### <a name="classloader-cautions"></a>ClassLoader 注意事项

在 Java 8 中，可以将系统类加载程序强制转换为 `URLClassLoader`。 这通常由需要在运行时将类注入到 classpath 的应用程序和库完成。 类加载程序层次结构在 Java 11 中已更改。 系统类加载程序（也称为应用程序类加载程序）现在是一个内部类。 强制转换为 `URLClassLoader` 会在运行时引发 `ClassCastException`。 Java 11 无法通过 API 在运行时动态增强 classpath，但可以通过反射来实现这一点，它会显示有关如何使用内部 API 的显著警告。 

在 Java 11 中，启动类加载程序只加载核心模块。 如果创建一个具有 null 父项的类加载程序，则它可能找不到全部平台类。 在 Java 11 中，需要在此类情况下传递 `ClassLoader.getPlatformClassLoader()` 而不是 `null` 作为父类加载程序。 

### <a name="locale-data-changes"></a>区域设置数据更改

Java 11 中区域设置数据的默认源已通过 [JEP 252](http://openjdk.java.net/jeps/252) 更改为 Unicode 联合会的公共区域设置数据存储库。 这可能会影响本地化的格式设置。 如有必要，请将系统属性设置为 `java.locale.providers=COMPAT,SPI`，还原到 Java 8 区域设置行为。 

### <a name="potential-issues"></a>潜在问题

下面是可能会遇到的一些常见问题。 有关这些问题的更多详细信息，请单击链接。

- [无法识别的 VM 选项](#unrecognized-options)
- [无法识别的选项](#unrecognized-options)
- [VM 警告：忽略选项](#vm-warnings)
- [VM 警告：选项 &lt;*option*&gt; 已弃用](#vm-warnings)
- [警告：发生非法的反射访问操作](#warning-an-illegal-reflective-access-operation-has-occurred)
- [java.lang.reflect.InaccessibleObjectException](#javalangreflectinaccessibleobjectexception)
- [java.lang.NoClassDefFoundError](#javalangnoclassdeffounderror)
- [-Xbootclasspath/p 不再是受支持的选项](#-xbootclasspathp-is-no-longer-a-supported-option)
- [java.lang.UnsupportedClassVersionError](#unsupportedclassversionerror)

#### <a name="unrecognized-options"></a>无法识别的选项

如果删除了某个命令行选项，则应用程序会输出 `Unrecognized option:` 或 `Unrecognized VM option`，后跟有问题的选项的名称。 无法识别的选项会导致 VM 退出。
已弃用但未删除的选项会生成 [VM 警告](#vm-warnings)。

通常情况下，已删除的选项没有替换项，唯一办法是从命令行中删除该选项。 垃圾回收日志记录的选项是一个例外。 GC 日志记录已在 Java 9 中[重新实现](http://openjdk.java.net/jeps/271)，可以使用[统一 JVM 日志记录框架](http://openjdk.java.net/jeps/158)。 请参阅 Java SE 11 工具参考的[允许通过 JVM 统一日志记录框架进行日志记录](https://docs.oracle.com/en/java/javase/11/tools/java.html#GUID-BE93ABDC-999C-4CB5-A88B-1994AAAC74D5)部分中的“表2-2 将旧的垃圾回收日志记录标志映射到 Xlog 配置”。 

#### <a name="vm-warnings"></a>VM 警告

使用弃用的选项会生成警告。 当某个选项被替换或不再有用时，即表明它已被弃用。 与使用[删除的选项](#unrecognized-options)一样，应从命令行中删除这些选项。
“`VM Warning: Option <option> was deprecated`”警告意味着，该选项仍受支持，但以后可能会取消该支持。 不再受支持的选项会生成“`VM Warning: Ignoring option`”警告。
不再受支持的选项不影响运行时。

Web 页面 [VM 选项资源管理器](https://chriswhocodes.com/hotspot_option_differences.html)提供了自 JDK 7 以后在 Java 中添加或删除的选项的详尽列表。 

#### <a name="error-could-not-create-the-java-virtual-machine"></a>错误：无法创建 Java 虚拟机

当 JVM 遇到[无法识别的选项](#unrecognized-options)时，会输出此错误消息。

#### <a name="warning-an-illegal-reflective-access-operation-has-occurred"></a>警告：发生非法的反射访问操作

当 Java 代码使用反射访问 JDK 内部 API 时，运行时会发出“非法的反射访问”警告。

```console
WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by my.sample.Main (file:/C:/sample/) to method sun.nio.ch.Util.getTemporaryDirectBuffer(int)
WARNING: Please consider reporting this to the maintainers of com.company.Main
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release
```

这意味着，模块未导出通过反射访问的包。 此包在模块中封装  ，本质上是内部 API。 在 Java 11 上启动并运行应用程序时，第一项操作可能就是忽略此警告。
Java 11 运行时允许反射访问，因此旧代码可以继续运行。  

若要解决此警告，请查找不使用内部 API 的已更新代码。 如果无法使用更新的代码解决该问题，则可使用 `--add-exports` 或 `--add-opens` 命令行选项来启用对包的访问权限。
这些选项允许从一个模块访问另一个模块的未导出类型。

[`--add-exports`](https://docs.oracle.com/javase/9/migrate/toc.htm#JSMIG-GUID-2F61F3A9-0979-46A4-8B49-325BA0EE8B66) 选项允许目标模块访问源模块的命名包的公共  类型。 有时，代码会使用 `setAccessible(true)` 访问非公共成员和 API。 这称为深度反射  。 在这种情况下，请使用 [`--add-opens`](https://docs.oracle.com/javase/9/migrate/toc.htm#JSMIG-GUID-12F945EB-71D6-46AF-8C3D-D354FD0B1781)，允许代码访问包的非公共成员。 如果不确定是使用 *--add-exports* 还是 *--add-opens*，请从 *--add-exports* 着手。 

应将 `--add-exports` 或 `--add-opens` 选项视为一种权宜解决方案，而不是长期解决方案。
使用这些选项会打破模块系统的封装，该封装是为了防止 JDK 内部 API 被使用。  如果删除或更改内部 API，应用程序会发生故障。  Java 16 会拒绝反射访问，但通过命令行选项（如 `--add-opens`）启用访问的情况除外。
若要模拟未来行为，请在命令行中设置 `--illegal-access=deny`。

发出上述示例中的警告是因为 `sun.nio.ch` 包不是由 `java.base` 模块导出的。 换言之，模块 `java.base` 的 `module-info.java` 文件中没有 `exports sun.nio.ch;`。 这可以通过 `--add-exports=java.base/sun.nio.ch=ALL-UNNAMED` 来解决。 未在模块中定义的类隐式属于未命名  模块，在字面上命名为 `ALL-UNNAMED`。

#### <a name="javalangreflectinaccessibleobjectexception"></a>java.lang.reflect.InaccessibleObjectException

此异常指示你尝试在已封装类的字段或方法上调用 `setAccessible(true)`。 也可能会收到一个[“非法的反射访问”警告](#warning-an-illegal-reflective-access-operation-has-occurred)。 使用 [`--add-opens`](https://docs.oracle.com/javase/9/migrate/toc.htm#JSMIG-GUID-12F945EB-71D6-46AF-8C3D-D354FD0B1781) 选项可以让代码访问包的非公共成员。 异常消息会告知你，模块未将包打开到试图调用 *setAccessible* 的模块。 如果模块是“未命名模块”，请使用 `UNNAMED-MODULE` 作为 *--add-opens* 选项中的目标模块。

```shell
java.lang.reflect.InaccessibleObjectException: Unable to make field private final java.util.ArrayList jdk.internal.loader.URLClassPath.loaders accessible: 
module java.base does not "opens jdk.internal.loader" to unnamed module @6442b0a6

$ java --add-opens=java.base/jdk.internal.loader=UNNAMED-MODULE example.Main
```

#### <a name="javalangnoclassdeffounderror"></a>java.lang.NoClassDefFoundError

*NoClassDefFoundError* 最有可能是由拆分包或引用删除的模块导致的。 

##### <a name="noclassdeffounderror-caused-by-split-packages"></a>拆分包导致的 NoClassDefFoundError

如果在多个库中找到某个包，则该包为拆分包。 拆分包问题的症状是，你知道某个类会在 class-path 上，但找不到该类。 

使用 module-path 时才会出现此问题。 Java 模块系统通过将包限制为一个命名的模块来优化类查找。  执行类查找时，运行时会优先处理 module-path 而不是 class-path。 如果包在某个模块和 class-path 之间拆分，则只使用该模块来执行类查找。 这可能导致 `NoClassDefFound` 错误。 

若要检查拆分包，一个简单的方法是将模块路径和类路径插入 [jdeps](https://docs.oracle.com/en/java/javase/11/tools/jdeps.html)，使用应用程序类文件的路径作为 &lt;path&gt;。 如果有拆分包，jdeps 会输出警告：`Warning: split package: <package-name> <module-path> <split-path>`。 

可以通过使用 `--patch-module <module-name>=<path>[,<path>]` 将拆分包添加到命名模块中来解决此问题。 

##### <a name="noclassdeffounderror-caused-by-using-java-ee-or-corba-modules"></a>使用 Java EE 或 CORBA 模块导致的 NoClassDefFoundError

如果应用程序在 Java 8 上运行但却引发 `java.lang.NoClassDefFoundError` 或 `java.lang.ClassNotFoundException`，则可能是应用程序在使用 Java EE 或 CORBA 模块中的包。 这些模块在 Java 9 弃用，[在 Java 11 中删除](https://openjdk.java.net/jeps/320)。 

若要解决此问题，请向项目添加运行时依赖项。

> [!div class="mx-tdBreakAll"]
> |删除的模块|受影响的包|建议的依赖项|
> |-|-|-|
> |Java API for XML Web Services (JAX-WS) |java.xml.ws |[JAX WS RI 运行时](https://mvnrepository.com/artifact/com.sun.xml.ws/jaxws-rt) |
> |用于 XML 绑定的 Java 体系结构 (JAXB) |java.xml.bind |[JAXB 运行时](https://mvnrepository.com/artifact/org.glassfish.jaxb/jaxb-runtime)|
> |JavaBeans Activation Framework (JAV) |java.activation |[JavaBeans (TM) Activation Framework](https://mvnrepository.com/artifact/javax.activation/activation) |
> |常见批注 |java.xml.ws.annotation |[Javax 批注 API](https://mvnrepository.com/artifact/javax.annotation/javax.annotation-api)|
> |通用对象请求代理体系结构 (CORBA) |java.corba | [GlassFish CORBA ORB](https://mvnrepository.com/artifact/org.glassfish.corba/glassfish-corba-orb) |
> |Java 事务 API (JTA) |java.transaction | [Java 事务 API](https://mvnrepository.com/artifact/javax.transaction/jta)|

#### <a name="-xbootclasspathp-is-no-longer-a-supported-option"></a>-Xbootclasspath/p 不再是受支持的选项

已删除对 `-Xbootclasspath/p` 的支持。 请改用 `--patch-module`。 *--patch-module* 选项在 [JEP 261](http://openjdk.java.net/jeps/261) 中介绍。 查找标为“修补模块内容”的部分。 可以将 *--patch-module* 与 *javac* 和 *java* 配合使用，以便重写或增强模块中的类。 

实际上， *--patch-module* 执行的操作是将修补模块插入模块系统的类查找。 模块系统会首先从修补模块获取类。 这与在 Java 8 中预挂起 bootclasspath 的效果相同。 

#### <a name="unsupportedclassversionerror"></a>UnsupportedClassVersionError

此异常表示你尝试在较低版本的 Java 上运行使用较高版本的 Java 编译的代码。 例如，在 Java 11 上运行其 jar 是使用 JDK 13 编译的程序。 

| Java 版本 | 类文件格式版本 |
|-|-|
| 8  | 52 |
| 9  | 53 |
| 10 | 54 |
| 11 | 55 |
| 12 | 56 |
| 13 | 57 |

## <a name="next-steps"></a>后续步骤

在 Java 11 上运行应用程序后，请考虑将库移出 class-path，然后再将其移入 module-path。 查找应用程序所依赖的库的已更新版本。 选择模块库（如果可用）。 尽可能使用 module-path，即使不打算在应用程序中使用模块。 与 class-path 相比，使用 module-path 可以获得更好的类加载性能。 
