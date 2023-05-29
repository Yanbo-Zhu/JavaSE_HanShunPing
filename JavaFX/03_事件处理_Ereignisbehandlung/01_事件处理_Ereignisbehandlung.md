
Interaktionen durch Bindings oder Ereignisbehandlung
- Bindings: Abhängigkeit zwischen zwei Properties. Bei Änderung der Quelleigenschaft, wird die Ziel-Eigenschaft automatisch aktualisiert. z.B. bei einem Schieberegler ändert sich sofort die Ansicht
- Ereignisbehandlung: Reaktion der Anwendung auf Benutzerinteraktionen - Ereignisse können durch den Controller verarbeitet werden
	-  das funktionale Interface `javafx.event.EventHandler<T extends Event>`
-` EventHandler<ActionEvent>`  oder  `EventHandler<MouseEvent>` and Methoden `setOnAction`

Action, Event and handler
![](image/Pasted%20image%2020230505233650.png)

- Node class 中包含了各种 Event
	- ![](image/Pasted%20image%2020230505233852.png)
	- 每个UI空间 也包含自己的 setOnAction()


# 1 例子 (Grundlagen der Ereignisbehandlung und Binding)

## 1.1 
1 点击 button, label 就会向上移动
![](image/Pasted%20image%2020230505235143.png)

![](image/Pasted%20image%2020230505235127.png)

## 1.2 

setOnkeyReleased(): 键盘释放 
按 keydown 键盘中向下箭头的 key, label 向下
![](image/Pasted%20image%2020230506000047.png)

## 1.3 

拖拽事件 
将一个文本拖进窗口中的 一个 文本框中. 然后松开鼠标, 然后 文本框中显示这个 文件的绝对路径 

![](image/Pasted%20image%2020230506000719.png)

![](image/Pasted%20image%2020230506000750.png)

## 1.4 GP2 教材上的

### 1.4.1 hotel010.java

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



