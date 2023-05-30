
# 1 Linien und Formen in einem Koordinatensystem

## 1.1 Koordinatensystem
Wie in der Mathematik wird die **x - Achse** horizontal von links nach rechts aufsteigend festgelegt, dagegen verläuft die **y - Achse** senkrecht von oben nach unten. Alle Koordinaten sind **positive ganze Zahlen**.

单位 为 pixel

![](image/Pasted%20image%2020230522185902.png)

![](image/Pasted%20image%2020230529105547.png)



# 2 javafx.scene.text.Text 

weil die Klasse Text eine Unterklasse (subclass) von Shape ist und somit die Methode setFill erbt.

Da die Klasse javafx.scene.text.Text eine Unterklasse von javafx.scene.shape.Shape ist, sind Farbeffekte mit Text-Objekten möglich.

Im Konstruktor Text(double x, double y, String text) bestimmt der Parameter x die Spalte, ab der die Textausgabe erfolgt, während y die Grundlinie für die Textausgabe festlegt.

Schauen Sie sich den Sachverhalt mit folgender Anweisung an.

```
Text text = new Text(5, 50, "Na, Super!");
```

  
Nach dem Hinzufügen in ein Group-Objekt wird der Text wie in der Abbildung aussehen.

![](image/Pasted%20image%2020230522192151.png)



# 3 Farben mit JavaFX

## 3.1 Model

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


## 3.2 Transparenz von Farben - Opacity

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


# 4 Superklasse javafx.scene.shape.Shape
Die Klasse Shape ist abstrakt. Sie definiert wichtige Methoden für geometrische Formen, beispielsweise die Farbe und die Stärke des Rahmens oder die Farbe den Innenraums.

![](image/Pasted%20image%2020230522190251.png)

In der Klasse Shape finden sich weitere hilfreiche Methoden, um beispielsweise den Durchschnitt (Methode intersect) oder die Vereinigung (Methode union) zweier Shape-Objekte zu bilden.  

# 5 Linie

javafx.scene.shape.Line
dass Line eine Unterklasse (subclass) von javafx.scene.shape.Shape ist.
Line-Objekt: Der Konstruktor Line(double startX, double startY, double endX, double endY)


```java
Line line = new Line(startx, starty, endx, endy);
Line line = new Line(0, 0, width, height);
Line line = new Line(width, 0, 0, height);
Line line = new Line(2, 3, 7, 3);
Line line = new Line(2, 3, 7, 7);
line.setStrokeWidth(5);
line.setStroke(Color.GREEN);
line.setStroke(Color.rgb(0, 150, 0, 0.5));
```

![](image/Pasted%20image%2020230529205305.png)



## 5.1 例子 GeometrischeFiguren01



![](image/Pasted%20image%2020230522190327.png)

Als Wurzelelement wird ein Objekt vom Typ import javafx.scene.Group; gewählt. Dieser Typ erlaubt, die Koordinaten der Shape-Objekte genau zu bestimmen.
Die Benutzung der Klasse javafx.scene.Group als Wurzel des scene graph erlaubt eine genau Positionierung dieser geometrischen Figuren in der Szene.


Zuerst wird eine Diagonale im Fenster erzeugt; dies passiert mit der Anweisung: 

```
Line line1 = new Line(0, 0, WIDTH, HEIGHT);
```

  
Dann wird eine Parallele über diese Diagonale erzeugt; dies passiert mit der Anweisung:  

```
Line line2 = new Line(INCREMENT, 0, WIDTH, HEIGHT-INCREMENT);
```

  
Die Methoden setStroke und setStrokeWidth der Klasse Shape werden benutzt, um jeweils Farbe (color) und Dicke der Linien zu verändern. 
Schließlich werden waagerechte Linien erzeugt, die immer länger werden, und immer tiefer im Fenster erscheinen. Damit sie sichtbar bleiben, wird in der Bedingung der while-Schleife (loop) geprüft, dass der Endpunkt der Linien weder Höhe noch Breite des Fensters überschreitet; dies passiert mit dem booleschen Ausdruck:

```
(startY < HEIGHT && endX < WIDTH)
```


```java
package application;

/**
 * This class demonstrates the class Line.
 * @author agathe merceron
 */

import java.util.Random;

import javafx.application.Application;
import javafx.scene.Group;
import javafx.scene.Scene;
import javafx.scene.paint.Color;
import javafx.scene.shape.Line;
import javafx.stage.Stage;

public class GeometrischeFiguren01 extends Application {
    //dimension of the scene
    private static final double WIDTH = 700;
    private static final double HEIGHT = 600;
    
    // fixes where the first horizontal line begins and ends
    // subsequent lines will be drawn INCREMENT lower and will be
    // INCREMENT longer
    private static final double STARTX = 10;
    private static final double ENDX = 20;
    private static final double STARTY = 10;
    private static final double INCREMENT = 40;
    @Override
    public void start(Stage primaryStage) {
        try {
            // Group as root for geometric applications
            Group root = new Group();
            
            // diagonal from the upper left corner to the
            // bottom right corner
            Line line1 = new Line(0, 0, WIDTH, HEIGHT);
            line1.setStrokeWidth(2); //makes the line twice as wide
            
            // draws a parallel to the diagonal from upper left
            // to bottom right INCREMENT pixels above the diagonal
            Line line2 = new Line(INCREMENT, 0, WIDTH, HEIGHT-INCREMENT);
            // changes its color 
            line2.setStroke(Color.AQUAMARINE);
            root.getChildren().addAll(line1, line2);

            
            Random r = new Random();
            
            double startY = STARTY;
            double endX = ENDX;
            // draws a series of horizontal lines that fit in the window
            while (startY < HEIGHT && endX < WIDTH) {
                Line line = new Line(STARTX, startY, endX, startY);
                line.setStroke(Color.RED);
                // how wide a line is, is randomly chosen
                // from 1 to 8 (because of +1)
                line.setStrokeWidth(r.nextInt(8)+1);
                root.getChildren().addAll(line);
                // the next line will grow longer
                startY += INCREMENT;
                // the next line will be below this one
                endX += INCREMENT;
            }
            
            Scene scene = new Scene(root, WIDTH, HEIGHT);
            primaryStage.setScene(scene);
            primaryStage.setTitle("GeometrischeFiguren01");
            primaryStage.show();
        } catch(Exception e) {
            e.printStackTrace();
        }
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```


