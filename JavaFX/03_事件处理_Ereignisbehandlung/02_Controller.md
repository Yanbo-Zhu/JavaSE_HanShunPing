
# 1 Controller 和 .fxml 中组件的关系 

在 .fxml 文件中 定义 窗口中的组件
然后在 Controller中 定义 这些 在 .fxml 文件中 已经定义好的窗口中的组件 对应的 方法
![](image/Pasted%20image%2020230507171117.png)

比如已经在 .fxml 中 定义好了一个 Circle fx:i id="ci"
![](image/Pasted%20image%2020230507211158.png)

然后要在 Controller.java 中  写 对这个Circle 空间的方法

![](image/Pasted%20image%2020230507211312.png)


# 2 Controller里面 intialize的方法

在 .fxml 文件中 定义 窗口中的组件
![](image/Pasted%20image%2020230507171117.png)

然后在 Controller中 定义 这些 在 .fxml 文件中 已经定义好的窗口中的组件 对应的 方法
<mark>如果作为一个事件, 就在事件触发后才会被填充. 而 intialize的方法 就是组件 在每次被调用前, 在加载ui空间的时候, 启动的时候 就会被执行的方法 不需要事件的触发</mark>

在 intialize() 方法中 是无法触发 scene 相关的属性的. 是因为需要先加载控件, 执行 intialize(), 之后这个控件才知道自己在那个scene 里面

![](image/Pasted%20image%2020230507171625.png)


## 2.1 例子
1在 .fxml 文件中 定义 窗口中的组件
为 tableview 组件 
![](image/Pasted%20image%2020230507171724.png)

2 Controller.java
![](image/Pasted%20image%2020230507172228.png)

main.java
![](image/Pasted%20image%2020230507172213.png)

Person.java
![](image/Pasted%20image%2020230507172456.png)

3
可以看到 tableview 这个组件 在启动的时候 就已经填充进去数据了 
![](image/Pasted%20image%2020230507171927.png)


# 3 Application 里面操作 Controller

main.java 中的主线程, 调用 Controller.java 中定义好的方法. 进而操作 .fxml 中定义的控件 

main.java: 
将 圆的中心点 和窗口的中心点 相一致
这样 随着窗口扩大缩小, 圆的中心点 和窗口的中心点 始终相一致

![](image/Pasted%20image%2020230507211737.png)

![](image/Pasted%20image%2020230507211615.png)

Controller.java

![](image/Pasted%20image%2020230507211629.png)

hello.fxml
![](image/Pasted%20image%2020230507211650.png)