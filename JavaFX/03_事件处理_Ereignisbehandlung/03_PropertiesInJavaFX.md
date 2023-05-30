
 
Properties in JavaFX sind Attribute eines besonderen Typs: der Typ implementiert das Interface `javafx.beans.property.Property<T>`.  Das Interface definiert die unten gelisteten Methoden, welche erlauben, Properties zu binden (Methoden bind und bindBidirectional) und loszubinden (Methoden unbind und unbindBidirectional). Eine Bindung kann in zwei Richtungen funktionieren: Änderungen in einem Property-Objekt werden gleich im anderen gespiegelt, und umgekehrt, siehe Methode bindBidirectional. 

![](image/Pasted%20image%2020230529232106.png)


 	
Damit Änderungen propagiert werden, müssen sie zuerst überhaupt möglich sein und beobachtet werden. Dafür ist das Interface `Property<T>` selbst eine Subinterface von mehreren Interfaces, darunter` javafx.beans.value.WritableValue<T>` und` javafx.beans.value.ObservableValue<T>. `Das Interface` WritableValue<T> `mit den Methoden getValue und setValue erlaubt den Wert des Attributes abzufragen und zu verändern. Die Methoden sind:

![](image/Pasted%20image%2020230529232225.png)

Das Interface `ObservableValue<T>` beobachtet Änderungen im Wert eines Attributes und propagiert sie. Dieses Interface enthält folgende Methoden:
![](image/Pasted%20image%2020230529232235.png)

 	
Properties in JavaFX, wie auch in JavaBeans folgen einer strikten Namenskonvention. Als Beispiel betrachten wir das Attribut text der Klasse TextField. Ziel dieses Attributes ist es, den Text zu erhalten, den ein Benutzer eintippt. Der Typ dieses Attributes ist nicht einfach String, sondern javafx.beans.property.StringProperty. Die Klasse StringProperty ist eine Wrapper-Klasse für String und implementiert das Interface Property. Dadurch werden Methoden zur Verfügung gestellt, welche Änderungen dieses Attributes beobachten und propagieren können.

Die Methode, welche den Text selbst zurückgibt, heißt: getText; der Rückgabetyp ist String. Die Methode, um den Text selbst zu verändern, heißt setText. Diese Methode hat einen Parameter vom Typ String und keine Rückgabe. Die Methode, welche das Property-Objekt selbst zurückgibt, heißt textProperty und der Typ der Rückgabetyp ist StringProperty. Diese Methode ist für Binding und den Aufruf der Methode bind nützlich. Der folgende Code zeigt am Beispiel String, wie eine Property-Klasse im Allgemeinen definiert wird:

```java
// Define a variable to store a string as property


    private StringProperty text;
 
    // Define a getter for the property's value
    public final String getText(){
    //code of the method
    }
 
    // Define a setter for the property's value
    public final void setText(String text){
    //code of the method
    }
 
     // Define a getter for the property itself
    public StringProperty textProperty() {
    //code of the method
    }
}
```



Im Paket javafx.beans.property stehen Klassen zur Verfügung, die gängige Typen in Property-Typen verpacken. JavaFX verwendet konsequent Properties. Die Dokumentation der Klassen wie GridLayout, Button oder Rectangle beginnt immer mit einer Liste der Properties der Klasse selbst und der Oberklassen. Schauen Sie sich die Dokumentation der Klasse javafx.scene.layout.GridPane an:

https://openjfx.io/javadoc/11/javafx.graphics/javafx/scene/layout/GridPane.html 

Die Attribute hgap und vgap zum Beispiel, die den Raum zwischen zwei Spalten oder Zeilen festlegen, sind nicht vom primitiven Typ double, sondern vom Typ DoubleProperty.
  	

Es ist möglich, beispielsweise mit Hilfe der Pakete javafx.beans, javafx.beans.property eigene Klassen zu schreiben, dessen Attribute Properties sind. Ein gutes Beispiel finden Sie im Tutorial von Oracle:

Tutorial: Using JavaFX Properties and Binding
  	

Bis jetzt wurden nur Änderungen in einzelnen Objekten besprochen. Es ist auch möglich, mit Hilfe des Paketes javafx.collections Änderungen in  Listen von Objekten zu beobachten und auf diese Veränderungen zu reagieren, unabhängig von der grafischen Oberfläche. Die Klasse javafx.collections.FXCollections hat die gleichen Funktionalitäten wie java.util.List<E> aus der Standard-Java-Bibliothek, ein Interface, das wir in der Lerneinheit über ArrayList<E> gesehen haben, liefert aber eine Liste, welche das Interface javafx.beans.Observable implementiert.

Als Einschränkung ist hier zu nennen, dass eine Anwendung, welche Klassen aus dem Paket javafx.collections benutzt, von der JavaFX-Bibliothek abhängig ist, nicht nur für die grafische Oberfläche sondern auch für die Logik oder für das Modell. Die JavaFX-Bibliothek ist nicht Teil der normalen Java-Umgebung. Die Beispiele, die wir bisher gezeigt haben, benutzen JavaFX nur für die grafische Oberfläche und nicht für das Modell. Das Modell könnte daher unverändert mit einer anderen grafischen Oberfläche funktionieren. Dies werden wir wegen der Wiederverwendbarkeit des Modells beibehalten. Wir werden lediglich die Properties nutzen, die JavaFX selbst für die Erstellung von grafischen Oberflächen zur Verfügung stellt. Wir werden keine eigene Klassen mit Properties in diesem Studienmodul erstellen.

