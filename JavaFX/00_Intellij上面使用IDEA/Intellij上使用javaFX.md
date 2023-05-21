
https://www.jianshu.com/p/9048022cce1a  (这个写得好)
https://yangfangs.github.io/2017/07/28/JavaFx-SceneBuilder/
https://www.w3cschool.cn/intellij_idea_doc/intellij_idea_doc-r2jo2y7r.html
https://edencoding.com/runtime-components-error/
https://openjfx.cn/openjfx-docs/#install-javafx


# 1 配置步骤

https://www.jianshu.com/p/9048022cce1a  (这个写得好)
https://openjfx.cn/openjfx-docs/#install-javafx

## 1.1 下载 javafx-sdk
java11 后默认不自带FavaFX , 需要自己下载: https://gluonhq.com/products/javafx/
我放在了d:\openjavafx_sdk\openjfx-20.0.1_windows-x64_bin-sdk\javafx-sdk-20.0.1\lib\


## 1.2 Create a library
![](image/Pasted%20image%2020230519223659.png)

## 1.3 Add VM options
还需要配置模拟机 
配置完jdk和sdk后，Idea是能识别JavaFX项目了，编译也不报错，可是运行时会提示找不到运行时环境，这个时候需要配置Configurations，添加虚拟机选项，这个是这种方式的关键（官网上虽然也是这么介绍搭建的，但是漏了一些细节）

1
先不急着配这个，先去配置一个可以在将来的项目中使用的全局变量，转至 Preferences (File -> Settings) -> Appearance & Behavior -> Path Variables，并将变量的名称定义为PATH_TO_FX，然后浏览至JavaFX SDK的lib文件夹以设置其值，然后单击Apply，如下图：
![](https://upload-images.jianshu.io/upload_images/22122088-83b975d055d67872.png?imageMogr2/auto-orient/strip|imageView2/2/w/692/format/webp)

  

2 
click on Run -> Edit Configurations... and add these VM options:

点击Edit  Configurations后打开的界面默认是没有VM options这个选项的，如下图：
![](https://upload-images.jianshu.io/upload_images/22122088-a094943d3bab148d.png?imageMogr2/auto-orient/strip|imageView2/2/w/692/format/webp)

然后再回到刚才配置VM options的地方，点击Edit  Configurations，刚才说了默认是没有VM options这个选项的，而这个是这种方式的关键。首先你需要点击右侧的 Modify options，在弹出的下拉菜单中找到Add VM options 如下图：
![](https://upload-images.jianshu.io/upload_images/22122088-53e62c928c314074.png?imageMogr2/auto-orient/strip|imageView2/2/w/691/format/webp)

添加完之后，这时会发现比原先面板多出来1个VM options的输入框，可以在里面输入VM options的配置参数了：
在VM options输入框中输入参数（官方推荐）：--module-path ${PATH_TO_FX} --add-modules javafx.controls,javafx.fxml
![](https://upload-images.jianshu.io/upload_images/22122088-5701358f595529b5.png?imageMogr2/auto-orient/strip|imageView2/2/w/692/format/webp)
  
  
然后点击OK，这时候再去运行Main程序就不会再报错了，并且程序运行完毕，会弹出一个框，类似于swing那种，当然你可以自定义里面的内容，实现自己想要展示的效果：

Alternatively, you can define a global variable that can be used in future projects. Go to Preferences (File -> Settings) -> Appearance & Behavior -> Path Variables, and define the name of the variable as PATH_TO_FX, and browse to the lib folder of the JavaFX SDK to set its value, and click apply. [![Path Variable](https://openjfx.cn/openjfx-docs/images/ide/intellij/ide/idea07.png)](https://openjfx.cn/openjfx-docs/images/ide/intellij/ide/idea07.png)

Then you can refer to this global variable when setting the VM options as:

```
--module-path ${PATH_TO_FX} --add-modules javafx.controls,javafx.fxml
```

[![Path Variable in VM options](https://openjfx.cn/openjfx-docs/images/ide/intellij/ide/idea08.png)](https://openjfx.cn/openjfx-docs/images/ide/intellij/ide/idea08.png)


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



