
https://www.jianshu.com/p/9048022cce1a
https://yangfangs.github.io/2017/07/28/JavaFx-SceneBuilder/
https://www.w3cschool.cn/intellij_idea_doc/intellij_idea_doc-r2jo2y7r.html

# 1 配置步骤

java11 后默认不自带FavaFX , 需要自己下载
还需要配置模拟机 

# 2 项目目录结构

项目默认由3部分组成，
- 首先是程序的启动类Main,用于加载布局文件，启动窗口，
- sample.fxml用于存放程序中用到的布局控件，
- Controller控制器，用于监听控件各种点击事件的，可自定义

![](image/Pasted%20image%2020230505185435.png)
  


## 2.1 用 maven 配置的时候

目录结构如下图所示，主要分为3大部分，分别是存放程序源代码的java目录，存放程序布局资源等的resources目录，和Maven依赖文件pom.xml。
![](image/Pasted%20image%2020230505185640.png)

Java目录中存放称着启动类App和自定义的控制器Controller，该程序主要实现的就是在两个界面中来回跳转，响应了控件的点击事件，这里重点讲下module-info.java这个文件，这个java文件跟我们常看到的java类文件不同，你看他的文件名和类名并不相同，这个文件是用来做模块化开发的，你可以理解为是1个配置文件，用于将本程序用的到的所有modul，java程序，布局文件组合起来。由于FXML使用反射来访问模块中的控制器，因此已将其打开javafx.fxml。如下图所示：
![](image/Pasted%20image%2020230505185655.png)



