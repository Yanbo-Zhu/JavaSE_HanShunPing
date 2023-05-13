

Diese Lerneinheit zeigt den Aufbau einer JavaFX-Anwendung, das Prinzip Layout und wichtige Layouts in JavaFX sowie das Paket javafx.scene.control. In der folgenden Lerneinheit werden weitere grafische Elemente behandelt, insbesondere Klassen des Pakets javafx.scene.shape. In der Lerneinheit FXE kommt Leben in die Anwendung durch die Ereignisbehandlung und das Binding.

# 1 Geschichte von JavaFX 
  	

Für die Erstellung grafischer Oberflächen wurde 1995 das Framework AWT (Abstract Window Toolkit)  entwickelt, was bereits Ende 1996 von Swing abgelöst wurde. Ein Nachteil des Frameworks AWT war seine Plattform-Abhängigkeit, was mit Swing korrigiert wurde. JavaFX wurde als die nächste Etappe in dieser Evolution bezeichnet und verbreitete sich vor allem nach der Einführung von Java 8 und den Lambda-Ausdrücken in 2014 als Ablösung von Swing. Einige der Merkmale von JavaFX sind:

    bessere Performance mit der Benutzung der Grafik-Engine "Prism"
    neue Funktionalitäten wie "Data Binding" und Animationen
    Möglichkeit, ein User-Interface in FXML zu definieren
    Möglichkeit, das Styling eines User-Interface mit einer .css-Datei zu definieren

Eine Interoperabilität mit Swing ist vorgesehen, insbesondere wurde mit JavaFX 8 die Klasse SwingNode eingeführt, um grafische Elemente – welche mit Swing programmiert sind – als Knoten in einen JavaFX-scene-graph hinzufügen zu können.

# 2 Einleitung 


Grafische Benutzeroberflächen sind heute für Programmbenutzer selbstverständlich. Wir werden grafische Benutzeroberflächen mit Hilfe der JavaFX-Pakete programmieren. Dabei werden wir prinzipielle Konzepte kennenlernen, die andere Programmiersprachen und Bibliotheken auch benutzen wie Layout, grafische Bedienelemente (Buttons, Labels, usw.), sowie "Ereignisbehandlung und Binding." So werden Sie in der Lage sein, dieses Wissen in andere Umgebungen zu transferieren.
Objektorientierung 	

## 2.1 MVC Model-View-Controller

JavaFX ist objektorientiert in dem Sinne, dass es eine klare Trennung von Aufgaben ermöglicht. Genauer gesagt: eine JavaFX-Anwendung kann nach dem Muster (pattern) "Model-View-Controller" gegliedert werden. Im Teil "Model" befindet sich die Darstellung der Daten, z. B. die Hotel-Klasse, wenn die Anwendung eine Liste von Hotels darstellt. Im Teil "View" befindet sich die statische grafische Darstellung der Anwendung. Im Teil "Controller" befindet sich die Steuerung der grafischen Elemente, z. B. was bewirkt das Klicken eines Buttons für die Daten? Diese drei Teile werden wir in drei Pakete gliedern. Darüber hinaus werden wir ein Paket für Ressourcen einführen, wenn die Anwendung weitere Dateien benötigt, wie z. B. Bilder.

![](image/Pasted%20image%2020230512123819.png)

Model-View-Controller  	 
![](image/Pasted%20image%2020230504143355.png)


## 2.2 Grafische Darstellung (.fxml file und SceneBuilder)
Die JavaFX-Umgebung unterstützt Programmierer bei dieser Trennung. Die statische grafische Darstellung der Anwendung kann mit einem grafischen Editor, dem SceneBuilder, erstellt werden. Das Ergebnis wird in eine besondere XML-Datei mit dem Suffix .fxml gespeichert. Des Weiteren können die Bedienelemente aus dem Paket javafx.scene.control durch eine besondere .css-Datei "geskinnt" werden; Eigenschaften eines Buttons wie Farbe, Schrift usw. können in der .css-Datei festgelegt werden, getrennt vom Element Button selbst.

