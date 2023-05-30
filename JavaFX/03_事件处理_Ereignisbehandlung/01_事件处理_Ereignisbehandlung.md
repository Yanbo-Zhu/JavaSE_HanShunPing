
# 1 理论

## 1.1 Interaktionen durch Bindings oder Ereignisbehandlung
- Bindings: Abhängigkeit zwischen zwei Properties. Bei Änderung der Quelleigenschaft, wird die Ziel-Eigenschaft automatisch aktualisiert. z.B. bei einem Schieberegler ändert sich sofort die Ansicht
- Ereignisbehandlung: Reaktion der Anwendung auf Benutzerinteraktionen - Ereignisse können durch den Controller verarbeitet werden
	-  das funktionale Interface `javafx.event.EventHandler<T extends Event>`
-` EventHandler<ActionEvent>`  oder  `EventHandler<MouseEvent>` and Methoden `setOnAction`


## 1.2 Methode setOnAction and `EventHandler<ActionEvent>`

Die Methode setOnAction ist in mehreren Klassen des Paketes javafx.scene.control wie z. B. TextField oder ButtonBase vorhanden. 
Sie erwartet einen Parameter vom Typ `EventHandler<ActionEvent>`. Mit der Implementierung der einzigen Methode handle des Interface `EventHandler<ActionEvent> `wird auf das ausgelöste Ereignis reagiert.

`EventHandler<ActionEvent>` ist ein so genanntes funktionales Interface: es hat eine einzige abstrakte Methode. Es kann mit Hilfe eines Lambda-Ausdruckes implementiert werden. 

## 1.3 Action, Event and handler 对照关系
![](image/Pasted%20image%2020230505233650.png)

- Node class 中包含了各种 Event
	- ![](image/Pasted%20image%2020230505233852.png)
	- 每个UI空间 也包含自己的 setOnAction()


# 2 例子 (Grundlagen der Ereignisbehandlung und Binding)

## 2.1 
1 点击 button, label 就会向上移动
![](image/Pasted%20image%2020230505235143.png)

![](image/Pasted%20image%2020230505235127.png)

## 2.2 

setOnkeyReleased(): 键盘释放 
按 keydown 键盘中向下箭头的 key, label 向下
![](image/Pasted%20image%2020230506000047.png)

## 2.3 

拖拽事件 
将一个文本拖进窗口中的 一个 文本框中. 然后松开鼠标, 然后 文本框中显示这个 文件的绝对路径 

![](image/Pasted%20image%2020230506000719.png)

![](image/Pasted%20image%2020230506000750.png)

## 2.4 GP2 教材上的

### 2.4.1 hotel010.java

```java

package application;

import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.stage.Stage;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.control.TextField;
import javafx.scene.image.Image;
import javafx.scene.image.ImageView;
import javafx.scene.layout.GridPane;
import javafx.scene.text.Font;
import javafx.scene.text.Text;

/**


 * A class to illustrate event handling and binding
 * @author agathe merceron
 *
 */


public class Hotel010 extends Application {
    @Override
    public void start(Stage primaryStage) {
        try {
            GridPane root = new GridPane();

            root.setHgap(10);

            root.setVgap(10);

            root.setPadding(new Insets(25, 25, 25, 25));
            
            Text hotelIbisMitte =
                new Text("ibis Styles Hotel Berlin Mitte\n"
                       + "Brunnenstrasse 1-2\n" +"10119 Berlin");

            root.add(hotelIbisMitte, 1, 0);
            
            Image image = new Image (getClass().getResource(
                "/resources/ibisMitteBerlin.jpg").toString());
            ImageView imageview = new ImageView(image);

            root.add(imageview, 0, 0);
            
            
            
            Label labelrating = new Label("current rating:");

            root.add(labelrating, 0, 1);
            Label currentrating = new Label("3.75");
            root.add(currentrating, 1, 1);
            
            Button button = new Button("Enter your rating");
            //font is part of the skin can be specified 
            // in a css file
            button.setFont(Font.font("Arial", 20));
            
            root.add(button, 0, 3);
            
            Label l = new Label("Your rating:");
            l.setVisible(false);
            root.add(l, 0, 5);
            
            TextField myrating = new TextField("3.75");
            myrating.setVisible(false);
            //binds the text of currentrating to the text of myrating
            currentrating.textProperty().bind(myrating.textProperty());
            root.add(myrating, 1, 5);
            // The Parameter of setOnAction is of Type 
            // EventHandler<T extends Event>, a functional Interface.
            // the unique instance method handle is implemented
            // with a lambda expression.
            // Preferred way with an extra method here enterRating
            // when the body of the method has several lines



            button.setOnAction(e -> enterRating(l, myrating));
            // other possible way because the body of the method
            // is still small, only 2 lines.
//          button.setOnAction(e -> {l.setVisible(true);
//              myrating.setVisible(true);
//          });
            
            
            Scene scene = new Scene(root,400,400);
            primaryStage.setScene(scene);
            primaryStage.show();
        } catch(Exception e) {
            e.printStackTrace();
        }
    }
    

    private void enterRating(Label l, TextField t){
        l.setVisible(true);
        t.setVisible(true);
    
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}

```


