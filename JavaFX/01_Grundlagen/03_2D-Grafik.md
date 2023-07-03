
# 1 Linien und Formen in einem Koordinatensystem

## 1.1 Koordinatensystem
Wie in der Mathematik wird die **x - Achse** horizontal von links nach rechts aufsteigend festgelegt, dagegen verläuft die **y - Achse** senkrecht von oben nach unten. Alle Koordinaten sind **positive ganze Zahlen**.

单位 为 pixel: Grober Anhaltspunkt: 100 Pixel nebeneinander erstrecken sich etwa über 3,5 cm. 


Die Position jedes Pixels wird durch ein Zahlenpaar (x | y) (= seine Koordinaten) in einem zweidimensionalen rechtwinkligen kartesischen Koordinatensystem festgelegt, dessen Ursprungspunkt ( 0 | 0 ) in der linken oberen Ecke des Grafik-Fensters bzw. der instanziierten Zeichenfläche liegt.
![](image/Pasted%20image%2020230522185902.png)

![](image/Pasted%20image%2020230529105547.png)




# 2 Superklasse javafx.scene.shape.Shape
Die Klasse Shape ist abstrakt. Sie definiert wichtige Methoden für geometrische Formen, beispielsweise die Farbe und die Stärke des Rahmens oder die Farbe den Innenraums.

![](image/Pasted%20image%2020230522190251.png)

In der Klasse Shape finden sich weitere hilfreiche Methoden, um beispielsweise den Durchschnitt (Methode intersect) oder die Vereinigung (Methode union) zweier Shape-Objekte zu bilden.  

## 2.1 javafx.scene.Group als Wurzel des scene graph 

Die Benutzung der Klasse javafx.scene.Group als Wurzel des scene graph 

```java
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
```


# 3 union and intersect und Substract

```java
Shape union = Shape.union(poly1, poly2);
Shape intersect = Shape.intersect(poly1, poly2);
Shape subtract = Shape.subtract(poly1, poly2);
```

原本 
e 为 ellipse,  为 红色 为先加入
r 为 rechtangle . 为黑色。 r 为后加入 
![](image/Pasted%20image%2020230702110714.png)


## 3.1 union

原本 
e 为 ellipse,  为 红色 为先加入
r 为 rechtangle . 为黑色。 r 为后加入 

union  的作用： 全部的部分 被变成了蓝色

![](image/Pasted%20image%2020230702110801.png)
![](image/Pasted%20image%2020230702110806.png)

## 3.2 intersect

原本 
e 为 ellipse,  为 红色 为先加入
r 为 rechtangle . 为黑色。 r 为后加入 

intersect 的作用： 只是交叉的部分 被变成了蓝色

![](image/Pasted%20image%2020230702110820.png)

![](image/Pasted%20image%2020230702110835.png)

## 3.3 subtract 


原本 
e 为 ellipse,  为 红色 为先加入
r 为 rechtangle . 为黑色。 r 为后加入 

Shape.subtract(e, r)   :  e 没有被 r 覆盖的地方 变为蓝色 
![](image/Pasted%20image%2020230702110920.png)


Shape.subtract(r, e)   :  
r 没有被 e 覆盖的地方 ， 变为蓝色
r 没有被 e 覆盖的地方， 仍然保持 e 的本来颜色 红色
e 中没有被 r 覆盖的地方， 仍然 保持为本来的颜色 黑色。 
 
![](image/Pasted%20image%2020230702111017.png)

![](image/Pasted%20image%2020230702111050.png)

# 4 javafx.scene.shape.Line

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



## 4.1 例子 GeometrischeFiguren01



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


# 5 Circle

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



# 6 Die Klasse Rectangle 

## 6.1 Konstructer  Rectangle(double x, double y, double width, double height)` 

Die Klasse Rectangle erlaubt rechteckige Formen zu zeichnen. Der Konstruktor` Rectangle(double x, double y, double width, double height)` erzeugt ein Rectangle-Objekt mit der Länge (width) und Höhe (height), dessen obere linke Ecke die Koordinaten (x, y) hat.

