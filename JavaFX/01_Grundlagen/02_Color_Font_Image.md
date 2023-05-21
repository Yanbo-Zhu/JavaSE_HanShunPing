
# 1 Color

![](image/Pasted%20image%2020230506001139.png)


## 1.1 例子

![](image/Pasted%20image%2020230506001322.png)

![](image/Pasted%20image%2020230506001450.png)


# 2 Font

![](image/Pasted%20image%2020230506001527.png)

setFont()

![](image/Pasted%20image%2020230506002000.png)

![](image/Pasted%20image%2020230506002058.png)


# 3 Image

![](image/Pasted%20image%2020230506002141.png)

ImageView class
setImage() method 
![](image/Pasted%20image%2020230506002342.png)


```java
            Image image = new Image (getClass().getResource("/personalphoto/"+p.getPhoto()).toString(), 200, 200, true, true);
            ImageView imageview = new ImageView(image);
            
	        image 自动调整大小  
	        https://stackoverflow.com/questions/12630296/resizing-images-to-fit-the-parent-node
            //imageview.setFitHeight(60);
            //imageview.setFitWidth(60);
            //imageview.fitWidthProperty().bind(root.widthProperty());
            //imageview.fitHeightProperty().bind(root.heightProperty());
```