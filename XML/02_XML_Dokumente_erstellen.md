# 1 Syntaxregeln

1. Es existiert nur ein einziges Wurzelelement (**root**-Element), das alle anderen XML-Elemente umschließt.
    
2. Jedes XML-Element steht in spitzen Klammern.  
    Start-Element: <...>  
    Ende-Element:  </...>  
    Beispiel: `<kindelement>inhalt</kindelement>`
    
3. XML unterscheidet zwischen Groß- und Kleinschreibung `<P>inhalt</p>.`  
    Beispiel: Das End-Element` </adresse>` ist nicht korrekt, wenn das Start-Element` <Adresse>` lautet.
    
4. Die Element-Bezeichner können mehrfach verwendet werden.  
    Beispiel:  
    ```
      <frage>  
        <frage>  
            <frage>Was ist eine Klasse?</frage>  
            <frage>Was ist ein Objekt?</frage>  
        </frage>  
        <beurteilung/>  
    </frage>
    ```  
    
5. Jedes geöffnete Element muss auch wieder geschlossen werden.  
    Beispiel: `<P>inhalt </P>`
    
6. Die Elemente müssen in der richtigen Reihenfolge verschachtelt werden.  
    Beispiel: `<A><B> inhalt</B></A>`
7. Die Elemente müssen in der richtigen Reihenfolge geschlossen werden und zwar in umgekehrter Reihenfolge wie sie geöffnet wurden.  
    Beispiel (die zusammengehörige Element sind in der gleichen Farbe dargestellt):
```xml
    <ELEMENT1>  
        <ELEMENT1_1>inhalt1</ELEMENT1_1>  
        <ELEMENT1_2>  
            <ELEMENT_1_2_1>inhalt2</ELEMENT_1_2_1>  
        </ELEMENT1_2>  
    </ELEMENT1>
```



8. Elemente ohne Inhalt sind ebenfalls möglich.  
    Beispiel: `<subchild></subchild>  `
    Diese können auch in Kurzform angegeben werden.  
    Beispiel: <subchild/>
    
9. Alle verwendeten Attribute müssen mindestens einen Wert enthalten.
10. Ein Attributbezeichner darf nur einmal pro Element verwendet werden.
11. Das XML Dokument muss mit dem sogenannten **Prolog** beginnen.  
    (mehr zum Prolog erfahren Sie im Abschnitt Aufbau eines XML Dokuments)

## 1.1 Sonderzeichen (special character)
wie <,>, &, ' und " können nicht als Inhalte von Elementen oder Attributen verwendet werden und werden wie in HTML angegeben:

```xml
& 	&amp;
' 	&apos;
< 	&lt;
> 	&gt;
" 	&quot;

```


## 1.2 Wohlgeformt und gültig

wohlgeformt:
Werden alle Syntaxregeln eingehalten, dann ist das XML-Dokument wohlgeformt (well-formed). Überprüft wird die Syntax durch sogenannte XML-Parser.


gültig：
Ein XML-Dokument ist gültig (valid), wenn seine Elemente und Struktur den Vorgaben einer sogenannten Dokumententypdefinition (Document Type Definition, DTD) oder eines Schemas entsprechen (XML Schema).


Um ein XML-Dokument verarbeiten zu können, muss das Dokument wohlgeformt aber nicht zwingend gültig sein.


## 1.3 Aufbau eines XML-Dokuments


### 1.3.1 prolog
In der ersten Zeile in einem XML-Dokument steht der sogenannte Prolog. Er deklariert das Dokument als XML-Dokument und enthält Informationen über die verwendete XML-Version:
`<?xml version="1.0"?>`

Der Prolog kann um die Angabe der zu verwendeten Zeichenkodierung (character encoding) erweitert werden:
`<?xml version="1.0" encoding="ISO-8859-1"?>`


### 1.3.2 Kommentare

Innerhalb des Dokuments können beliebig viele Kommentare (comments) eingefügt werden, angegeben werden sie wie in HTML.
`<!-- Ich bin ein Kommentar -->`


### 1.3.3 Inhalt von Elementen
Ein Element kann:
- leer sein  
    (dann abgekürzt geschrieben, wie <Adresse/> im Beispiel)
- einen Textinhalt haben,
- andere geschachtelte (nested) Elemente haben
- oder beides enthalten.

### 1.3.4 Attribute

==Ein Element muss immer ein Attribut haben.==
 	
Elemente können Attribute enthalten. Attribute (fields) enthalten zusätzliche Informationen zum Element. In unserem Beispiel hat das Element <Kontakt> ein Attribut typ. Attribute werden in folgendem Format angegeben:

```
<elementname attributname="attributinhalt"> oder
<elementname attributname='attributinhalt'>
Beispiel: <Adresse typ="privat"> <Adresse id='ad_1'>
````

Weitere Beispiele für Attribute sind width, als Angabe der Bildbreite bei einem image Element oder href, als Adressangabe für einen Link.

Ein Entwickler, der eine XML-Sprache, ein XML Schema oder eine DTD entwirft, kann entscheiden ob er Attribute verwendet oder ob er diese Daten ebenfalls als Elemente vereinbart.