In diesen drei Kapiteln werden wir aber die statische grafische Darstellung in einem eigenen Paket programmieren und nicht mit dem SceneBuilder zeichnen. Der Grund dafür ist didaktisch – Programmieren lernt man durch viel Übung. Durch die Programmierung der grafischen Darstellung werden Sie mehr Programmierpraxis gewinnen und den Aufbau der JavaFX-Bibliothek viel gründlicher kennenlernen.



# 3 Interaktion durch bindings oder Ereignisbehandlung

![](image/Pasted%20image%2020230512124457.png)


# 4 JavaFX 特性

-   **Java API**：JavaFX 使用 java 开发，能够调用丰富的 Java 接口，同时对使用 JVM 的编程语言支持友好，如 JRuby、Scala。
-   **FXML**：JavaFX 中使用 fxml （基于 xml 的描述标记语言）来定义 UI 界面，使用 java 编码程序逻辑，实现展示和逻辑分离。
-   **WebView**：使用 WebKitHTML 技术在 JavaFX 应用中集成 web 组件，支持 heml5 特性，支持 JavaScript 调用 Java API。
-   **Swing 互操作性**：在 JavaFX 中可直接使用 Swing，使用 `SwingNode` 类将 Swing 内容嵌入到 JavaFX 程序中。
-   **CSS**：JavaFX 支持内嵌样式和使用 css 定义 UI 样式。
-   **3D图形功能**：支持 Shape3D（Box、Cylinder、MeshView、Sphere 子类）、SubScene、Material、PickResult、LightBase、SceneAntialiasing。
-   **Canvas**：使用 Canvas API，支持在 javaFX 场景上绘制。
-   **打印**：在 Java SE 8 的 `javafx.print` 包中提供了操作打印机的接口。
-   **富文本**：支持增强文本，包括双向文本和复杂的文本脚本，如多行、多风格文本。
-   **多点触控**：支持多点触控操作。
-   **Hi-DPI**：JavaFX 8 已支持 Hi-DPI 显示。
-   **硬件加速图形管道**：JavaFX 支持图形显卡加速渲染。
-   **高性能媒体引擎**：提供基于 GStreamer 的稳定、低延迟的媒体框架。
-   **自包含应用部署模式**：JavaFX程序可包括所有需要的资源及 Java、JavaFX 运行时的私有副本，可作为本机可安装包分发，并提供与该操作系统本机应用程序相同的安装和启动体验。

# 5 Aufbau JavaFX 程序基本结构

![](image/Pasted%20image%2020230504152512.png)

JavaFX 程序结构内容包括舞台（Stage）、场景（Scene）、容器（Container）、布局（Layout）和控件（Controls）。

