
# 1 model-view-controller: Das Beispiel Hotel

Die Trennung in model und view erlaubt, dass die gleiche Logik mit einer anderen grafischen Darstellung wieder verwendet werden kann.
Graphische Anwendungen werden in den Paketen model - view - controller gegliedert. 
Im Paket `controller` befinden sich Klassen, in denen Reaktionen auf Ereignisse programmiert sind, die wiederum Veränderungen im Modell bewirken.

View zeigt Model
View reagiert  Controller 
Controller aktualisiert model 


![](image/Pasted%20image%2020230529232724.png)

1 
```java
yourrating.setOnAction(new RatingDialogController(primaryStage, h, 
       currentrating));
```

![](image/Pasted%20image%2020230702125442.png)
 
Die Klasse RatingDialogController, die wir gleich näher betrachten werden, implementiert das Interface` EventHandler<ActionEvent>`. 
Die Aufgabe dieses Objektes hat jetzt weitere Wirkungen: sie soll die Eingabe einer Bewertung ermöglichen und diese Eingabe soll wiederum die Aktualisierung des model.Hotel-Objektes und die Aktualisierung der Anzeige bewirken, falls die Bewertung gültig ist.

Die Klasse RatingDialogController wird im Paket controller programmiert. Sie ist eine kleine Klasse, die lediglich auf das Anklicken mit der Anzeige eines neuen Fensters reagiert. Das neue Fenster wird in der Klasse RatingDialog im Paket view programmiert. 
Dieses Fenster gibt BenutzerInnen die Möglichkeit, eine Bewertung sowie einen Kommentar einzugeben und mit dem Button submit diese Eingaben für die Weiterverarbeitung frei zu geben. Die Reaktion auf das Anklicken des submit-Buttons wird in der Klasse NewRatingController im Paket controller programmiert.


2 
```
submit.setOnAction(new NewRatingController(hotel, currentRating, 
                    enterrating, rating, area, secondStage));
```

Wie die Klasse RatingDialogController implementiert die Klasse NewRatingController das Interface` EventHandler<ActionEvent> `und wird ebenso im Paket controller programmiert.Ihre Aufgabe ist, den Input zu kontrollieren.

Wenn der angegebene Wert eine Zahl zwischen 1 und 5 ist, wird das Hotel-Objekt aktualisiert und die neue Bewertung angezeigt. Die Klassen-Methode Mix.formatTo2Decimals bewirkt, dass nur zwei Dezimalwerte angezeigt werden. Das Fenster wird zum Schluss geschlossen. Wenn die angegebene Zahl nicht richtig ist, wird der Prompt-Text für die Eingabe der Bewertung rot gefärbt.


# 2 Model-View: Beispiel 


Im Studienmodul Grundlagen der Programmierung I  haben Sie die Klassen Rectangle, Circle usw. als Unterklassen (subclass) der Klasse Shape programmiert. Diese Klassen – um ein paar Getters ergänzt – beschreiben die mathematischen Eigenschaften der Figuren, wie Ursprung, Länge, Fläche usw. und bilden jetzt unser Modell – diese Shape-Objekte werden nun grafisch dargestellt.

Wir nehmen hier eine Umbenennung der Klassen vor, um eine lange Benennung in der Klasse im Paket (package)  view zu vermeiden. In der Tat gibt es sowohl die Klasse Rectangle im Paket model, um Rectangle-Objekte mathematisch zu beschreiben – als auch im Paket javafx.scene.shape, um Rectangle-Objekte grafisch darzustellen. 

In der Klasse des Pakets view sollte immer z. B. model.Rectangle geschrieben werden, wenn die Klasse für die mathematischen Objekte gemeint ist. Um diese lange Schreibweise zu vermeiden, werden einfach alle Klassen des Paketes model das Präfix M haben.


----
 	
Das folgende Klassendiagramm zeigt, wie die Klassen strukturiert sind und wie sie zusammenhängen. Die Richtung der Pfeile zeigt an, welche Klasse durch die andere Klasse benutzt wird. Klassen aus Paket view benutzen Klassen aus dem Paket model, aber nicht umgekehrt. Die graphische Darstellung kann verändert werden, die Logik der Anwendung bleibt bestehen.

Die Anwendung ist völlig dynamisch: es kann eine beliebige Anzahl von Shape-Objekten dargestellt werden; die Reihenfolge der Figuren, Rechteck oder Kreis, ist auch beliebig.

![](image/Pasted%20image%2020230522193637.png)

## 2.1 View ShowShapesA.java
ShowShapesA.java

![](image/Pasted%20image%2020230522193540.png)
 	
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


## 2.2 Model
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


# 3 Model-View Beispiel:  Lösung mit Reflection 
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


## 3.1 Klassendiagramm
Wie die Klassen zusammenhängen, ist im folgenden Klassendiagramm dargestellt. Es gilt die gleiche Aussage wie im Programm ShowShapesA: 
Klassen aus Paket view benutzen Klassen aus dem Paket model, aber nicht umgekehrt. Die graphische Darstellung kann verändert werden, die Logik der Anwendung bleibt bestehen. 
Die Anwendung ist völlig dynamisch: es kann eine beliebige Anzahl von Shape-Objekten dargestellt werden; 
die Reihenfolge der Figuren, Rechteck oder Kreis, ist auch beliebig. Außerdem benötigt die Einführung einer neuen Figur lediglich die Einführung  zweier Klassen: die M-Klasse, welche die Mathematik der Figur beschreibt, und die V-Klasse, welche die grafische Darstellung regelt. Alle bestehenden Klassen bleiben unberührt.

![](image/Pasted%20image%2020230522201015.png)



## 3.2 View ShowShapesB.java

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

## 3.3 View VRectangle aus VRectangle.java

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

## 3.4 View  VCircle aus VCircle.java
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


