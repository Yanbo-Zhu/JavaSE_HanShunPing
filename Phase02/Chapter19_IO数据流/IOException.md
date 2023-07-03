 	

Alle Methoden dieser Lerneinheit sind so deklariert, dass sie Ausnahmen (exceptions) vom Typ IOException auslösen. Fehlerhafte Eingabe- und Ausgabeoperationen können so mit Hilfe dieser Klasse vom Programm aufgefangen werden.

Die Klasse IOException ist im Paket (package) java.io enthalten. Sie erbt ihre Attribute (fields) und Methoden von den Klassen Exception und Throwable aus dem Paket java.lang. 

![](image/Pasted%20image%2020230630174345.png)

![](image/Pasted%20image%2020230630173941.png)

```java
try {
    // Ein- und Ausgabe-Operationen werden programmiert
} catch (IOException e) {
    // Anweisungen, die ausgef�hrt werden, wenn 
    // Ausnahmesituation eintritt
}  



public static void main(String[] args) throws IOException {


```