![](http://blog-images.qiniu.wqf31415.xyz/javafx_program_frame.jpg)


## 5.1 Class `javafx.application.Application`

JavaFX—Application: https://www.cnblogs.com/DirWang/articles/11263615.html

Eine JavaFX-Anwendung ist eine Unterklasse (subclass) von `javafx.application.Application` und muss die Methode start überschreiben. Das Vokabular ist dem Theater entliehen. Ein Theaterstück wird auf einer Bühne gespielt. Entsprechend ist der Parameter der Methode start vom Typ Stage, und entspricht dem Hauptfenster, das die grafische Oberfläche zeigt. Ein Theaterstück hat mindestens eine Szene. 
![](image/Pasted%20image%2020230504144221.png)

1.Application是JavaFX程序的入口，任何javafx应用程序程序都要继承该类并重写start()方法
```java
public class TsetStage extends Application{
public void start(Stage primaryStage) throws Exception {
}
}
```

2  
- 通过main()执行Application的launch(String str)方法，当然launch(String str)方法不传入任何值也是可以执行的.launch(String str)方法会默认执行本类下的init()、start()、stop()方法。
	- 执行下面的main()方法后显示顺序为： 这是初始化方法➡这是start()方法➡这是stop()方法➡这是main()方法。
- 可以看出只有当stop()方法（即Application应用程序被正常关闭）被调用时（也可以是图形界面右上角的❌被点击时），launch()方法才执行完毕。（通过Platform.exit()退出界面也是属于正常退出）
- 通过IDE控制台终止应用程序属于非正常终止，它是通过关闭JVM从而关闭应用程序的。
- 另外线程方面，执行main()方法时是主线程，当调用init()方法时是JavaFX-Launcher线程，当调用start()方法时是JavaFX Application Thread线程，当调用stop()方法时依然是JavaFX Application Thread线程

- start()
	- 打开窗口时候, 运行的程序
- init()
	-  打开窗口时候, 要同步运行的程序, 比如说 数据库连接等等
- stop()
	- 关闭窗口的时候 运行的程序


```java
import javafx.application.Application;
public class TsetStage extends Application{
    public static void main(String[] args) {
        launch(args);//也可以写作launch()
        System.out.println("这是main方法");
}
 @Override
       public void init(){
       System.out.println("这是初始化方法");
    }
    @Overrid
    public void start(Stage primaryStage) throws Exception {
    priamryStage.show();
    System.out.println("这是start()方法");
}
    @Override
    public void stop() throws Exception {
        super.stop();
        System.out.println("这个是stop()方法");
}
}
```

![](image/Pasted%20image%2020230504162531.png)

3

.也可以在其他类中的main()方法中调用launch()方法，但要传入相应的参数，第一个参数指定一个Application的子类从而调用该类下的init()、start()、stop()方法，第二个参数制定一个String类型的值。

当然同上一个launch(String str)方法，该launch(? extends Application,String str)也可以不指定String类型的值。
复制代码
```java
import javafx.application.Application
public class TestStage2{
public static void main(String args[]){
Application.launch(TestStage.class,args);//也可以写作Application.luanch(TestStage.class)
}
}
```


4 
Application类下的getHostServices()方法，该方法返回一个HostServices实例，该实例的showDocument()方法可以指定一个网站地址或者URI
从而以系统默认的浏览器打开该URI。geDocumentBase()返回一个String类型的当前文档所在的路径。

```java
import javafx.application.Application;
import javafx.application.HostServices;
public class TsetStage extends Application{
    public static void main(String[] args) {
        launch(args);
}
 @Override
    public void start(Stage primaryStage) throws Exception {
    HostServices hostServices = getHostServices();
    hostServices.showDocument("www.baidu.com");
    System.out.println(hostServices.getDocumentBase());
}
}
```

## 5.2 Class javafx.stage.Stage

Attribute: 
- Title
- icon: 窗口左上角的图标
- resiziable: 能否改变窗口大小
- x,y,width,height: 设置坐标和宽高. 系统默认在中间显示 
- StageStyle: 窗口样式
	- ![](image/Pasted%20image%2020230504164411.png)
- Modality
	- 默认为 Modality.none 非模式: 各个窗口之间的 打开, 不印象其他窗口的使用
	- Modality.Application_MODAL: 当这个窗口开启的时候, 其他窗口是禁用的. 父窗口禁用(无法点击)
	- Modality.WINDOWS_MODAL: 当这个窗口开启的时候, 其他窗口是不禁用的, 只有他的父窗口禁用(无法点击)
- event

例子: 

![](image/Pasted%20image%2020230505183925.png)

Modality 的例子:
![](image/Pasted%20image%2020230505183339.png)

event的例子: 
- Platform.setImplicitExit(false): 点击 x 后窗口不会被关闭
- 弹出窗口, 问你是否要关闭
	- Platfor,.exit(): 退出程序加关闭窗口. 换成 primaryStage.close() 仅仅只是关闭窗口, 不退出程序
	- ![](image/Pasted%20image%2020230505184441.png)

## 5.3 Class javafx.scene.Scene

Stage -> Scene -> 内涵多个组件 

Warum ist die Bibliothek so programmiert, dass die Klasse javafx.scene.Scene sein muss? Warum wird das GridPane-Objekt im Programm Main nicht direkt in das Stage-Objekt hinzugefügt? Um den Grund zu verstehen, muss man den Aufbau der Bibliothek ein wenig vertiefen. Schauen wir uns den kleinsten Konstruktor (mit einem einzigen Parameter) der Klasse [javafx.scene.Scene](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/Scene.html#Scene-javafx.scene.Parent-) an.
[Scene](https://openjfx.io/javadoc/11/javafx.graphics/javafx/scene/Scene.html#<init>(javafx.scene.Parent))([Parent](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/Parent.html) root):  Creates a Scene for a specific root Node.

Das Erzeugen eines Scene-Objekts braucht immer einen Parameter vom Typ [javafx.scene.Parent](https://openjfx.io/javadoc/11/javafx.graphics/javafx/scene/Parent.html), das Parent-Objekt ist die Wurzel des scene graph. Die abstrakte Parent-Klasse hat drei direkte Unterklassen. 


Im Programm Hotel01 wird mit folgender Anweisung eine Szene erzeugt.

```java
Scene scene = new Scene (root,400,400);
```

1 
![](image/Pasted%20image%2020230505190049.png)

2 可以来回切换场景
并且 第二个场景中 鼠标的 cursor 样子不一样
![](image/Pasted%20image%2020230505190302.png)

![](image/Pasted%20image%2020230505190253.png)

![](image/Pasted%20image%2020230505190429.png)

### 5.3.1 Class javafx.scene.Parent

-   [java.lang.Object](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Object.html)
    -   [javafx.scene.Node](https://openjfx.io/javadoc/11/javafx.graphics/javafx/scene/Node.html)
        -   [javafx.scene.Parent](https://openjfx.io/javadoc/11/javafx.graphics/javafx/scene/Parent.html)
-   All Implemented Interfaces: [Styleable](https://openjfx.io/javadoc/11/javafx.graphics/javafx/css/Styleable.html), [EventTarget](https://openjfx.io/javadoc/11/javafx.base/javafx/event/EventTarget.html)
- Direct Known Subclasses: [Group](https://openjfx.io/javadoc/11/javafx.graphics/javafx/scene/Group.html), [Region](https://openjfx.io/javadoc/11/javafx.graphics/javafx/scene/layout/Region.html), [WebView](https://openjfx.io/javadoc/11/javafx.web/javafx/scene/web/WebView.html)

Diese drei Unterklassen sind für prinzipiell unterschiedliche grafische Anwendungen vorgesehen. Kurz erklärt wird eine Group-Wurzel (root) für geometrische Anwendungen gewählt, eine Region-Wurzel für Formulare (GridPane ist eine Unterklasse von Region) und WebView für Anwendungen, welche Inhalte aus dem Web darstellen sollen. Diese drei Unterklassen haben ganz andere Anforderungen an die Grafik, wie z. B. was passiert, wenn das Hauptfenster vergrößert oder verkleinert wird. Die Scene-Klasse kapselt diese Anforderungen und Möglichkeiten für das Hauptfenster ein (Stage-Objekt), das vom Betriebssystem zur Verfügung gestellt wird.

## 5.4 例子

1 
```java
import javafX.appliction.Application
import javafx.stage.Stage

public class Main extends Application {

    @Override
    public void start(Stage primaryStage) throws Exception {
        try {
            primaryStage.show();
        } catch(Exception e) {
            e.printStackTrace();
        }
    }
    
    public static void main(String[] args) {
        Application.launch(args);
    }
}
```


2
Was ist die nächste kleinste mögliche JavaFX-Anwendung? Eine Bühne mit einer Szene ohne Schauspieler, aber mit Inszenierung - siehe Programm Main unten. Oder mit anderen Worten, ein Hauptfenster mit einem Objekt vom Typ Scene; ein Konstruktor der Klasse Scene hat mindestens einen Parameter vom Typ javax.scene.Parent. Die Klasse Parent ist abstrakt; die direkten Unterklassen (Group, Region, Webview) sind für die Inszenierung (Anordnung der grafischen Komponenten) verantwortlich. Die Klasse GridPane - die hier benutzt wird - ist eine indirekte Unterklasse (subclass) von Region.
```java
 	

public class Main extends Application {
    @Override
    public void start(Stage primaryStage) throws Exception {
        try {
            GridPane root = new GridPane();
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

3
```java
import javafX.appliction.Application
import javafx.stage.Stage

public class Main extends Application {

    @Override
    public void start(Stage primaryStage) throws Exception {
	    Label label = new Label("Hello World!")
	    borderPane pane = new BorderPane(label);
		stage.setTitle("窗口");
		Stage.show();
    }
    
    public static void main(String[] args) {
        Application.launch(args);
    }
}
```

![](image/Pasted%20image%2020230504160910.png)


4

![](image/Pasted%20image%2020230504163005.png)

# 6 Layout (Paket javafx.scene.layout)


Ein Layout bestimmt die Größe und die Positionen der grafischen Komponenten (auch Knoten (node) genannt) zueinander, die es enthält. In diesem Abschnitt sehen wir Klassen aus dem Paket javafx.scene.layout, die alle Unterklassen von  javafx.scene.layout.Region sind. Das heißt, sie eignen sich eher für Anwendungen wie Formulare. In diesem Kapitel beschränken wir uns auf: GridPane, VBox und Hbox, StackPane und BorderPane. 


## 6.1 Layout GridPane


![](image/Pasted%20image%2020230504144739.png)

```java
package application;

import javafx.application.Application;
import javafx.geometry.HPos;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.geometry.VPos;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.image.Image;
import javafx.scene.image.ImageView;
import javafx.scene.layout.GridPane;
import javafx.stage.Stage;

/**
 * This class illustrates GridLayout.
 * @author agathe merceron
 *
 */
public class HotelGrid extends Application {

    @Override
    public void start(Stage primaryStage) {
        try {
            GridPane root = new GridPane();
            //components will be drawn from the right
            root.setAlignment(Pos.TOP_RIGHT);
            // lines are made visible
            // to understand how they are drawn
            root.setGridLinesVisible(true);
            root.setHgap(10);
            root.setVgap(10);

            // the address of the hotel is displayed via a Label-Object
            // to be able to use the setPadding method
            Label text = new Label();
            text.setText("ibis Styles Hotel Berlin Mitte\n"
                    + "Brunnenstrasse 1-2\n" +"10119 Berlin");
            //to add some space around the label
            text.setPadding(new Insets(25, 25, 25, 25));
            text.setAlignment(Pos.BASELINE_RIGHT);
            // the label extends over 2 columns and 1 row
            root.add(text, 0, 0, 2, 1);


            Image image = new Image (getClass().getResource(
                    "/resources/ibisMitteBerlin.jpg").toString());
            ImageView imageview = new ImageView(image);
            root.add(imageview, 2, 1);

            Label labelrating = new Label("current rating:");
            root.add(labelrating, 0, 1);
            Label currentrating = new Label("3.75");
            // the label will appear in the top right corner
            GridPane.setHalignment(currentrating, HPos.RIGHT);
            GridPane.setValignment(currentrating, VPos.BASELINE);
            root.add(currentrating, 1, 1);

            Button button = new Button("Enter your rating");
            //put some space around the text of the button
            button.setPadding(new Insets(25, 25, 25, 25));

            root.add(button, 2, 3);
            Scene scene = new Scene(root,400,400);
            primaryStage.setTitle("Ibis Mitte");
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


## 6.2 VBox und HBox

Die Klassen `javafx.scene.layout.VBox `und `javafx.scene.layout.HBox` erlauben eine flexible Gestaltung  der grafischen Komponenten, vor allem wenn sie kombiniert werden. In einem HBox-Objekt stehen die Elemente nebeneinander, während sie in einem VBox-Objekt übereinander stehen. Die Reihenfolge ist bestimmt durch die Reihenfolge, in der sie hinzugefügt wurden.


Das Beispiel HotelVBoxHBox zeigt eine ähnliche grafische Oberfläche wie das Programm HotelGrid, diesmal realisiert mit einer VBox für die senkrechte Anordnung der Elemente, kombiniert mit der Klasse Hbox für die waagerechte Anordnung der zwei Label-Objekte und des Bildes. 

Beachten Sie dabei, wie die Elemente hinzugefügt werden: root.getChildren().add(text); die Methode getChildren liefert einer Liste aller Elemente, welche root als Eltern-Knoten haben. Die Methode add fügt das Element text am Ende dieser Liste hinzu. 


```java

package application;

import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.image.Image;
import javafx.scene.image.ImageView;
import javafx.scene.layout.HBox;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

/**
 * This class illustrates HBox combined with VBox.
 * @author agathe merceron
 *
 */
public class HotelVBoxHBox extends Application {
    
    @Override
    public void start(Stage primaryStage) {
        try {
            VBox root = new VBox();
            //components will be drawn from the right
            root.setAlignment(Pos.TOP_RIGHT);
            
            
            
            // the address of the hotel is displayed via a Label-Object
            // to be able to use the setPadding method
            Label text = new Label();
            text.setText("ibis Styles Hotel Berlin Mitte\n"
                    + "Brunnenstrasse 1-2\n" +"10119 Berlin");
            //to add some space around the label
            text.setPadding(new Insets(25, 25, 25, 25));
            root.getChildren().add(text);
            
            HBox second = new HBox();
            //inside second all components will be placed in the middle
            //and on the bottom
            second.setAlignment(Pos.BASELINE_CENTER);
            
            Image image = new Image (getClass().getResource(
                    "/resources/ibisMitteBerlin.jpg").toString());
            ImageView imageview = new ImageView(image);
            Label labelrating = new Label("current rating:");
            //puts some space around this label
            labelrating.setPadding(new Insets(25, 25, 25, 25));
            Label currentrating = new Label("3.75");
            
            second.getChildren().addAll(
                    labelrating, currentrating, imageview);
            
            Button button = new Button("Enter your rating");
            //puts some space around the text of the button
            button.setPadding(new Insets(25, 25, 25, 25));
            
            root.getChildren().addAll(second, button);
            
            Scene scene = new Scene(root,400,400);
            primaryStage.setTitle("Ibis Mitte with VBox and HBox");
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


![](image/Pasted%20image%2020230504145000.png)


## 6.3 Layout StackPane

 	

Mit der Klasse StackPane werden die grafischen Komponenten übereinander gestapelt, in der Reihenfolge, wie sie hinzugefügt werden. Das Programm HotelStack lässt das Bild des Hotels über zwei Kreisen erscheinen. Ähnlich wie mit VBox und HBox werden Elemente in die Liste der Kinder-Knoten vom StackPane-Objekt hinzugefügt. Mit der Methode addAll können mehrere Elemente in der Reihenfolge, wie sie erscheinen, zugleich in diese Liste hinzugefügt werden. 

![](image/Pasted%20image%2020230504145137.png)

```java
package application;

import javafx.scene.paint.Color;
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.image.Image;
import javafx.scene.image.ImageView;
import javafx.scene.layout.StackPane;
import javafx.scene.shape.Circle;
import javafx.stage.Stage;

/**
 * This class illustrates StackPane.
 * @author agathe merceron
 *
 */

public class HotelStack extends Application {
    
    @Override
    public void start(Stage primaryStage) {
        try {
            StackPane root = new StackPane();
            

            Circle c1 = new Circle(100);
            c1.setFill(Color.RED);

            Circle c2 = new Circle(80);
            root.getChildren().addAll(c1, c2);
            
            Image image = new Image (getClass().
                getResource("/resources/ibisMitteBerlin.jpg").toString());
            ImageView imageview = new ImageView(image);
            root.getChildren().add(imageview);
                
            Scene scene = new Scene(root,400,400);
            primaryStage.setTitle("Hotel Stack");
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



## 6.4 Beipsiel: GrindPane 和 StackPane kombiniert 

Wie wir schon gesehen haben, ist durch Vererbung ein StackPane-Objekt auch ein Node-Objekt und kann somit als grafische Komponente zu einem anderen Layout-Objekt hinzugefügt werden, wie das Programm HotelGridStack zeigt.
 
Die Methode embedPicture gibt ein StackPane-Objekt zurück, das die gestapelten Kreise und das Bild enthält. In der Zeile 51 wird dieses Objekt in Spalte 2 und Zeile 0 des Wurzel-Elementes hinzugefügt: root.add(embedPicture(this), 2, 0);


HotelGridStack
```java
 
    //help method to embed the picture of the hotel on top of two circles
    private static StackPane embedPicture(HotelGridStack o){
        StackPane hotel = new StackPane();
        Circle c1 = new Circle(100);
        c1.setFill(Color.RED);
        Circle c2 = new Circle(80);
        hotel.getChildren().addAll(c1, c2);
        Image image = new Image (o.getClass().
                getResource("/resources/ibisMitteBerlin.jpg").toString());
        ImageView imageview = new ImageView(image);
        hotel.getChildren().add(imageview);
        return hotel;
    }
}
```

![](image/Pasted%20image%2020230504145352.png)


## 6.5 Layout BorderPane

Das Layout BorderPane teilt die Fläche in fünf Bereiche: oben, unten, links, rechts und Mitte, wie die Abbildung  zeigt:

![](image/Pasted%20image%2020230504145437.png)


例子: Das Programm ExampleBorderPane kombiniert die Klassen BorderPane und HBox, um vier Label-Objekte Namens top, left, right und bottom an den Seiten des Fensters zu platzieren und drei Button-Objekte - button1, button2 und button3 - in der Mitte.

```java
package application;

import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.HBox;
import javafx.stage.Stage;

/**
 * This class illustrates BorderPane combined with HBox.
 * @author agathe merceron
 *
 */
public class ExampleBorderPane extends Application {
    
    @Override
    public void start(Stage primaryStage) {
        try {
            BorderPane root = new BorderPane();
    
            Label top = new Label("label top");
            top.setPadding(new Insets(25, 25, 25, 25));
            root.setTop(top);
            
            Label left = new Label("label left");
            left.setPadding(new Insets(25, 25, 25, 25));
            root.setLeft(left);
            //put this label in the center of the left side
            BorderPane.setAlignment(left, Pos.CENTER);
            
            Label right = new Label("label right");
            right.setPadding(new Insets(25, 25, 25, 25));
            root.setRight(right);
            
            Label bottom = new Label("label bottom");
            bottom.setPadding(new Insets(25, 25, 25, 25));
            root.setBottom(bottom);
            //put this label in the center above the bottom line
            BorderPane.setAlignment(bottom, Pos.CENTER);
            
            HBox center = new HBox();
            //inside second all components will be placed in the middle
            //and on the bottom
            center.setAlignment(Pos.BASELINE_CENTER);
            Button button1 = new Button("button1");
            //puts some space around the text of the button
            button1.setPadding(new Insets(25, 25, 25, 25));
            Button button2 = new Button("button1");
            //puts some space around the text of the button
            button2.setPadding(new Insets(25, 25, 25, 25));
            Button button3 = new Button("button1");
            //puts some space around the text of the button
            button3.setPadding(new Insets(25, 25, 25, 25));
            
            center.getChildren().addAll(button1, button2, button3);
            root.setCenter(center);
            
            Scene scene = new Scene(root,400,400);
            primaryStage.setTitle("Example with BorderPane");
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

![](image/Pasted%20image%2020230504145512.png)



## 6.6 Ein Beispiel mit model-view und der Klasse Scrollpane



# 7 Zusammenfassung


- Syntaktisch betrachtet ist eine JavaFX-Anwendung eine Unterklasse der Klasse` javafx.application.Application`. Die Methode start muss überschrieben werden.
- Das Vokabular von FX ist dem Theater entliehen. Die Methode start hat einen Parameter vom Typ `javafx.stage.Stage`, der das Hauptfenster darstellt. Mit Hilfe der Methode `setScene` muss ein` javafx.scene.Scene` dem `Stage`-Objekt assoziiert werden.
- Das `Scene`-Objekt fungiert als Container
- Die Klasse `javafx.scene.layout.GridPane` erlaubt es, grafische Elemente mit der Angabe von Spalten und Zeilen im Fenster zu positionieren
- Mit Klassen aus dem Paket` javafx.scene.control` können Formulare gestaltet werden.
- Die Trennung in `model` und `view` erlaubt, dass die gleiche Logik mit einer anderen grafischen Darstellung wieder verwendet werden kann.