# 6 Circle

```java
Circle circle = new Circle(centerx, centery, rad);
Circle circle = new Circle(width/2, heigth/2, 10)
circle.setFill(null);
circle.setFill(Color.BLUE);
circle.setStrokeWidth(staerke);
circle.setStroke(Color.rgb(20, 25, 245));
circle.setStrokeType(StrokeType.OUTSIDE);
circle.setStrokeType(StrokeType.INSIDE);
circle.getTransforms().add(new Translate(2, 3));
circle.getTransforms().add(new Scale(0.5, 0.5));
circle.getTransforms().add(new Scale(1.0, 0.5));
```

![](image/Pasted%20image%2020230529205340.png)



# 7 Die Klasse Rectangle 

 	
Die Klasse Rectangle erlaubt rechteckige Formen zu zeichnen. Der Konstruktor Rectangle(double x, double y, double width, double height) erzeugt ein Rectangle-Objekt mit der Länge (width) und Höhe (height), dessen obere linke Ecke die Koordinaten (x, y) hat.

Die folgende Grafik zeigt ein Rechteck (rectangle) mit den Koordinaten (40 | 60), einer Breite von 200 Pixel und einer Höhe von 40 Pixel. Dieses kann mit der folgenden Anweisung erzeugt werden: new Rectangle(40, 60, 200, 40);


## 7.1 Method (setArcHeight/Width, setStroke, setStrokeWidth/Height, setFill  )
1 Methoden setArcHeight und setArcWidth der Klasse Rectangle 
边沿变得原话

2  Methoden setStroke und setStrokeWidth
Wie Line ist Rectangle eine Unterklasse (subclass) von Shape. Damit können die schon gesehenen Methoden setStroke und setStrokeWidth benutzt werden, um den Rand eines Rectangle-Objektes anzupassen

3  Die Methode setFill 
Die Methode setFill erlaubt, die Farbe des inneren Bereiches eines Shape-Objektes zu bestimmen. Diese drei Methoden werden mit der Variable rec1 benutzt.


## 7.2 例子1 

```java
Rectangle rect = new Rectangle(
startx, starty, width, height);
Rectangle rect = new Rectangle(
width/3, height/3, 250, 100);
rect.setFill(new Color(1.0, 0.0, 0.0, 1.0));
rect.setArcHeight(30);
rect.setArcWidth(30);
rect.setStrokeWidth(5);
rect.setStroke(Color.rgb(30, 70, 125));
rect.setStrokeType(StrokeType.OUTSIDE);
rect.setStrokeType(StrokeType.INSIDE);
rect.getTransforms().add(
new Translate(2, 8));
rect.getTransforms().add(
new Scale(0.5, 0.5));
rect.getTransforms().add(
new Rotate(45, 2, 8));
```

![](image/Pasted%20image%2020230529205919.png)


```java
Rectangle rect = new Rectangle(
startx, starty, width, height);
Rectangle rect = new Rectangle(
width/3, height/3, 250, 100);
rect.setFill(new Color(1.0, 0.0, 0.0, 1.0));
Text txt = new Text(
rect.getX(), rect.getY(), "(" +
rect.getX() + "/" + rect.getY() + ")");
txt.setFont(new Font("Arial", 30));
Group root = new Group();
root.getChildren().addAll(rect, txt);
```

![](image/Pasted%20image%2020230529205943.png)

## 7.3 例子 2

![](image/Pasted%20image%2020230522191238.png)