Die folgende Grafik zeigt ein Rechteck (rectangle) mit den Koordinaten (40 | 60), einer Breite von 200 Pixel und einer Höhe von 40 Pixel. Dieses kann mit der folgenden Anweisung erzeugt werden: `new Rectangle(40, 60, 200, 40);`

```
Rectangle rect = new Rectangle( startx, starty, width, height);
```

![](image/Pasted%20image%2020230702101242.png)

## 6.2 Method (setArcHeight/Width, setStroke, setStrokeWidth/Height, setFill  )


### 6.2.1 setArcHeight und setArcWidth
边沿变得圆滑

Mit den Methoden setArcHeight und setArcWidth der Klasse Rectangle können die spitzen Ecken des Rechtecks abgerundet werden.

![](image/Pasted%20image%2020230702101340.png)

### 6.2.2 setStroke und setStrokeWidth
Wie Line ist Rectangle eine Unterklasse (subclass) von Shape. Damit können die schon gesehenen Methoden setStroke und setStrokeWidth benutzt werden, um den Rand eines Rectangle-Objektes anzupassen


### 6.2.3 setFill 
Die Methode setFill erlaubt, die Farbe des inneren Bereiches eines Shape-Objektes zu bestimmen. Diese drei Methoden werden mit der Variable rec1 benutzt.


## 6.3 例子1 

```java
Rectangle rect = new Rectangle( startx, starty, width, height);
Rectangle rect = new Rectangle( width/3, height/3, 250, 100);

rect.setFill(new Color(1.0, 0.0, 0.0, 1.0));
rect.setArcHeight(30);
rect.setArcWidth(30);
rect.setStrokeWidth(5);
rect.setStroke(Color.rgb(30, 70, 125));
rect.setStrokeType(StrokeType.OUTSIDE);
rect.setStrokeType(StrokeType.INSIDE);


rect.getTransforms().add( new Translate(2, 8));
rect.getTransforms().add( new Scale(0.5, 0.5));
rect.getTransforms().add( new Rotate(45, 2, 8));
```

![](image/Pasted%20image%2020230529205919.png)


## 6.4 例子3

```java
Rectangle rect = new Rectangle( startx, starty, width, height);
Rectangle rect = new Rectangle( width/3, height/3, 250, 100);
rect.setFill(new Color(1.0, 0.0, 0.0, 1.0));

Text txt = new Text( rect.getX(), rect.getY(), "(" + rect.getX() + "/" + rect.getY() + ")");
txt.setFont(new Font("Arial", 30));

Group root = new Group();
root.getChildren().addAll(rect, txt);
```

![](image/Pasted%20image%2020230529205943.png)

## 6.5 例子 2

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



# 7 javafx.scene.paint.LinearGradient/RadialGradient

LinearGradient
RadialGradient

Es wird nicht mit einer einzigen Farbe gefüllt, sondern mit Farben, die progressiv ineinander übergehen - einem Verlauf (gradient). JavaFX stellt dafür zwei Klassen zur Verfügung: javafx.scene.paint.LinearGradient; und javafx.scene.paint.RadialGradient.

Mit der Klasse LinearGradient erfolgt der progressive Wechsel einer Farbe entlang einer Linie, 
mit der Klasse RadialGradient entlang eines Kreises. 

## 7.1 LinearGradient

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


### 7.1.1 具体解读 
Wir betrachten hier die erste gradient-Methode:

```
private LinearGradient gradient1() {
       return new LinearGradient (0, 0, 0, 1, true, CycleMethod.NO_CYCLE,
              new Stop(0.0, Color.GREEN), new Stop(1.0, Color.RED));
}
```

Die Form, welche das LinearGradient-Objekt füllen soll, kann beliebig groß sein. 
- Die richtige Größe wird abstrahiert. Für das LinearGradient-Objekt wird sie auf dem Intervall [0, 1] skaliert, weil wir true für den Parameter proportional gewählt haben - was sich (fast) immer empfiehlt. 
	- Somit bedeutet 0, 0 die Ecke links oben und 0, 1 die Ecke links unten: die Linie für die Veränderung der Farbe ist von oben nach unten. 
