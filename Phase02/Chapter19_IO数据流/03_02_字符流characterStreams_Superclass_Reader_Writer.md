

# 1 总览 
Byteströme enthalten also ihre Daten (primitive Werte oder ganze Objekte) byteweise. Texte können zwar auch byteweise gespeichert werden (z. B. mit Hilfe der Methode DataOutputStream.writeChars()). Zeichenströme (Characterstreams) bieten aber zahlreiche zusätzliche Methoden an, mit denen Texte bequemer (z. B. zeilenweise) geschrieben und gelesen werden können.<br><br>Die beiden **abstrakten Klassen** aus dem Paket java.io<br><br>- Reader<br>- Writer<br><br>bilden die Grundlage für das Lesen und Schreiben von Text.<br><br>Die Klasse Reader ist ein Eingabestrom. Die Klasse Writer ist ein Ausgabestrom.<br><br>Character- bzw. zeichenorientierte Ströme werden verwendet, um Daten in einer von Menschen lesbaren Form zu lesen oder ausgeben zu können.|


## 1.1 flush()
Die Methode flush schreibt Daten, die in einem Puffer zwischengespeichert wurden, in den Ausgabestrom, der die Daten dann in das Ausgabeziel, in unserem Fall die Datei, schreibt.

## 1.2 close()
Die Methode close schließt den Datenstrom und gibt dadurch entsprechende Systemressourcen frei.

# 2 Writer

Die Klasse Writer ist eine abstrakte Klasse (abstract class). Sie stellt Methoden zur Verfügung, mit denen ein aus Unicode-Zeichen bestehender Datenstrom (stream) in eine Datei schreiben kann.


![](image/Pasted%20image%2020230425145650.png)

![](image/Pasted%20image%2020230425145718.png)


## 2.1 CharacterAusgabeStrom.java
Die Strings aus der Reihung textA werden zunächst in der Variable bWriter gepuffert und an osWriter weiter gegeben – immer, wenn dieser für neue Daten aufnahmefähig ist. osWriter wiederum leitet die Strings an foStream weiter, der sie in die Datei EinAusgabe.tmp auf der Festplatte schreibt. 
```java

import java.io.BufferedWriter;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;

/**
 * Klasse zum Schreiben von zeichenorientierten Ausgaben in eine Datei.
 *
 * @author skalt
 * @version 2.0, 07/2009
 */
class CharacterAusgabeStrom {

    /**
     * Main-Methode.
     *
     * @param args
     *            Uebergabeparameter
     * @throws IOException
     */
    public static void main(String[] args) throws IOException {
        // Dateiname
        String dateiName = "EinAusgabe.tmp";
        // Array mit Ausgabetext
        String[] textA = { "Hallo Tester!", "Wie geht es?", "Danke, gut!" };

        // Methode zum Testen des Ausgabestroms aufrufen
        testeCharacterAusgabeStrom(dateiName, textA);
    }

    /**
     * Methode zum Testen eines Character-Ausgabestroms.
     *
     * @param dateiName
     *            Name der Datei in die Daten geschrieben werden
     * @param textA
     *            zu schreibendes String-Array
     * @throws IOException
     */
    public static void testeCharacterAusgabeStrom(String dateiName,
            String[] textA) throws IOException {
        // Eine Datei und drei Ausgabestroeme erzeugen und miteinander
        // verbinden
        FileOutputStream foStream = new FileOutputStream(dateiName);
        OutputStreamWriter osWriter = new OutputStreamWriter(foStream);
        BufferedWriter bWriter = new BufferedWriter(osWriter);
        // Datenfluss: Programm --> bWriter --> osWriter --> foStream --> Datei

        // Ausgabetext zeilenweise (durch die Stroeme) in die Datei schreiben
        for (int i = 0; i < textA.length; i++) {
            bWriter.write(textA[i]);
            bWriter.newLine(); // Plattformabhaengig!
        }
        // Ausgabestrom schliessen
        bWriter.close();
    }
}
```

# 3 Reader


Die Klasse Reader ist ein auf Unicode-Zeichen basierender Eingabestrom. Sie ist abstrakt. Sie stellt Methoden zur Verfügung, mit denen ein aus Unicode-Zeichen bestehender Datenstrom von einer Datenquelle eingelesen werden kann.

![](image/Pasted%20image%2020230425145923.png)

![](image/Pasted%20image%2020230425150045.png)

Die Subklassen der Klasse Reader lassen sich in Klassen einteilen, die angeben,<br><br>- von wo gelesen werden soll (z. B. FileReader, InputStreamReader, StringReader)<br>- und wie der Einlesevorgang erfolgen soll (z. B. BufferedReader, LineNumberReader).|


## 3.1 例子