```java
package application;

/**
 * this class demonstrates the class Rectangle, LinearGradient,
 * RadialGradient and transformations on a Group-Object.
 * @author agathe merceron
 */


import javafx.application.Application;
import javafx.scene.Group;
import javafx.scene.Scene;
import javafx.scene.paint.Color;
import javafx.scene.paint.CycleMethod;
import javafx.scene.paint.LinearGradient;
import javafx.scene.paint.RadialGradient;
import javafx.scene.paint.Stop;
import javafx.scene.shape.Rectangle;
import javafx.stage.Stage;

public class GeometrischeFiguren02 extends Application {
    //dimension of the scene
    private static final double WIDTH = 600;
    private static final double HEIGHT = 400;
    
    // Space to the border of the window
    private static final double SPACE = 20;
    @Override
    public void start(Stage primaryStage) {
        try {
            // Group as root for geometric applications
            Group root = new Group();
    
            // draws a rectangle 
            Rectangle rec1 = new Rectangle(SPACE, 0 , WIDTH/3, (HEIGHT-SPACE)/3);
            // makes the border blue
            rec1.setStroke(Color.BLUE);
            // changes the color of the rectangle itself
            rec1.setFill(Color.AQUAMARINE);
            rec1.setStrokeWidth(2); //makes the border twice as wide

            root.getChildren().addAll(rec1);
            
            // a second rectangle partially overlapping the first one
            Rectangle rec2 = new Rectangle(SPACE*5, 0, WIDTH/3, (HEIGHT-SPACE)/3);
            // makes it slightly transparent
            rec2.setFill(Color.rgb(255, 0, 255, 0.25));
            // round corners
            rec2.setArcHeight(30);
            rec2.setArcWidth(30);
            root.getChildren().addAll(rec2);
            
            // a third rectangle below the first one
            Rectangle rec3 = new Rectangle(SPACE, WIDTH/3, WIDTH/3, (HEIGHT-SPACE)/3);
            // makes a smooth change of color
            rec3.setFill(gradient1());

            root.getChildren().addAll(rec3);

//          root.setRotate(30);
//          root.setTranslateX(WIDTH/3);
//          root.setTranslateY(WIDTH/4);
            Scene scene = new Scene(root, WIDTH, HEIGHT);
            primaryStage.setScene(scene);
            primaryStage.setTitle("GeometrischeFiguren02");
            primaryStage.show();
        } catch(Exception e) {
            e.printStackTrace();
        }
    }
    
    //the idea of all the gradient-Methods below comes from an example
    //in "JavaFX for dummies" by Doug Lawe, Hoboken, NJ, Wiley,
    
    /**
     * @return a linear gradient that changes the color from green to red
     * from top to bottom
     */
    private LinearGradient gradient1() {
        return new LinearGradient (0, 0, 0, 1, true, CycleMethod.NO_CYCLE,
                new Stop(0.0, Color.GREEN), new Stop(1.0, Color.RED));
    }
    
    
    /**
     * @return a linear gradient that changes the color from green to red
     * from left to right
     */
    private LinearGradient gradient2() {
        return new LinearGradient (0, 0, 1, 0, true, CycleMethod.NO_CYCLE,
                new Stop(0.0, Color.GREEN), new Stop(1.0, Color.RED));
    }
    
    /**
     * @return a linear gradient that changes the color from green to red
     * from top to bottom in cycles of 20% of the height
     */
    private LinearGradient gradient3() {
        return new LinearGradient (0, 0, 0, 0.2, true, CycleMethod.REPEAT,
                new Stop(0.0, Color.GREEN), new Stop(1.0, Color.RED));
    }
    
    /**
     * @return a linear gradient that changes the color from green to red
     * from top to 20% of the height; rest is red.
     */ 
    private LinearGradient gradient3a() {
        return new LinearGradient (0, 0, 0, 0.2, true, CycleMethod.NO_CYCLE,
                new Stop(0.0, Color.GREEN), new Stop(1.0, Color.RED));
    }
    
    /**
     * @return a linear gradient that changes the color from green to red
     * from top to 20% of the height; rest in red.
     */ 
    private LinearGradient gradient3b() {
        return new LinearGradient (0, 0, 0, 1, true, CycleMethod.NO_CYCLE,
                new Stop(0.0, Color.GREEN), new Stop(0.2, Color.RED));
    }
    
    /**
     * @return a linear gradient that changes the color from green to red
     * along the diagonal
     */ 
    private LinearGradient gradient4() {
        return new LinearGradient (0, 1, 1, 0, true, CycleMethod.NO_CYCLE,
                new Stop(0.0, Color.GREEN), new Stop(1.0, Color.RED));
    }
    
    /**
     * @return a linear gradient that changes the color from green to white
     * to red from top to bottom. The change to white is at 75% of the height
     */
    private LinearGradient gradient5() {
        return new LinearGradient (0, 0, 0, 1, true, CycleMethod.NO_CYCLE,
                new Stop(0.0, Color.GREEN), new Stop(0.75, Color.WHITE), new Stop(1.0, Color.RED));
    }
    
    /**
     * @return a radial gradient that changes the color from green to white
     * to red from top to bottom. The change to white is at 75% of the radius
     */
    private RadialGradient gradient6() {
        return new RadialGradient (0, 0, 0.5, 0.5, 0.5, true, CycleMethod.NO_CYCLE,
                new Stop(0.0, Color.GREEN), new Stop(0.75, Color.WHITE), new Stop(1.0, Color.RED));
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```



# 8 Die Klassen LinearGradient, RadialGradient

javafx.scene.paint.LinearGradient/RadialGradient

Es wird nicht mit einer einzigen Farbe gefüllt, sondern mit Farben, die progressiv ineinander übergehen - einem Verlauf (gradient). JavaFX stellt dafür zwei Klassen zur Verfügung: javafx.scene.paint.LinearGradient; und javafx.scene.paint.RadialGradient.

Mit der Klasse LinearGradient erfolgt der progressive Wechsel einer Farbe entlang einer Linie, 
mit der Klasse RadialGradient entlang eines Kreises. 

### 8.1.1 LinearGradient

Der Konstruktor der Klasse LinearGradient sieht so aus:

```
public LinearGradient(double startX,
                      double startY,
                      double endX,
                      double endY,
                      boolean proportional,
                      CycleMethod cycleMethod,
                      Stop... stops)
//Creates a new instance of LinearGradient.
```

Parameters:
-   startX - the X coordinate of the gradient axis start point
-   startY - the Y coordinate of the gradient axis start point
-   endX - the X coordinate of the gradient axis end point
-   endY - the Y coordinate of the gradient axis end point
-   proportional - whether the coordinates are proportional to the shape which this gradient fills
-   cycleMethod - cycle method applied to the gradient
-   stops - the gradient's color specification

  
Diesen Konstruktor versteht man am besten durch Experimentieren. Aus diesem Grund gibt es mehrere gradient-Methoden in der Klasse GeometrischeFiguren02. Testen Sie nacheinander einige Methoden und schauen Sie, was diese bewirken. Versuchen Sie sich zu erklären, warum dies so ist.

Wir betrachten hier die erste gradient-Methode:

```
private LinearGradient gradient1() {
       return new LinearGradient (0, 0, 0, 1, true, CycleMethod.NO_CYCLE,
              new Stop(0.0, Color.GREEN), new Stop(1.0, Color.RED));
}
```

Die Form, welche das LinearGradient-Objekt füllen soll, kann beliebig groß sein. Die richtige Größe wird abstrahiert. Für das LinearGradient-Objekt wird sie auf dem Intervall [0, 1] skaliert, weil wir true für den Parameter proportional gewählt haben - was sich (fast) immer empfiehlt. 
Somit bedeutet 0, 0 die Ecke links oben und 0, 1 die Ecke links unten: die Linie für die Veränderung der Farbe ist von oben nach unten. 
Die Stop-Elemente bestimmen, wo eine Farbe anfängt. Hier wird die grüne Farbe oben anfangen und die rote Farbe unten. Dazwischen gehen diese zwei Farben progressiv ineinander; mit CycleMethod.NO_CYCLE gibt es keine Wiederholung.

Was passiert mit der Änderung: 0, 0, 1, 0 statt 0, 0, 0, 1?

Die Linie geht jetzt von der oberen Ecke links zur oberen Ecke rechts. In diesem Fall wird die grüne Farbe links anfangen und die rote Farbe rechts, siehe gradient2().

# 9 geometrische Transformationen

Mit Hilfe der Methoden setRotate, setTranslateX, setTranslateY der Klasse Node ist es möglich, das Node-Elemente samt ihrer Kinder zu drehen und zu versetzen.   
Das Paket javafx.scene.shape hat noch andere fertige geometrische Figuren wie die Klassen Circle, Ellipse, Sphere usw.


# 10 javafx.scene.shape.Polygon 多边形 


Die Klassen Polyline und Polygone (geschlossene Polyline-Objekte) erlauben es, eigene geometrische Figuren zu erstellen. 
Polygone sind Vielecke oder Streckenzüge.  Der/Die ProgrammiererIn übergibt als Eingabeparameter eine Menge von Punkten, deren Position im Pixel-Koordinatensystem durch die Koordinaten x, y vorgegeben ist.
Die Anweisung new Polygon(40, 40, 40, 80, 80, 80); erzeugt ein rechtwinkliges Dreieck.


## 10.1 例子
![](image/Pasted%20image%2020230522192208.png)

```java
package application;

/**
 * this class demonstrates how to create a star from two triangles
 * and illustrates further the class Text.
 * @author agathe merceron
 */


import javafx.application.Application;
import javafx.scene.Group;
import javafx.scene.Scene;
import javafx.scene.paint.Color;
import javafx.scene.paint.CycleMethod;
import javafx.scene.paint.LinearGradient;
import javafx.scene.paint.RadialGradient;
import javafx.scene.paint.Stop;
import javafx.scene.shape.Polygon;
import javafx.scene.shape.Shape;
import javafx.scene.text.Font;
import javafx.scene.text.Text;
import javafx.stage.Stage;

public class GeometrischeFiguren03 extends Application {
    //dimension of the scene
    private static final double W = 300;
    private static final double H = 300;
    
    //space from the border of the window
    private static final double SPACE = H/6;
    @Override
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
    
    
    /**
     * @return a linear gradient that changes the color from yellow to white
     * from left to right
     */
    private LinearGradient gradient2() {
        return new LinearGradient (0, 0, 1, 0, true, CycleMethod.NO_CYCLE,
                new Stop(0.0, Color.YELLOW), new Stop(1.0, Color.WHITE));
    }
    
    
    /**
     * @return a linear gradient that changes the color from green to white
     * to red from top to bottom. The change to white is at 75% of the height
     */
    private RadialGradient gradient6() {
        return new RadialGradient (0, 0, 0.5, 0.5, 0.5, true, CycleMethod.NO_CYCLE,
                new Stop(0.0, Color.GREEN), new Stop(0.75, Color.WHITE), new Stop(1.0, Color.RED));
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```




## 10.2 Geometrische Figuren mit den Klassen Polyline und Polygone

Das Programm GeometrischeFiguren04 zeigt eine Benutzung der Methode makeStar in einer anderen Anwendung.

![](image/Pasted%20image%2020230522192727.png)

```java
package application;

/**
 * this class illustrates a possibility to combine
 *  Group and GridPane, and reuse already programmed shapes.
 * The class OurShapes is used to show a scene with 5 stars.
 * @author agathe merceron
 */

import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.scene.Group;
import javafx.scene.Scene;
import javafx.scene.layout.GridPane;
import javafx.scene.paint.Color;
import javafx.stage.Stage;

public class GeometrischeFiguren04 extends Application {
	
	private static final double W = 300;
	private static final double H = 300;
	
	@Override
	public void start(Stage primaryStage) {
		try {
			GridPane root = new GridPane();
			root.setPadding(new Insets(25, 25, 25, 25));
			// it is important to put some space between the columns
			// as a Group is NOT a Region-Object;
			// the method padding cannot be called with a Group-Object
			root.setHgap(10);
			root.setVgap(10);
			
			//adding 5 stars of different colors
			Color[] colors = new Color[]{Color.BLUE, Color.CORNSILK, Color.GREENYELLOW,
					Color.PINK, Color.YELLOW};
			for (int i=0; i<5; i++) {
				Group star1 =  OurShapes.makeStar(60, 60, colors[i]);
				root.add(star1, i, 0);
			}
			Scene scene = new Scene(root, W, H);
			primaryStage.setScene(scene);
			primaryStage.setTitle("GeometrischeFiguren04");
			primaryStage.show();
		} catch (Exception e) {
		e.printStackTrace();
		}
	}
	
	public static void main(String[] args) {
		launch(args);
	}

}
```

---

