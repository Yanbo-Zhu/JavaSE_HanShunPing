# 1 JVM

跨平台性靠 JVM 实现
JAM： 

- 虚拟的计算机，包含在jdk 中 
- 具有指令集并使用不同的储存区域。负责执行指令、管理数据、内存、寄存器
- 不同的操作系统 ， 用不同的jvm (不同的 jdk)

Die Java Virtuelle Maschine (JVM) ist:   der Interpreter, der das Byte-Programm ausführt.  Teil der Java-Laufzeitumgebung 
JVM 不是   der Debugger, der das Quellprogramm auf Fehler testet.   der Übersetzer, der das Quellprogramm kompiliert. 

# 2 JDK

1.JDK的全称（Java Development Kit Java开发工具包）
JDK = JRE + java的开发工具[java,javac,javadoc,javap等]
2.JDK是提供给Java开发人员使用的，其中包含了Java的开发工具，也包括了JRE。

# 3 JRE基本介绍

1.JRE（Java Runtime Environment Java运行环境）
JRE = JVM + Java的核心类库[类]
2.包括Java虚拟机（JVM Java Virtual Machine）和Java程序所需的核心类库等。如果想运行一个开发好的Java程序，计算机中只需安装JRE即可。

# 4 JDK、JRE和JVM的包含关系

1.JDK = JRE + 开发工具集（例如Javac,java编译工具等）
2.JRE = JVM + Java SE标准类库(java核心类库)
3.如果只想运行开发好的.class文件 只需要JRE
4.但是想要 开发 java, 就需要 java 开发工具, jdk中才有 
