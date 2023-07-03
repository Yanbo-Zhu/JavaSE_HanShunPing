
DTD: Document type Definition

XML-Dokumente sollten nicht nur wohlgeformt (well-formed) sondern auch gültig (valid) sein. Eine Validierung kann durch die Beschreibung der Struktur des Dokuments mittels einer DTD oder eines XML-Schemas durchgeführt werden. Der Vorteil der Validierung liegt darin, dass die Struktur der XML-Dokumente überprüft werden kann. Falsch strukturierte Dokumente können erkannt und Verarbeitungsfehler vermieden werden.

# 1 DTD

 Ein DTD-Dokument dient der Beschreibung der Struktur von XML-Dokumenten.

Das DTD-Dokument kann innerhalb des XML-Dokuments deklariert oder als externe Referenz angegeben werden. Die DTD als externe Referenz wird vor allem dann benutzt, wenn ein und dieselbe DTD mehrere XML-Dokumente beschreibt.

Eine DTD funktioniert wie eine kontextfreie Grammatik – mit besonderer Schreibweise. 

## 1.1 DTD intern und extern deklarieren


intern deklarieren 
Wenn eine DTD innerhalb eines XML-Dokuments deklariert wird, muss es in ein DOCTYPE-Element geschachtelt (nested) werden:
`<!DOCTYPE root-Element [Elementdeklarationen]> `
``` xml
<?XML version="1.0"?>
<!-- DTD Definition -->
<!DOCTYPE student [     
    <!ELEMENT student (vorname, nachname)>     
    <!ELEMENT vorname (#PCDATA)>     
    <!ELEMENT nachname (#PCDATA>
] >
<!-- XML-Dokument -->
<student>     
    <vorname>Anna</vorname>     
    <nachname>Schwarz</nachname>
</student>
```


externe Definition

`<!DOCTYPE root-Element SYSTEM "dateiname">`
Die Element- und Attributdefinitionen stehen in dem externen DTD-Dokument in beliebiger Reihenfolge und werden in einer Datei mit der Endung .dtd gespeichert.

Das nachfolgende Beispiel zeigt, wie die DTD student.dtd in des XML-Dokument eingebunden wird. Pfadangaben vor dem Dateinamen sind nicht notwendig, wenn die DTD im gleichen Verzeichnis wie die XML-Datei liegt. Es können aber relative Pfade oder ein URL-Link angegeben werden. 

```xml
<?XML version="1.0"?>
<!-- DTD Einbinden -->
<!DOCTYPE student SYSTEM "student.dtd">
<student>
    <vorname>Anna</vorname>
    <nachname>Schwarz</nachname>
</student>
```


## 1.2 DTD Syntaxregeln für Elemente

- Elemente werden folgendermaßen definiert:  
    <!ELEMENT Element (Kindelement)>  
- Mehrere Elemente, die nacheinander vorkommen, werden mit Komma getrennt angegeben:  
    <!ELEMENT Element (Kindelement1, Kindelement2)>  
- Die Auswahl zwischen verschiedenen Elementen wird mit dem Operator | angegeben:  
    <!ELEMENT Element (Kindelement1|Kindelement2|Kindelement3)>