Methode makeStar 
```java
package application;

/**
 * this class contains methods to generate ready-made shapes,
 * that can be used in other programs.
 * Dear students: any kind of shape is allowed!
 * Program a shape and add it to this class. Thank you!
 * @author Agathe Merceron
 */

import javafx.scene.Group;
import javafx.scene.paint.Color;
import javafx.scene.paint.Paint;
import javafx.scene.shape.Polygon;
import javafx.scene.shape.Shape;

public class OurShapes {
    
    /**
     * this method generates a star that fits in a frame with the given width
     * and the given height, and filled with the given color
     * @param w, width of the frame for the star
     * @param h, height of the frame for the star
     * @param c, color or gradient to fill the star
     * @return a frame with a star in it
     */
    
    public static Group makeStar(double w, double h, Paint c) {
        // for more flexibility a color for the edge of the star
        // could be added as a parameter. Presently, the edge is black.
        double space = h/6;
        Group root = new Group();
        
        // one triangle
        Polygon poly1 = new Polygon(w/2, space, w/2 + w/4, h - 2*space,
                w/2 - w/4 , h - 2*space);
        
        // the second triangle
        Polygon poly2 = new Polygon(w/2 - w/4, 2*space, w/2 + w/4,  2*space,
                w/2 , h - space);
        
        //star as union of 2 triangles
        Shape union = Shape.union(poly1, poly2);
        union.setFill(c);
        union.setStroke(Color.BLACK);
        
        root.getChildren().addAll(union);
        return root;
        
    }

}
```



# 11 Model View: Beispiel 


Im Studienmodul Grundlagen der Programmierung I  haben Sie die Klassen Rectangle, Circle usw. als Unterklassen (subclass) der Klasse Shape programmiert. Diese Klassen – um ein paar Getters ergänzt – beschreiben die mathematischen Eigenschaften der Figuren, wie Ursprung, Länge, Fläche usw. und bilden jetzt unser Modell – diese Shape-Objekte werden nun grafisch dargestellt.

Wir nehmen hier eine Umbenennung der Klassen vor, um eine lange Benennung in der Klasse im Paket (package)  view zu vermeiden. In der Tat gibt es sowohl die Klasse Rectangle im Paket model, um Rectangle-Objekte mathematisch zu beschreiben – als auch im Paket javafx.scene.shape, um Rectangle-Objekte grafisch darzustellen. 

In der Klasse des Pakets view sollte immer z. B. model.Rectangle geschrieben werden, wenn die Klasse für die mathematischen Objekte gemeint ist. Um diese lange Schreibweise zu vermeiden, werden einfach alle Klassen des Paketes model das Präfix M haben.


----
 	
Das folgende Klassendiagramm zeigt, wie die Klassen strukturiert sind und wie sie zusammenhängen. Die Richtung der Pfeile zeigt an, welche Klasse durch die andere Klasse benutzt wird. Klassen aus Paket view benutzen Klassen aus dem Paket model, aber nicht umgekehrt. Die graphische Darstellung kann verändert werden, die Logik der Anwendung bleibt bestehen.

Die Anwendung ist völlig dynamisch: es kann eine beliebige Anzahl von Shape-Objekten dargestellt werden; die Reihenfolge der Figuren, Rechteck oder Kreis, ist auch beliebig.

![](image/Pasted%20image%2020230522193637.png)


---

ShowShapesA.java

![](image/Pasted%20image%2020230522193540.png)
 	
Die Klasse ShowShapesA zeigt eine graphische Darstellung der Objekte, welche mit der Methode getDefaultShapes erzeugt wurden. Ein MRectangle-Objekt aus dem Paket model wird mit einem Rectangle-Objekt aus dem Paket javafx.scene.shape gezeichnet. Ähnliches gilt für ein MCircle-Objekt.

Die Klasse ShowShapesA zeigt eine graphische Darstellung der Objekte, welche mit der Methode getDefaultShapes erzeugt wurden. Ein MRectangle-Objekt aus dem Paket model wird mit einem Rectangle-Objekt aus dem Paket javafx.scene.shape gezeichnet. Ähnliches gilt für ein MCircle-Objekt.


ShowShapesA.java
```java
package view;

/**
 * A class to represent graphically an Array of Shape-objects.
 * A Shape Object can be a rectangle or a circle.
 * The shapes appear with their real dimensions and positions,
 * and their area (number rounded) displayed at their origin.
 * @author agathe merceron
 */


import model.MAllShapes;
import model.MCircle;
import model.MRectangle;
import model.MShape;
import javafx.application.Application;
import javafx.scene.Group;
import javafx.scene.Scene;
import javafx.scene.paint.Color;
import javafx.scene.shape.Circle;
import javafx.scene.shape.Rectangle;
import javafx.scene.text.Text;
import javafx.stage.Stage;

public class ShowShapesA extends Application {
    
    @Override
    public void start(Stage primaryStage) {
        try {
            Group root = new Group();
            // the data that should be graphically represented
            MShape[] shapes = MAllShapes.getDefaultShapes();
            
            for (MShape s : shapes) {
                // check whether the shape is a rectangle
                if (s instanceof MRectangle) {
                    // cast necessary for the methods getXDelta and getYDelta
                    MRectangle srec = (MRectangle) s;
                    Rectangle rec = new Rectangle(srec.getXOrigin(),
                            srec.getYOrigin(), 2*srec.getXDelta(), 2*srec.getYDelta());
                    //color red is made transparent
                    //so that overlapping shapes are visible
                    rec.setFill(Color.rgb(255, 0, 0, 0.15));
                    // the area (number rounded) displayed as text
                    Text text = new Text(srec.getXOrigin(), srec.getYOrigin(),
                            "a: "+Double.toString(Math.round(srec.area())));
                    
                    root.getChildren().addAll(rec, text);
                } else if (s instanceof MCircle) {
                    // cast necessary for the methods getXDelta and getYDelta
                    MCircle sc = (MCircle) s;
                    Circle circ = new Circle(sc.getXOrigin(),
                            sc.getYOrigin(), sc.getRadius());
                    //color green is made transparent
                    //so that overlapping shapes are visible
                    circ.setFill(Color.rgb(0, 255, 0, 0.15));
                    // the area (number rounded) displayed as text
                    Text text = new Text(sc.getXOrigin(), sc.getYOrigin(),
                            "a: "+Double.toString(Math.round(sc.area())));
                    
                    root.getChildren().addAll(circ, text);
                }
            }
            
            Scene scene = new Scene(root,400,400);
            primaryStage.setScene(scene);
            primaryStage.show();
        } catch(Exception e) {
            e.printStackTrace();
        }
    }
    
    public static void main(String[] args) {
        launch(args);
    }

}
```

