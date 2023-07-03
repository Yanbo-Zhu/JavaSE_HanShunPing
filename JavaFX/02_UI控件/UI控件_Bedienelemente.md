
# 1 Paket javafx.scene.control


In diesem Abschnitt sehen wir einige Klassen aus dem Paket (package) javafx.scene.control, die für Formulare gängige Bedienelemente bereitstellen. Wir haben schon die Klassen Button und Label kennengelernt. 
In diesem Abschnitt sehen wir Klassen für die Eingabe von Text: TextField, TextArea, PasswordField und die Klassen Tooltip und ScrollPane.

Das Paket bietet natürlich noch viel mehr; ein partieller Überblick wird am Ende dieses Abschnittes vorgestellt. Die Klassen für Bedienelemente sind direkte oder indirekte Unterklassen der abstrakten Klasse javafx.scene.control.Control.

# 2 Class Node

Node 类 为抽象类
所有UI 控件 都来自于 Node 类

![](image/Pasted%20image%2020230505192151.png)

- parent: 这个 UI节点的父节点
- scene: 这个UI节点所在的 场景

## 2.1 Button 

```java
Button button = new Button("Enter your rating");
root.add(button, 0, 3);

button.setOnAction(e -> enterRating(l, myrating));
```
 
## 2.2 Label 

```java
 Label labelrating = new Label("current rating:");
//puts labelrating column 0 row 1
root.add(labelrating, 0, 1);
Label currentrating = new Label("3.75");
root.add(currentrating, 1, 1);
```

## 2.3 TextField und TextArea

Die Klassen TextField und TextArea erlauben beide, Texte einzugeben. Die Klasse TextField ist für kurze Eingaben in einer Zeile, wie man sie typischerweise für Namen usw. braucht
Die Klasse TextArea ist für mehrzeilige Eingaben vorgesehen.

```java
TextField myrating = new TextField("2");
myrating.setVisible(false);
//binds the text of currentrating to the text of myrating
currentrating.textProperty().bind(myrating.textProperty());
root.add(myrating, 1, 5);
```



![](image/Pasted%20image%2020230702005109.png)

```java
VBox root = new VBox();
Label userlabel = new Label("Enter your last name");
TextField usertext = new TextField();
HBox hbox1 = new HBox();
//Space between hbox and window
hbox1.setPadding(new Insets(15, 15, 15, 15));
//space between the elements in the hbox
hbox1.setSpacing(15);
hbox1.getChildren().addAll(userlabel, usertext);
root.getChildren().add(hbox1);



HBox hbox2 = new HBox();
//space between hbox and window
hbox2.setPadding(new Insets(15, 15, 15, 15));
//space between the elements in the hbox
hbox2.setSpacing(15);
hbox2.getChildren().addAll(psswd, psswdF);
root.getChildren().add(hbox2);

Label address = new Label("Enter your address street"+
"\n zip code and city \n country");
TextArea addresstxt = new TextArea();
//how big the area should be
//when more is entered scrolling begins automatically
addresstxt.setPrefColumnCount(12);
addresstxt.setPrefRowCount(3);
HBox hbox3 = new HBox();
//space between hbox and window
hbox3.setPadding(new Insets(15, 15, 15, 15));
//space between the elements in the hbox
hbox3.setSpacing(15);
hbox3.getChildren().addAll(address, addresstxt);
root.getChildren().add(hbox3);

```


## 2.4 PasswordField and Tooltip

```java

Label psswd = new Label("password");
// when the program runs
//wait a while for the tooltip to be shown
Tooltip ptool = new Tooltip("password should have at least 8 characters");
psswd.setTooltip(ptool);

// a Tooltip can also be associated to any Control-Object like this:
// Tooltip.install(psswd, ptool);
PasswordField psswdF = new PasswordField();
psswdF.setPromptText("password");

HBox hbox2 = new HBox();
//space between hbox and window
hbox2.setPadding(new Insets(15, 15, 15, 15));
//space between the elements in the hbox
hbox2.setSpacing(15);
hbox2.getChildren().addAll(psswd, psswdF);
root.getChildren().add(hbox2);

```

![](image/Pasted%20image%2020230702005109.png)



## 2.5 MenuBar, Menu, MenuItem, CheckMenuItem und RadioMenuItem
In diesem Abschnitt werden weitere wichtige Klassen kurz vorgestellt. Eine vollständige Anleitung über alle Klassen finden Sie in der Java-Dokumentation.
http://docs.oracle.com/javafx

1 Klasse  *MenuBar, Menu, MenuItem, CheckMenuItem und RadioMenuItem*
Die Klassen *MenuBar, Menu, MenuItem, CheckMenuItem und RadioMenuItem* erlauben Menüs zu gestalten. 
- In ein MenuBar-Objekt können mehrere Menu-Objekte hinzugefügt werden. 
- Ein Menu-Objekt wiederum enthält MenuItem-Objekte.
- Ein CheckMenuItem-Objekt ist ein MenuItem-Objekt, das mit einem Zeichen markiert wird, wenn es ausgewählt ist.
- Ein RadioMenuItem-Objekt ist wie ein CheckMenuItem-Objekt, es kann zusätzlich geschaltet werden. 

Ein Beispiel wird in der Lerneinheit "2DG - 2D-Grafik" vorgestellt.

## 2.6 `ComboBox<T>` , RadioButton, CheckBox

2 Klasse `ComboBox<T>`
Die generische Klasse `ComboBox<T>` erlaubt, eine Auswahlliste zu erstellen. Dabei wird häufig der Typ String für T benutzt.

3  Klasse RadioButton
Mit der Klasse RadioButton können Items selektiert werden. 
enn mehrere RadioButton-Objekte erstellt werden, kann mit der Klasse ToggleGroup erreicht werden, dass nur ein einziges RadioButton-Objekt selektiert wird. 

4 Klasse CheckBox
Die Klasse CheckBox ist ähnlich wie RadioButton. Es können aber immer so viele CheckBox-Objekte selektiert werden wie gewünscht.

## 2.7 DatePicker und Klasse ColorPicker
5 Klasse DatePicker und Klasse ColorPicker
Schließlich kann mit der Klasse DatePicker ein Datum eingegeben und mit der Klasse ColorPicker eine Farbe ausgewählt werden.

## 2.8 例子

![](image/Pasted%20image%2020230505192425.png)

# 3 UI控件的属性绑定和属性监听

## 3.1 属性绑定 bind()
主要是用 interface Property 的 子类, 像是 LongProperty, DoubleProperty 

例子:  利用 绑定 圆的中心点这个属性 , 使得 圆 的中心点 根据窗口的大小 变化, 始终在 窗口的中心点 
![](image/Pasted%20image%2020230505193330.png)

![](image/Pasted%20image%2020230505193340.png)


## 3.2 属性监听 addListener()
只是属性绑定 不能够适应复杂的 场景
通过属性监听可以实现 在 场景变化的时候 自动做出一些反应


例子: 窗口 x轴大小改变后 自动 在 输出语句
窗口 y轴大小改变后 不会自动 在 输出语句

![](image/Pasted%20image%2020230505193718.png)

![](image/Pasted%20image%2020230505193803.png)