CharacterEingabeStrom.java
```java
package de.vfh.gp2.dud;

import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;

/**
 * Klasse zum Testen von zeichenorientierter Eingabe aus einer Datei.
 *
 * @author skalt
 * @version 2.0, 07/2009
 */
public class CharacterEingabeStrom {

    /**
     * Main-Methode.
     *
     * @param args
     *            Uebergabeparameter
     * @throws IOException
     */
    public static void main(String[] args) throws IOException {
        // Dateiname der einzulesenden Datei
        String dateiName = "EinAusgabe.tmp";

        // Methode zum Testen des Ausgabestroms aufrufen
        testeCharacterEingabeStrom(dateiName);
    }

    /**
     * Methode zum Testen eines Character-Eingabestroms.
     *
     * @param dateiName
     *            Name der Datei aus der Daten gelesen werden
     * @throws IOException
     */
    public static void testeCharacterEingabeStrom(String dateiName)
            throws IOException {
        // Drei Eingabestroeme erzeugen und mit der Datei verbinden:
        FileInputStream fiStream = new FileInputStream(dateiName);
        InputStreamReader isReader = new InputStreamReader(fiStream);
        BufferedReader bReader = new BufferedReader(isReader);
        // Datenfluss: Programm <-- bReader <-- isReader <-- fiStream <-- Datei

        // Alle Zeilen (durch die Stroeme) aus der Datei lesen und zur
        // Standardausgabe ausgeben
        String eineZeile;
        while (bReader.ready()) {
            eineZeile = bReader.readLine();
            System.out.println(eineZeile);
        }
        // Eingabestrom schliessen
        bReader.close();
    }
}

```

# 4 常用 使用的套路

Die Klasse FileInputStream beispielsweise liest Bytes aus einer Datei. Die Klasse InputStreamReader wandelt diese in char-Typen um. Damit die Typumwandlung schnell und effizient erfolgen kann, werden die Daten in einem weiteren Eingabestrom, dem BufferedReader, gesammelt, so dass der Umwandlungsprozess nicht für jedes einzelne Byte durchgeführt werden muss.

Um einen in die Konsole eingegebenen Text in eine Datei zu schreiben, wird dieser zuerst vom Programm Zeile für Zeile eingelesen und anschließend ausgegeben.

Mittels des Eingabestroms InputStreamReader werden die aus dem einfachen Eingabestrom System.in eingelesenen Daten entsprechend einer speziellen Zeichencodierung in Unicode-Zeichen umgewandelt.

Um den Eingabestrom InputStreamReader wird die Pufferklasse BufferedReader gelegt. Durch das Zwischenspeichern der Daten ist es möglich gleich ganze Bytefolgen in einem einzigen Umwandlungsprozess in die entsprechende Zeichenfolge umzusetzen.

Mittels der Klasse FileWriter werden die in den BufferedWriter eingelesenen Daten in eine Datei geschrieben. Auch hier wird wieder ein Puffer zum Zwischenspeichern der Daten verwendet. 

```java

BufferedReader br = new BufferedReader(new InputStreamReader( new FileInputStream(filePath), "gbk")); 
BufferedWriter bWriter = new BufferedWriter(new FileWriter(dateiName));
```



```java

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStreamReader;

/**
 * Klasse zur Kombination von Ein- und Ausgabestroemen: Text wird ueber die
 * Tastatu eingelesen und in eine Datei geschrieben.
 * 
 * @author skalt
 * @version 2.0 07/2009
 */

public class TextInDateiSchreiben {
    /**
     * Main-Methode.
     * 
     * @param args
     *            Uebergabeparameter
     * @throws IOException
     */
    public static void main(String[] args) throws IOException {
        // Dateiname ohne Pfadangabe
        String dateiName = "TestDatei.txt";
        // Methode zum Schreiben in eine Datei aufrufen
        schreibeTextInDatei(dateiName);
    }

    /**
     * Methode zum Testen Schreiben von Text, der ueber die Tastatur eingegeben
     * wird, in eine Datei.
     * 
     * @param dateiName
     *            Name der Datei in die Text geschrieben wird
     * @throws IOException
     */
    public static void schreibeTextInDatei(String dateiName) throws IOException {
        // Benutzerhinweise anzeigen
        System.out.println("Geben Sie einen Text ein und beenden Sie Ihre "
                + "Eingabe mit Enter.");
        // Eingabestroeme erzeugen und miteinander verbinden
        BufferedReader bReader = new BufferedReader(new InputStreamReader(
                System.in));
        // Datenfluss: Programm <-- bReader <-- InputStreamReader <--
        // Standardeingabe

        // Ausgabestroeme erzeugen und miteinander verbinden
        BufferedWriter bWriter = new BufferedWriter(new FileWriter(dateiName));
        // Datenfluss: Programm --> bWriter --> FileWriter --> Datei

        // ueber Tastatur eingegebenen Text zwischenspeichern
        String text = bReader.readLine();
        // Eingabestrom wird geschlossen
        bReader.close();
        // Zeichenfolge aus der Variable text wird in die Datei geschrieben
        bWriter.write(text);
        // Ausgabestrom wird geschlossen
        bWriter.close();
        // Benutzerhinweise anzeigen
        System.out.println("Ihre Eingaben wurden in die Datei '" + dateiName
                + "' gespeichert.");
    }
}

```