---
0
MAllShapes.java
```java
package model;

/**
 * a fabric for Shape-Objects.
 * @author Agathe Merceron
 */

import java.util.Arrays;

public class MAllShapes {
    
    /**
     * construct a number of Rectangle and Circle objects, 
     * stores them in an array and returns it
     * @return an array filled with Rectangle or Circle objects
     */
    
    public static MShape[] getDefaultShapes(){
        MShape[] allshapes= new MShape[5];
        allshapes[0] = new MRectangle( 255, 125, 30, 25);
        allshapes[1] = new MRectangle( 155, 75, 10, 40);
        allshapes[2] = new MCircle( 80, 80, 10);
        allshapes[3] = new MRectangle( 45, 105, 80, 40);
        allshapes[4] = new MCircle( 200, 100, 50);
        return allshapes;
    }
    
    public static void main(String[] args) {
        MShape[] mix =  MAllShapes.getDefaultShapes();
        System.out.println(Arrays.deepToString(mix));
    }

}
```




1
MShape.java
```java

package model;


/**
 * Base class for geometric shapes. Each shape has an explicit origin
 * represented by a point: (x,y).
 * 
 * @ version 1.00 27 Apr 2009
 * 
 * @author Gert Veltink, updated Agathe Merceron
 */
public class MShape {

    /** x-coordinate of the origin */
    protected double xOrigin;

    /** y-coordinate of the origin */
    protected double yOrigin;

    /**
     * Constructor for a shape taking as argument the two coordinates of the
     * origin.
     * 
     * @param x
     *            the x-coordinate of the shapes' origin
     * @param y
     *            the y-coordinate of the shapes' origin
     * 
     */
    public MShape(double x, double y) {
        xOrigin = x;
        yOrigin = y;
    }
    
    /**
     * returns the x-origin of this shape
     * @return the x-origin
     */
    
    public double getXOrigin() {
        return xOrigin;
    }
    
    /**
     * returns the y-origin of this shape
     * @return the y-origin
     */
    
    public double getYOrigin(){
        return yOrigin;
    }

    /**
     * calculate the area of a shape. the base shape is just point so it has no
     * area.
     * 
     * @return the area of the shape
     */
    public double area() {
        return 0;
    }

    /**
     * calculate the circumference of a shape. the base shape is just point so
     * it has no circumference.
     * 
     * @return the circumference of the shape
     */
    public double circumference() {
        return 0;
    }

    /**
     * constructs a textual representation of the origin in the form: "(x, y)".
     * *
     * 
     * @return the origin on textual representation
     */
    public String origin() {
        return ("(" + xOrigin + ", " + yOrigin + ")");
    }

    /**
     * constructs a textual representation of the object.
     * 
     * @return the current object in a textual representation
     */
    @Override
    public String toString() {
        return ("Shape with origin: " + origin());
    }
}
```


2
MRectangle.java
```java

package model;

/**
 * represents a Rectangle.
 * A Rectangle is a Shape with a length and a width.
 * @author Gert Veltink, updated Agathe Merceron
 *
 */

public class MRectangle extends MShape {
	
	private double xDelta;
	private double yDelta;
	
	/**
     * constructs a rectangle object 
     * 
     * @param xOrigin
     *            the x-coordinate of the shape's origin
     * @param yOrigin
     *            the y-coordinate of the shape's origin
     * @param xDelta
     *            half of the length
     * @param yDelta
     *            half of the width
     * 
     */
    
	public MRectangle(double xOrigin, double yOrigin, double xDelta, double yDelta) {
        super(xOrigin, yOrigin);
        this.xDelta = xDelta;
        this.yDelta = yDelta;
    }
	
	/**
	 * returns half the length of the rectangle
	 * @return half the length
	 */
	
	public double getXDelta() {
		return xDelta;
	}
	
	/**
	 * returns half the width of the rectangle
	 * @return half the width
	 */
	
	public double getYDelta(){
		return yDelta;
	}

    /**
     * calculate the area of this rectangle.
     * 
     * @return the area of the rectangle
     */
	@Override
    public double area() {
        return (2*this.xDelta) * (2*this.yDelta);
    }

    /**
     * calculate the circumference of this rectangle.
     * 
     * @return the circumference of the rectangle
     */
	@Override
    public double circumference() {
        return (4*this.xDelta) + (4*this.yDelta);
    }

    /**
     * constructs a textual representation of the object.
     * 
     * @return the current object in a textual representation
     */
    @Override
    public String toString() {
    	return "Rectangle with origin: " + origin() + 
				", xDelta: " + this.xDelta + ", yDelta: ";
    }

}


```