![](image/Pasted%20image%2020230528202437.png)

1 
```
button.setOnAction(e -> enterRating(l, myrating));
```

Die in der Oberklasse ButtonBase definierte Methode setOnAction erlaubt es festzulegen, was passiert, wenn das Button-Objekt button geklickt wird. Diese Methode erwartet ein Objekt vom Typ` EventHandler<ActionEvent>`. Das Interface` javafx.event.EventHandler<T extends Event>` ist ein generisches funktionales Interface. 
Funktional heißt , das Interface definiert eine einzige Objekt-Methode, auch Instanz-Methode genannt, mit dem Kopf :void handle (T event). In unserem Fall wird der generische Typ T mit dem Typ ActionEvent ersetzt, der Typ des Ereignisses, das ausgelöst wird, wenn ein Button-Objekt geklickt wird.

Merken Sie, dass ActionEvent eine Unterklasse von Event ist, was auch die Schreibweise T extends Event bedeutet: T muss entweder Event oder eine Unterklasse von Event sein. Statt eine Klasse zu erstellen, welche das Interface implementiert, und ein Objekt dieser Klasse zu erzeugen, ist es möglich, mittels eines Lambda-Ausdruckes lediglich den Rumpf dieser einzigen Methode zu programmieren. Der Rest passiert hinter der Kulissen. Der Rumpf dieser Methode wird hier in der Hilfsmethode enterRating programmiert und bewirkt, dass die Bedienelemente - damit ein Benutzer seine Bewertung geben kann - sichtbar werden.

2

```
currentrating.textProperty().bind(myrating.textProperty());
```
 	
Die Methode textProperty() der direkten Oberklasse javafx.scene.control.Labeled gibt das Property-Objekt zurück, das den eigentlichen Text des Label-Objektes enthält. 
Die Verpackung von Text in ein Property-Objekt bewirkt, dass jede Änderung im Text ein Ereignis auslöst.
Die im Interface` javafx.beans.property.Property<T>` deklarierte Methode bind verbindet die zwei properties und im Endeffekt die zwei Texte der Objekte currentrating und  myrating und erlaubt somit eine Reaktion auf die ausgelösten Ereignisse. 
Diese Verbindung durch die Methode bind hat eine Richtung: Änderungen im Text des Objektes myrating werden gleich im Text des Objektes currentrating gespiegelt. In der Abbildung unten merken Sie, dass die eingegebene Bewertung 5.4 genauso rechts von der Beschriftung "current rating" zu sehen ist.


