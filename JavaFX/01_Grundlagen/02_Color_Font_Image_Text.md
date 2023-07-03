
# 1 Farben mit JavaFX

![](image/Pasted%20image%2020230506001139.png)

## 1.1 Model

Neben dem **RGB-Standard** gibt es noch weitere Farbmodelle für die Sprache Java.

Das **HSB-Modell** stellt eine Farbe durch die Parameter dar:

-   Farbton
-   Intensität
-   Helligkeit

Das **CMYK-Konzept** verwendet vier Farben:

-   Cyan
-   Magenta
-   Yellow
-   Black

![](image/Pasted%20image%2020230522190626.png)


## 1.2 Transparenz von Farben - Opacity
 1 is not transparent at all, 0.5 is 50% see-through, and 0 is completely transparent.
 
```java
Creates a new instance of color. 
public Color(double red,
             double green,
             double blue,
             double opacity)

Parameters:
-   red - red component ranging from 0 to 1
-   green - green component ranging from 0 to 1
-   blue - blue component ranging from 0 to 1
-   opacity - opacity ranging from 0 to 1

```

```java
public static Color rgb(int red,
                        int green,
                        int blue,
                        double opacity)

Creates an sRGB color with the specified RGB values in the range 0-255, and a given opacity.
Parameters:

    red - the red component, in the range 0-255
    green - the green component, in the range 0-255
    blue - the blue component, in the range 0-255
    opacity - the opacity component, in the range 0.0-1.0
```


Die folgenden Methoden der Klasse Color erlauben den Rot-, Grün- und Blau- und Transparent-Anteil jeder Farbe zu ermitteln. 
```java
 
public final double getRed()
//ermittelt Rot - Anteil im Bereich 0.0 <= r <= 1.0
public final double getGreen()
//ermittelt Grün - Anteil im Bereich 0.0 <= g <= 1.0
public final double getBlue()
//ermittelt Blau - Anteil im Bereich 0.0 <= b <= 1.0
public final double getOpacity()
//ermittelt Durchsichtigkeit - Anteil im Bereich 0.0 <= o <= 1.0

```


## 1.3 例子

```java
Circle circle = new Circle();
circle.setFill(Color.rgb(255, 0, 0))
circle.setFill(Color.rgb(255, 0, 0, 1))
```

![](image/Pasted%20image%2020230506001322.png)

![](image/Pasted%20image%2020230506001450.png)


# 2 Font

![](image/Pasted%20image%2020230506001527.png)

```java
yourrating.setFont(Font.font("Arial", 12));
```

setFont()

![](image/Pasted%20image%2020230506002000.png)

![](image/Pasted%20image%2020230506002058.png)


# 3 Image

![](image/Pasted%20image%2020230506002141.png)

ImageView class
setImage() method 
![](image/Pasted%20image%2020230506002342.png)

```java
Image image = new Image (getClass().
getResource("/resources/ibisMitteBerlin.jpg").toString());
ImageView imageview = new ImageView(image);
//puts imageview column 0 row 0
root.add(imageview, 0, 0);
```



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



# 4 javafx.scene.text.Text 

Da die Klasse javafx.scene.text.Text eine Unterklasse von javafx.scene.shape.Shape ist, sind Farbeffekte mit Text-Objekten möglich.


weil die Klasse Text eine Unterklasse (subclass) von Shape ist und somit die Methode setFill erbt.

Im Konstruktor` Text(double x, double y, String text)` bestimmt der Parameter x die Spalte, ab der die Textausgabe erfolgt, während y die Grundlinie für die Textausgabe festlegt.


## 4.1 

```java
public void start(Stage primaryStage) {
	try {
		Group root = new Group();
		
		// one triangle
		Polygon poly1 = new Polygon(W/2, SPACE, W/2 + W/4, H - 2*SPACE,
				W/2 - W/4 , H - 2*SPACE);
		
		// the second triangle
		Polygon poly2 = new Polygon(W/2 - W/4, 2*SPACE, W/2 + W/4,  2*SPACE,
				W/2 , H - SPACE);
		
		//star as union of 2 triangles
		Shape union = Shape.union(poly1, poly2);
		union.setFill(gradient2());
		union.setStroke(Color.BLACK);
		root.getChildren().addAll(union);
		
		Text text = new Text(0, H-SPACE, "My first star !");
		text.setStroke(Color.BLACK);
		text.setFont(new Font(50));
		text.setFill(gradient6());
		root.getChildren().addAll(text);

		Scene scene = new Scene(root, W, H);
		primaryStage.setScene(scene);
		primaryStage.setTitle("GeometrischeFiguren03");
		primaryStage.show();
	} catch(Exception e) {
		e.printStackTrace();
	}
}
```


Schauen Sie sich den Sachverhalt mit folgender Anweisung an.
```java
Text text = new Text(5, 50, "Na, Super!");
root.getChildren().addAll(text);

Text text = new Text(0, H-SPACE, "My first star !");
text.setStroke(Color.BLACK);
text.setFont(new Font(50));
text.setFill(gradient6());
root.getChildren().addAll(text);

```

Nach dem Hinzufügen in ein Group-Objekt wird der Text wie in der Abbildung aussehen.

![](image/Pasted%20image%2020230522192151.png)


## 4.2 例子
```java
Text hotelIbisMitte = new Text("ibis Styles Hotel\n "
   + "Berlin Mitte, Brunnenstrasse 1-2\n" +"10119 Berlin");
//puts hotelIbisMitte column 1 row 0
root.add(hotelIbisMitte, 1, 0);

```