3
MCircle.java
```java
package model;

/**
 * This class represents a circle.
 * A circle is a shape with a radius.
 * @author Gert Veltink, updated Agathe Merceron
 *
 */

public class MCircle extends MShape {
    
       private double radius;

        /**
         * Constructs a circle with the given origin and the given radius
         * 
         * @param x
         *            the x-coordinate of the shape's origin
         * @param y
         *            the y-coordinate of the shape's origin
         * @param radius
         *            Radius of this circle
         * 
         */
        public MCircle(double x, double y, double radius) {
            super(x, y);
            this.radius = radius;
        }
        
        public double getRadius(){
            return radius;
        }

        /**
         * calculate the area of a circle. 
         * 
         * @return the area of the circle
         */
        @Override
        public double area() {
            return (Math.PI * (this.radius * this.radius));
        }

        /**
         * calculate the circumference of a circle.
         * 
         * @return the circumference of the circle
         */
        @Override
        public double circumference() {
            return (2 * Math.PI * this.radius);
        }

        /**
         * constructs a textual representation of this circle.
         * 
         * @return the current object in a textual representation
         */
        @Override
        public String toString() {
            return "Circle with origin: " + origin() + 
                    " and radius: " + this.radius;
        }

}

```


# 12 Model View Beispiel:  Lösung mit Reflection 
Lösung mit Reflection (optional)
使用 泛类

---
上一个 解法 中的潜在问题:

Ein Problem der Klasse ShowShapesA ist, dass sie verändert werden muss, wenn das Modell um eine neue geometrische Form ergänzt wird. 
Dies ist im Allgemeinen nicht die Art der Erweiterbarkeit, die man sich wünscht. 
Praktischer und robuster ist es, dass lediglich das Paket (package) view um eine weitere Klasse erweitert werden sollte und dass in den schon existierenden Klassen keine Änderungen notwendig sind; so gibt es keine Gefahr von zusätzlichen Fehlern als sekundärer Effekt der Änderungen in existierenden Klassen. 
Diese Idee wird mit Hilfe von Reflection umgesetzt. 

---

解决方法: 
Die generische Klasse` Class<T>` kann jede Klasse reflektieren, welche in Java geschrieben wird. 
Sei zum Beispiel s ein Objekt, das mit der Klasse MRectangle instanziiert wurde. 

- Die Anweisung` Class<?> c1 = s.getClass();` weist der Variablen c1 das einzige Objekt vom Typ `Class<MRectangle> `zu, welches die Klasse MRectangle reflektiert. 
- Die Methode `getClass` ist in der Klasse Object definiert. 
- `String modelname = s.getClass().getName();`
	- Diese obige Anweisung weist der Variablen modelname den Namen der Klasse zu, womit s instanziiert wurde. 
- Sei viewname eine Variable, welche den vollen Namen (mit Paketen) einer Klasse enthält. 
	- `Class<?> c = Class.forName(viewname);`   
	- Diese obige Anweisung erzeugt das Objekt c, welches die Klasse mit dem Namen wie in viewname angegeben reflektiert.

总结
Wir haben gerade zwei Möglichkeiten gesehen, um das Objekt zu erzeugen, welches eine Klasse reflektiert. 
Die erste Möglichkeit wird benutzt, wenn ein Objekt zur Verfügung steht – wie oben das Objekt s.  
Die zweite Möglichkeit wird benutzt, wenn der volle Name der Klasse bekannt ist, wie zuletzt in der Variable viewname. 
Diese zwei Möglichkeiten werden wir in diesem Abschnitt verwenden.

---
上面解决方法 的 实践

Die erste Möglichkeit wird in der Main-Klasse benutzt, welche immer existiert, wenn mit Eclipse ein neues JavaFX-Projekt erzeugt wird.
```java
scene.getStylesheets().add(getClass().getResource("application.css").toExternalForm())
```
In dieser langen Anweisung ermittelt der Aufruf getclass() das Objekt, welche die Main-Klasse reflektiert. Der Aufruf der Methode getResource("application.css") übermittelt dann die URL dieser Ressource.

什么是 Reflection:
==_Reflection_ ist ein mächtiges Werkzeug. Mit Hilfe dieses Class-Objektes können insbesondere Methoden der Klasse ermittelt und sogar aufgerufen werden. Dies alles passiert dynamisch, je nachdem, welche Klasse vom Objekt c reflektiert wird.

In dieser Lösung wird eine view-Klasse für jede Klasse im model entwickelt, welche grafisch dargestellt werden soll, aktuell die Klassen VRectangle und VCircle. 
Die Konvention der Namen ist an der Stelle wichtig. Eine view-Klasse heisst genauso wie eine Klasse im Paket model; der Unterschied ist lediglich der erste Buchstabe. 
Wichtig ist auch die Konvention für den Aufbau einer solchen V-Klasse:  sie enthält eine einzige Klassenmethode namens getShape mit einem einzigen Parameter vom Typ MShape.

Im Wesentlichen wird in dieser Methode programmiert, was in der Anweisung `if (s instanceof MRectangle)` in der Klasse  ShowShapesA programmiert ist.

---


Klasse VRectangle aus VRectangle.java
```java
package view;

/**
 * A class to represent graphically a MRectangle-Object 
 * @author agathe merceron
 */

import javafx.scene.paint.Color;
import javafx.scene.shape.Rectangle;
import model.MRectangle;
import model.MShape;

public class VRectangle {
    
    /**
     * expects a MRectangle-Object and returns its graphical representation
     * as a Rectangle-Object
     * @param s a MRectangle-Object
     * @return the Rectangle-Object that represents s
     */
    
    public static Rectangle getShape(MShape s) {
        MRectangle srec = (MRectangle) s;
        Rectangle rec = new Rectangle(srec.getXOrigin(),
                srec.getYOrigin(), 2*srec.getXDelta(), 2*srec.getYDelta());
        //color red is made transparent
        //so that overlapping shapes are visible
        rec.setFill(Color.rgb(255, 0, 0, 0.15));
        return rec;
    }

}
```