### 2.4.2 hotel Hotel01EventHandler

 
In der Variante namens Hotel01EventHandler wird die Bewertung des Hotels erst verändert, wenn der Benutzer die Taste "enter" gedrückt hat, nachdem er seine Bewertung abgegeben hat. Dies wird mit dem Weg der Ereignisbehandlung realisiert. In der Abbildung unten sehen Sie, dass die Bewertung rechts von der Beschriftung "current rating" den alten Wert zeigen kann, solange die Taste "enter" vom Benutzer nicht gedrückt wird.

![](image/Pasted%20image%2020230529224531.png)

```java
package application;

import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.stage.Stage;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.control.TextField;
import javafx.scene.image.Image;
import javafx.scene.image.ImageView;
import javafx.scene.layout.GridPane;
import javafx.scene.text.Text;

/**
 * A class to illustrate event handling with textField
 * @author agathe merceron
 *
 */


public class Hotel01EventHandler extends Application {
    @Override
    public void start(Stage primaryStage) {
        try {
            GridPane root = new GridPane();
            root.setHgap(10);
            root.setVgap(10);
            root.setPadding(new Insets(25, 25, 25, 25));
            
            Text hotelIbisMitte =
            new Text("ibis Styles Hotel Berlin Mitte\n"
                   + "Brunnenstrasse 1-2\n" +"10119 Berlin");
            root.add(hotelIbisMitte, 1, 0);
            
            Image image = new Image (getClass().getResource(
                "/resources/ibisMitteBerlin.jpg").toString());
            ImageView imageview = new ImageView(image);
            root.add(imageview, 0, 0);
            
            
            
            Label labelrating = new Label("current rating:");
            root.add(labelrating, 0, 1);
            Label currentrating = new Label("3.75");
            root.add(currentrating, 1, 1);
            
            Button button = new Button("Enter your rating");
            
            root.add(button, 0, 3);
            
            Label l = new Label("Your rating:");
            l.setVisible(false);
            root.add(l, 0, 5);
            
            TextField myrating = new TextField();
            myrating.setVisible(false);
            root.add(myrating, 1, 5);
            // The Parameter of setOnAction inherited from ButtonBase
            // is of Type EventHandler<T extends Event>, a functional
            // Interface.
            // The unique instance method handle is implemented
            // with a lambda expression.
            // Preferred way with an extra method as here enterRating
            // when the body of the method has several lines
            button.setOnAction(e -> enterRating(l, myrating));
            // Other possible way because the body of the method
            // is still small: only 2 lines.
//          button.setOnAction(e -> {l.setVisible(true);
//              myrating.setVisible(true);
//          });
            
            // The Parameter of setOnAction of the class TextField
            // is of Type EventHandler<T extends Event>, a functional
            // Interface.
            // The unique instance method handle is implemented
            // with a lambda expression.
            // As the body of the method has 2 lines only
            // it is programmed directly in the lambda expression
            
            myrating.setOnAction( e -> {
                String s = myrating.getText();
                    currentrating.setText(s);
            });
            
            Scene scene = new Scene(root,400,400);
            primaryStage.setScene(scene);
            primaryStage.show();
        } catch(Exception e) {
            e.printStackTrace();
        }
    }
    
    private void enterRating(Label l, TextField t){
        l.setVisible(true);
        t.setVisible(true);
    
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```


Wir betrachten jetzt die Anweisung:

```
myrating.setOnAction( e -> {
     String s = myrating.getText();
     currentrating.setText(s);
});
```

Die Methode setOnAction, diesmal in der Klasse TextField selbst definiert, erlaubt zu programmieren, was passiert, wenn die _enter_-Taste in einem TextField-Objekt gedrückt wird. Wie im Hotel010 erwartet diese Methode ein Objekt vom Typ `EventHandler<ActionEvent>`. 
Auch hier wird die einzige Objekt-Methode mittels eines Lambda-Ausdruckes implementiert und bewirkt, dass der Inhalt des TextField-Objektes myrating den Text des Label-Objektes currentrating ersetzt.

# 3 例子 Die Klasse javafx.scene.control.Slider und Maus-Ereignisse