- Wenn ein Element nur Daten enthält, wird #PCDATA benutzt:  
    <!ELEMENT Element (#PCDATA)>  
    PCDATA heißt parsed character data – hier werden die Zeichen &, < und > vom Parser als XML-Metazeichen interpretiert; diese müssen dann als **Unicode-Kodierung** angegeben werden.


Beispiele für Elementdefinitionen
```
    Die Datenstruktur eines HTML-Dokuments kann mittels einer DTD folgendermaßen definiert werden: <!ELEMENT html (head, body)>
    Das title-Element enthält einen konkreten Wert. <!ELEMENT title (#PCDATA)>
    Ein boolean-Element kann das Element true oder das Element false annehmen. <!ELEMENT boolean (true|false)>

```


### 1.2.1 Regeln für die Wiederholung von Elementen

Standardmäßig kommt jedes Element nur einmal vor. Wenn ein Element mehr als einmal vorkommen soll oder darf, muss man einen der Operatoren *, + oder ? nach dem Elementnamen in den Klammern verwenden.

```
* 	
beliebig 0, 1 oder mehrmals

+ 	
mindestens 1 oder mehrmals

? 	
optional – 0 oder 1

, 	
Sequenz


| 	
oder
```


Beispiel 

```xml


<a>
  <b>
    <c>
      <d>Das ist ein erster Test.</d>
    </c>
    <c>
      <d>Das ist ein zweiter Test.</d>
    </c>
  </b>
</a>


```

DTD 

```xml


<!ELEMENT a (b?)>
<!ELEMENT b (c*)>
<!ELEMENT c (d+)>
<!ELEMENT d (#PCDATA)>


```


### 1.2.2 Attribute

Jedes Element kann ein oder mehrere Attribute (fields) besitzen. Ein Attribut kann in verschiedenen Elementen vorkommen, aber ein Element kann nicht zwei Attribute mit demselben Namen besitzen. Die Attributliste für ein Element wird folgendermaßen definiert:

```
<!ATTLIST [Elementname] [Attributname] [Datentyp] [Modifizierer] >
```


Datentyp
```
#CDATA 	

CDATA heißt "character data" und ist der Datentyp für die Attributdeklaration. CDATA wird für gewöhnlichen Text verwendet.
	 
definierte Typen 	
Der Datentyp kann auch aus selbst definierten Typen bestehen.
Beispiel: (privat | geschaeftlich).

Ein Defaultwert für den Datentyp kann angegeben werden.
Beispiel: privat wird als Defaultwert angegeben:
<...(privat | geschaeftlich) "privat">
```

Modifizierer
```
#REQUIRED 	
Bedeutet, dass das Attribut existieren d. h. angegeben sein muss.
	 
#IMPLIED 	
Bedeutet, dass das Attribut angegeben sein kann, aber nicht angegeben sein muss.
	 
#Fixed wert 	
Der Inhalt des Attributes muss dem angegebenen wert entsprechen. 
```


 Attributdefinition (Beispiel)
```xml
<!ATTLIST img
     src    CDATA #REQUIRED
     width  CDATA #REQUIRED
     height CDATA #IMPLIED>
```

- img ist der Name des Elements, für das die Attribute vereinbart werden
- src, width, height sind die Attributnamen
- CDATA ist der Datentyp der Attribute
- die Attribute src und width müssen angegeben sein
- das Attribute height kann angegeben sein


# 2 XML-Schema (Exkurs)


Eine weitere Möglichkeit der XML-Validierung ist das **XML-Schema**. Das XML-Schema hat folgende Eigenschaften:

- Das XML-Schema ist komplexer und bietet mehr Funktionalität als die DTD.
- erlaubt exaktere Beschreibung der Daten und Datentypen.
- komplexe Strukturen können definiert werden.
- XML-Schema ist selber XML, d. h. es kann mit demselben Mechanismen verarbeitet werden wie das XML-Dokument auch.
- das Vokabular gehört zu einem festgelegten Namensraum (namespace).  
    [http://www.w3.org/2001/XMLSchema](http://www.w3.org/2001/XMLSchema)
    

Das Codebeispiel zeigt das XML-Schema für unser Adressbuch-XML-Dokument. XSD steht für XML-Schema-Definition.

```xml


<?xml version="1.0" encoding="utf-16"?>
  <xsd:schema attributeFormDefault="unqualified" elementFormDefault="qualified"
   version="1.0" xmlns:xsd="htttp://www.w3.org/2001/XMLSchema">
    <xsd:element name="Adressbuch">
     <xsd:complexType>
      <xsd:sequence>
       <xsd:element maxOccurs="unbounded" name="Kontakt">
        <xsd:complexType>
         <xsd:sequence>
          <xsd:element name="Name">
           <xsd:complexType>
            <xsd:sequence>
             <xsd:element name="Vorname" type="xsd:string" />
             <xsd:element name="Nachname" type="xsd:string" />
            </xsd:sequence>
           </xsd:complexType>
          </xsd:element>
          <xsd:element name="Adresse">
           <xsd:complexType>
            <xsd:sequence>
             <xsd:element name="Strasse" type="xsd:string" />
             <xsd:element name="PLZ" type="xsd:string" />
             <xsd:element name="Ort" type="xsd:string" />
             <xsd:element name="Land" type="xsd:string" />
            </xsd:sequence>
           </xsd:complexType>
          </xsd:element>
         </xsd:sequence>
         <xsd:attribute name="typ" type="xsd:string" />
        </xsd:complexType>
       </xsd:element>
      </xsd:sequence>
     </xsd:complexType>
    </xsd:element>
  </xsd:schem>


```