- Die Stop-Elemente bestimmen, wo eine Farbe anfängt. Hier wird die grüne Farbe oben anfangen und die rote Farbe unten.Dazwischen gehen diese zwei Farben progressiv ineinander; 
	- mit CycleMethod.NO_CYCLE gibt es keine Wiederholung.

Was passiert mit der Änderung: 0, 0, 1, 0 statt 0, 0, 0, 1?
- Die Linie geht jetzt von der oberen Ecke links zur oberen Ecke rechts. In diesem Fall wird die grüne Farbe links anfangen und die rote Farbe rechts, siehe gradient2().

### 7.1.2 使用 

```java

// a third rectangle below the first one
Rectangle rec3 = new Rectangle(SPACE, WIDTH/3, WIDTH/3, (HEIGHT-SPACE)/3);
// makes a smooth change of color
rec3.setFill(gradient1());

private LinearGradient gradient1() {
	return new LinearGradient (0, 0, 0, 1, true, CycleMethod.NO_CYCLE,
			new Stop(0.0, Color.GREEN), new Stop(1.0, Color.RED));
}

```


### 7.1.3 例子

![](image/Pasted%20image%2020230522191238.png)

Das Objekt rec2 wird mit einer transparenten Farbe (Magenta) gefüllt, seine Ecken (corners) werden abgerundet und dann so erzeugt, dass es rec1 überlappt. 


Das letzte Objekt namens rec3 wird unter rec1 platziert. Es wird nicht mit einer einzigen Farbe gefüllt, sondern mit Farben, die progressiv ineinander übergehen - einem Verlauf (gradient). JavaFX stellt dafür zwei Klassen zur Verfügung: javafx.scene.paint.LinearGradient; und javafx.scene.paint.RadialGradient.

javafx.scene.paint.LinearGradient;
![](image/Pasted%20image%2020230702104109.png)

javafx.scene.paint.RadialGradient.
![](image/Pasted%20image%2020230702104149.png)


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


# 8 geometrische Transformationen (` setRotate, setTranslateX, setTranslateY `)

Mit Hilfe der Methoden` setRotate, setTranslateX, setTranslateY `der Klasse Node ist es möglich, das Node-Elemente samt ihrer Kinder zu drehen und zu versetzen.   
Das Paket javafx.scene.shape hat noch andere fertige geometrische Figuren wie die Klassen Circle, Ellipse, Sphere usw.


```java
Rectangle rect = new Rectangle( startx, starty, width, height);
Rectangle rect = new Rectangle( width/3, height/3, 250, 100);

rect.setFill(new Color(1.0, 0.0, 0.0, 1.0));
rect.setArcHeight(30);
rect.setArcWidth(30);
rect.setStrokeWidth(5);
rect.setStroke(Color.rgb(30, 70, 125));
rect.setStrokeType(StrokeType.OUTSIDE);
rect.setStrokeType(StrokeType.INSIDE);


rect.getTransforms().add( new Translate(2, 8));
rect.getTransforms().add( new Scale(0.5, 0.5));
rect.getTransforms().add( new Rotate(45, 2, 8));
```

![](image/Pasted%20image%2020230529205919.png)



# 9 javafx.scene.shape.Polygon （geometrische Figuren） 

多边形链 Polyline
Polygone  (geschlossene Polyline-Objekte) 

Die Klassen Polyline und Polygone (geschlossene Polyline-Objekte) erlauben es, eigene geometrische Figuren zu erstellen. 
Polygone sind Vielecke oder Streckenzüge.  Der/Die ProgrammiererIn übergibt als Eingabeparameter eine Menge von Punkten, deren Position im Pixel-Koordinatensystem durch die Koordinaten x, y vorgegeben ist.

## 9.1 ##
Die Anweisung `new Polygon(40, 40, 40, 80, 80, 80);` erzeugt ein rechtwinkliges Dreieck.

使用 Group ,Shape,  setFill(), 

```java
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
```


## 9.2 例子
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


## 9.3 例子 
Geometrische Figuren mit den Klassen Polyline und Polygone

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