Die Klasse Slider erlaubt einen Schieberegler darzustellen. Der Wert des Slider-Objektes kann benutzt werden, um z. B. andere Figuren zu skalieren. Dies wird im Programm SliderCircle eingesetzt: die Größe des Kreises wird mit dem Wert des Slider-Objektes gebunden und somit skaliert.

![](image/Pasted%20image%2020230529232442.png)

```java
package application;
    
import javafx.application.Application;
import javafx.stage.Stage;
import javafx.scene.Scene;
import javafx.scene.control.Slider;
import javafx.scene.layout.BorderPane;
import javafx.scene.shape.Circle;

/**
 * This program demonstrates the binding of a Node, here a Circle,
 * to the value of a Slider-Object.
 * Idea borrowed from JavaFX 8 by Anton Epple p. 58
 * @author agathe merceron
 *
 */


public class SliderCircle extends Application {
    //dimension of the scene
    private static final double WIDTH = 350;
    private static final double HEIGTH = 400;
    
    @Override
    public void start(Stage primaryStage) {
        try {
            BorderPane root = new BorderPane();
            // the radius of the circle is adjusted
            // to fit half of the smallest side
            double min = WIDTH > HEIGTH ? HEIGTH : WIDTH;
            
            Circle c = new Circle(WIDTH/2, HEIGTH/2, min/2);
            root.setCenter(c);
            // the slider can take values in the range [0, 1]
            // with initial value 0.5 
            Slider slider = new Slider(0, 1, 0.5);
            root.setBottom(slider);
            // Circle inherits from Node and so of the
            // properties to scale a Node-object 
            // along the X-axis and Y-axis
            c.scaleXProperty().bind(slider.valueProperty());
            c.scaleYProperty().bind(slider.valueProperty());
            // comment out one of the binding
            // and notices what changes when you slide the slider
            
            Scene scene = new Scene(root,WIDTH,HEIGTH);
            
            primaryStage.setScene(scene);
            primaryStage.setTitle("SliderCircle");
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

ie kontinuierliche Änderung des Kreises durch das Binding von properties kann in manchen Anwendungen zu Performance-Problemen führen. Die Variante SlideCircleEvent bewirkt, dass der Kreis erst skaliert wird, wenn die Maus losgelassen wird, und somit der Wert des Schiebereglers sich nicht mehr ändert. Dabei werden die properties der Oberklasse javafx.scene.Node benutzt.

```java
package application;
    
import javafx.application.Application;
import javafx.stage.Stage;
import javafx.scene.Scene;
import javafx.scene.control.Slider;
import javafx.scene.layout.BorderPane;
import javafx.scene.shape.Circle;

/**
 * This program demonstrates the scaling of a Node, here Circle,
 * when the mouse is released from another Node, here Slider.
 * @author agathe merceron
 *
 */


public class SlideCircleEvent extends Application {
    //dimension of the scene
    private static final double WIDTH = 350;
    private static final double HEIGTH = 400;
    