Klasse VCircle aus VCircle.java
```java
package view;

/**
 * A class to represent graphically a MCircle-Object 
 * @author agathe merceron
 */

import javafx.scene.paint.Color;
import javafx.scene.shape.Circle;
import model.MCircle;
import model.MShape;

public class VCircle {
    
    /**
     * expects a MCircle-Object and returns its graphical representation
     * as a Circle-Object
     * @param s a MCircle-Object
     * @return the Circle-Object that represents s
     */
    
    public static Circle getShape(MShape s) {
        MCircle sc = (MCircle) s;
        Circle circ = new Circle(sc.getXOrigin(),
                sc.getYOrigin(), sc.getRadius());
        //color green is made transparent
        //so that overlapping shapes are visible
        circ.setFill(Color.rgb(0, 255, 0, 0.15));
        return circ;
    }

}
```

---


Wie die Klassen zusammenhängen, ist im folgenden Klassendiagramm dargestellt. Es gilt die gleiche Aussage wie im Programm ShowShapesA: 
Klassen aus Paket view benutzen Klassen aus dem Paket model, aber nicht umgekehrt. Die graphische Darstellung kann verändert werden, die Logik der Anwendung bleibt bestehen. 
Die Anwendung ist völlig dynamisch: es kann eine beliebige Anzahl von Shape-Objekten dargestellt werden; 
die Reihenfolge der Figuren, Rechteck oder Kreis, ist auch beliebig. Außerdem benötigt die Einführung einer neuen Figur lediglich die Einführung  zweier Klassen: die M-Klasse, welche die Mathematik der Figur beschreibt, und die V-Klasse, welche die grafische Darstellung regelt. Alle bestehenden Klassen bleiben unberührt.

![](image/Pasted%20image%2020230522201015.png)




---
==ShowShapesB.java

In der Klasse ShowShapesB gibt es – wie in der Klasse ShowShapesA – eine Schleife durch alle MShape-Objekte. Jetzt aber wird nicht mehr jedes MShape-Objekt mit instanceof abgefragt, um zu wissen, mit welcher Klasse es instanziiert wurde, sondern mit Hilfe der Methode getClass wird das Objekt ermittelt, das die instanziierende Klasse reflektiert. Die Methode getName() wird benutzt, um den Namen der instanziierenden Klasse zu bilden. Dies passiert mit der Anweisung: 

```
String modelname = s.getClass().getName();
```

Durch die Namenskonvention wird der Name der  entsprechenden view-Klasse gebildet - im Quellcode wird dies mit folgender Anweisung programmiert: 
```
String viewname = PREFIXVIEW+modelname.substring(PREFIXMODEL.length());
```

und auch das Objekt wird ermittelt, welches diese View-Klasse reflektiert - im Quellcode mit der folgenden Anweisung: 
```
Class<?> c = Class.forName(viewname);
```

Die Konvention des Aufbaus einer view-Klasse wird benutzt, um die einzige Methode dieser Klasse zu ermitteln - im Quellcode mit der folgenden Anweisung programmiert: 
```
Method m = c.getMethod(METHODNAME, Class.forName(PARAMTYPE));
```

und danach die Methode aufrufen: im Quellcode: 
```
Shape s1 = (Shape) m.invoke(null, s);
```


```java
package view;

/**
 * A class to represent graphically an Array of Shape-objects.
 * A Shape Object can be a rectangle or a circle.
 * The shapes appear with their real dimensions and positions,
 * and their area (number rounded) displayed at their origin.
 * @author agathe merceron
 */


import java.lang.reflect.Method;
import model.MAllShapes;
import model.MShape;
import javafx.application.Application;
import javafx.scene.Group;
import javafx.scene.Scene;
import javafx.scene.shape.Shape;
import javafx.scene.text.Text;
import javafx.stage.Stage;

public class ShowShapesB extends Application {
    private static final String PREFIXMODEL = "model.M";
    private static final String PREFIXVIEW = "view.V";
    private static final String METHODNAME = "getShape";
    private static final String PARAMTYPE = "model.MShape";
    
    
    @Override
    public void start(Stage primaryStage) {
        try {
            Group root = new Group();
            // the data that should be graphically represented
            MShape[] shapes = MAllShapes.getDefaultShapes();
            
            for (MShape s : shapes) {
                    //the name of the class the object s is an instance of
                    String modelname = s.getClass().getName();
                    
                    // the class that should be taken to build the view for s
                    String viewname = PREFIXVIEW + modelname.substring(PREFIXMODEL.length()); // extract the substring from position x to the end of orgianl String. Endindex of substring starts from 1 and not from 0.
                    Class<?> c = Class.forName(viewname);
                    
                    //the Method-Object has to be created in order to invoke the method
                    //the getMethod needs the name of the method and the Class-Object of each parameter
                    Method m = c.getMethod(METHODNAME, Class.forName(PARAMTYPE));
                    
                    // Method getShape is called;
                    // the fist parameter is null, because getShape is static;
                    // the method invoke returns an object of type Object,
                    // therefore the cast
                    Shape s1 = (Shape) m.invoke(null, s);

                    // the area (number rounded) displayed as text
                    Text text = new Text(s.getXOrigin(), s.getYOrigin(), "a: "+ Double.toString(Math.round(s.area())));
                    
                    root.getChildren().addAll(s1, text);
            }
            
            Scene scene = new Scene(root,400,400);
            primaryStage.setScene(scene);
            primaryStage.show();
        } catch(Exception e) {
            e.printStackTrace();
        }
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}

```