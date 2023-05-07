
xml 文件中 定义 布局. 以此来 实现 UI 布局

# 1 例子
1
![](image/Pasted%20image%2020230506002804.png)

2 
模仿上面对布局的设计, 变相写入fxml文件中
![](image/Pasted%20image%2020230506003228.png)

FXMLoader.load()
![](image/Pasted%20image%2020230506003314.png)

因为使用了 fxml文件. 还要设置事件响应, 这样 点击button 后, label 的文职才能在窗口中上移
![](image/Pasted%20image%2020230506003507.png)

![](image/Pasted%20image%2020230506003705.png)

DemoController.java 中
![](image/Pasted%20image%2020230506003646.png)