    @Override
    public void start(Stage primaryStage) {
        try {
            BorderPane root = new BorderPane();
            
            double min = WIDTH > HEIGTH ? HEIGTH : WIDTH;
            
            Circle c = new Circle(WIDTH/2, HEIGTH/2, min/2);
            root.setCenter(c);
            
            Slider slider = new Slider(0, 1, 0.5);
            root.setBottom(slider);
            
            // Slider inherits from Node and so
            // from all the properties related to the
            // movements of the mouse.
            // When the mouse is released from slider, 
            // the ActionHandler programmed as lambda expression
            // causes the Circle c to scale
            slider.setOnMouseReleased(e -> {
                c.setScaleX(slider.getValue());
                c.setScaleY(slider.getValue());
            });
            
            
            Scene scene = new Scene(root, WIDTH,HEIGTH);
            
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

Die Klasse javafx.scene.Node hat sämtliche Properties, welche die Maus, Tastatur sowie Gesten wie Scrollen oder Wischen (Swipe) usw. betreffen. Da etliche Klassen von der Klasse javafx.scene.Node erben, kann auf viele verschiedene Ereignisse reagiert werden.


# 4 例子: Menüs

![](image/Pasted%20image%2020230529232609.png)

```java
package application;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.CheckMenuItem;
import javafx.scene.control.Menu;
import javafx.scene.control.MenuBar;
import javafx.scene.control.MenuItem;
import javafx.scene.control.SeparatorMenuItem;
import javafx.scene.layout.BorderPane;
import javafx.scene.paint.Color;
import javafx.stage.Stage;

/**
 * this class demonstrates how to make a menu, 
 * and further event handling.
 * Idea from Mastering JavaFX controls, Hendrik Ebbers, McGraw-Hill Education 
 * @author agathe merceron
 *
 */

public class MenuBarDemo extends Application {
    
    @Override
    public void start(Stage primaryStage) {
        try {

            MenuBar bar = new MenuBar();
            
            // the Main menu makes changes on the Button-Object
            Menu mainmenu = new Menu("Main");
            Button b1 = new Button("Button 1");

            MenuItem rotateItem = new MenuItem("rotate");
            rotateItem.setOnAction(e -> b1.setRotate(b1.getRotate()+30));
            
            CheckMenuItem underlineItem = new CheckMenuItem("underline");
            // when the item is not selected anymore, the text
            // of the button is not anymore underlined
            b1.underlineProperty().bind(underlineItem.selectedProperty());
            
            //looks like a CheckItem but has the toggle properties
            MenuItem blueItem = new MenuItem("make blue");
            blueItem.setOnAction(e -> b1.setTextFill(Color.BLUE));
            
            mainmenu.getItems().addAll(rotateItem,  underlineItem, blueItem);
            
            // to show how to make more complex menus
            Menu editmenu = new Menu("Edit");
            Menu convertmenu = new Menu("Convert");
            convertmenu.getItems().addAll(new MenuItem("PDF"), new MenuItem("png"));
            // the SeraratorMenuItem adds a line between the items
            editmenu.getItems().addAll(convertmenu, new SeparatorMenuItem(),
                    new MenuItem("copy-paste"));
            
            bar.getMenus().addAll(mainmenu,  editmenu);

            BorderPane pane = new BorderPane();
            pane.setTop(bar);
            pane.setCenter(b1);
            
            Scene scene = new Scene(pane,400,400);
            
            primaryStage.setScene(scene);
            primaryStage.setTitle("MenuBarDemo");
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


Wir betrachten die Anweisung in Zeile 36:
`rotateItem.setOnAction(e -> b1.setRotate(b1.getRotate()+30));`

Die Variable rotateItem ist vom Typ MenuItem. Die Methode setOnAction, diesmal in der Klasse MenuItem selbst definiert (merken Sie sich die konsequente Benennung dieser Methoden durch die Klassen), erlaubt es zu programmieren, was passieren soll, wenn ein MenuItem-Objekt selektiert wird. Wie in den Klassen ButtonBase oder TextField erwartet diese Methode ein Objekt vom Typ `EventHandler<ActionEvent>.` Auch hier wird die einzige Objekt-Methode mittels eines Lambda-Ausdruckes implementiert und bewirkt, dass das Button-Objekt b1 um weitere 30 Grad aus der aktuellen Position gedreht wird.

Wir betrachten jetzt die Anweisung (Zeile 41):
`b1.underlineProperty().bind(underlineItem.selectedProperty());`

Die Variable underlineItem ist vom Typ CheckMenuItem. Die Methode selectedProperty – in der Klasse CheckMenuItem definiert – gibt das property-Objekt zurück, das eine Boolsche Variable (Typ bool) verpackt – nämlich, ob dieses Item selektiert ist oder nicht. Wenn es selektiert ist, wird die Schrift in b1 unterstrichen, wenn nicht, hat es keine Unterstreichung.


# 5 Das Beispiel Hotel in model-view-controller

![](image/Pasted%20image%2020230529232724.png)

