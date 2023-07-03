

# 1 Java Parser (SAX (Simple Api for XML) und DOM (Document Object Model))

In Java können XML-Dokumente unterschiedlich verarbeitet werden. Wir betrachten zwei unterschiedliche Strategien: SAX (Simple Api for XML) und DOM (Document Object Model)

 	
## 1.1 SAX
Teile verarbeiten SAX führt eine sequentielle Verarbeitung des XML-Dokuments durch. Für jeden Bestandteil des XML-Dokuments (Elemente, Attribute etc.) sowie für Fehler und Warnungen wird ein definiertes Ereignis (event) ausgelöst (ähnlich wie beim Klicken eines Buttons), das mit für diesen Zweck programmierten Ereignisbehandlungsmethoden (event handler) behandelt werden kann.

Nachteil des SAX-Parser ist, das nicht die gesamte XML-Struktur im Speicher vorliegt, sondern immer nur Teile verarbeitet werden können. Die Verarbeitung mit SAX ist empfohlen für XML-Dokumente, die nur einmal verarbeitet werden müssen.

Eigenschaften von SAX

    ereignisgesteuert (event controlled)
    schnell und einfach
    wenig Speicherplatzbedarf
    Nach dem Parsen kann nicht mehr auf bereits geparste Elemente zugegriffen werden.


## 1.2 **DOM** 
DOM liest XML-Dokumente komplett in den Speicher ein und ist dadurch unter Umständen langsamer als SAX. Der Inhalt aller Elemente und Attribute wird im Speicher als Baum (in-memory tree) dargestellt und kann sehr effektiv bearbeitet werden. Der Speicher- und Ressourcenbedarf steigt mit der Größe des XML-Dokuments. Im Gegensatz zu SAX ist es mit DOM möglich, auf bereits verarbeitete Teile des XML-Dokuments wiederholt zuzugreifen oder die Struktur des gesamten Dokuments auf einmal zu verändern.<br><br>Eigenschaften von DOM
- Das Parsen ist langsamer als bei SAX, insbesondere bei größeren XML-Dokumenten  
- Großer Speicherplatzbedarf

DOM-Implementationen
- Standard Java Implementation  <br>    Die in Java enthaltene XML-Implementation folgt den **W3C**-Spezifikationen, ist aber nicht besonders objektorientiert.  
- JDOM  <br>    Die JDOM-Implementation folgt nicht den W3C-Spezifikationen, erlaubt aber eine kompaktere Programmierung.|



# 2 XML-Dateien einlesen und verarbeiten mit SAX

In diesem Abschnitt lernen Sie wie Sie eine existierende XML-Datei mit dem SAX-Parser einlesen.<br><br>Folgende Schritte sind für das Parsen eines XML-Dokuments in einer Datei mit SAX notwendig:<br<br>1. Erzeugen eines SAXParserFactory-Objekts.<br>2. Erzeugen eines SAXParser-Objekts.<br>3. Implementieren und Erzeugen des Ereignishandlers (event handler) DefaultHandler.<br>4. Starten des Parsens des XML-Dokuments durch den Aufruf der Methode (method invocation) parse(File f, DefaultHandler dh) des SAXParser-Objekts.


Ereignisbehandlung des DefaultHandlers
Die Klasse DefaultHandler enthält leere Methoden (ähnlich einer Adapter-Klasse), die überschrieben werden müssen, um eine eigene Ereignisbehandlung zu implementieren. 

|Methoden|Beschreibung|
|---|---|
|startDocument()|Wird am Anfang des XML-Dokuments aufgerufen.|
|endDocument()|Wird am Ende des XML-Dokuments aufgerufen.|
|startElement(String namespaceURI,  <br>String localName,  <br>String qualifiedName,  <br>Attributes attrs)|Wird aufgerufen wenn ein neues Element anfängt. Die wichtigsten Parameter der Methode sind: String qualifiedName enthält den Elementnamen, Attributes attrs enthält die Liste der Attribute die zu dem Element gehören.|
|endElement(String namespaceURI,  <br>String localName,  <br>String qualifiedName)|Wird aufgerufen wenn ein Element endet. Der wichtigste Parameter der Methode ist: String qualifiedName. Er enthält den Elementnamen.|
|characters(char[] chars,  <br>int start,  <br>int len)|Wird aufgerufen für die Zeichen die in einem Element enthalten sind. Der Parameter der Methode sind: char[] chars ein Zeichen-Array mit den aktuell eingelesenen XML-Daten, int start Startposition der Zeichen des aktuellen Elements in dem übergebenen Array, int len Anzahl der Zeichen des aktuellen Elements in dem übergebenen Array.|

## 2.1 Beispiel

```java
import java.io.File;
import java.io.IOException;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

/*
 * Klasse zum Ausgeben eines XML-Dokuments auf der Konsole unter der Benutzung
 * des SAX Parsers.
 * 
 * @author Prof. Dr. Andreas Solymosi, Sandra Kaltofen
 */
public class SAXAusgabe {
    // Pfad und Name der XML-Datei die ausgegeben werden soll
    private static String XMLDateiName = "adressen2.xml";
    
    /*
     * Main Methode.
     * 
     * @param args Uebergabeparameter
     */
    public static void main(String[] args) {
        // zu parsende XML-Datei
        File eineDatei = new File(XMLDateiName);
        // Sax Ereignis Handler erzeugen
        MyDefaultHandler handler = new MyDefaultHandler();

        try {
            // SAXParserFactory erzeugen
            SAXParserFactory factory = SAXParserFactory.newInstance();
            // SAXParser erzeugen
            SAXParser parser = factory.newSAXParser();
            // XML-Dokument parsen unter Verwendung des implementierten
            // Ereignishandlers
            parser.parse(eineDatei, handler);
            
        // ExceptionHandling    
        } catch (SAXException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ParserConfigurationException e) {
            e.printStackTrace();
        }
    }

    /*
     * Implementation des Handlers fuer Sax-Ereignisse.
     */
    private static class MyDefaultHandler extends DefaultHandler {
        /*
         * Methode die am Anfang des XML-Dokuments aufgerufen wird.
         */
    	@Override 
        public void startDocument() {
            System.out.println("Start");
        }

        /*
         * Methode die am Ende des XML-Dokuments aufgerufen wird.
         */
    	@Override
        public void endDocument() {
            System.out.println("Ende");
        }

        /*
         * Methode die am Anfang eines Elements aufgerufen wird.
         */
    	@Override 
        public void startElement(String namespaceURI, String localName,
                String qualifiedName, Attributes attrs) {
            // Elementnamen ermitteln
            String elementName = localName.equals("") ? qualifiedName : localName;
            // Elementnamen ausgeben
            System.out.print("<" + elementName);

            // Schleife ueber alle Attribute des Elements
            for (int i = 0; i < (attrs == null ? 0 : attrs.getLength()); i++) {
                String attribut = attrs.getLocalName(i);
                if (attribut.equals(""))
                    attribut = attrs.getQName(i);
                System.out.print(" " + attribut + "=\"" + attrs.getValue(i)
                        + "\"");
            }
            // Element schliessen
            System.out.print(">");
        }

        /*
         * Methode, die am Ende eines Elements aufgerufen wird.
         */
    	@Override 
        public void endElement(String namespaceURI, String localName,
                String qualifiedName) {
            // End-Element ausgeben
            System.out.print("</" + qualifiedName + ">");
        }

        /*
         * Methode, die fuer Zeichendaten innerhalb eines Elements aufgerufen
         * wird.
         */
    	@Override 
        public void characters(char[] chars, int start, int len) {
            System.out.print(new String(chars, start, len));
        }
    }
}
```


SAXAusgabe_UXML05.java 
Ändern Sie den Quellcode von SAXAusgabe.java so, dass nicht das gesamte XML-Dokument mit seiner Syntax, sondern nur die Kontaktdaten mit Namen und Adresse und der Typ in der Eclipse-Konsole ausgegeben werden.

```java

import java.io.File;
import java.io.IOException;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

/**
 * Klasse zum Ausgeben der Kontaktdaten in dem Adressen XML-Dokument
 * auf der Konsole unter der Benutzung des SAX-Parsers.
 * 
 * @author Prof. Dr. Andreas Solymosi, Sandra Kaltofen
 */
public class SAXAusgabe_UXML05 {
    // Pfad und Name der XML-Datei, die ausgegeben werden soll
    private static String XMLDateiName = "adressen2.xml";

    /**
     * Main-Methode.
     * 
     * @param args
     *            Uebergabeparameter
     */
    public static void main(String[] args) {
        // zu parsende XML-Datei
        File eineDatei = new File(XMLDateiName);
        // Sax-Ereignis-Handler erzeugen
        MyDefaultHandler handler = new MyDefaultHandler();

        try {
            // SAXParserFactory erzeugen
            SAXParserFactory factory = SAXParserFactory.newInstance();
            // SAXParser erzeugen
            SAXParser parser = factory.newSAXParser();
            // XML-Dokument parsen unter Verwendung des implementierten
            // Ereignishandlers
            parser.parse(eineDatei, handler);

            // ExceptionHandling
        } catch (SAXException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ParserConfigurationException e) {
            e.printStackTrace();
        }
    }

    /**
     * Implementation des Handlers fuer Sax-Ereignisse.
     */
    private static class MyDefaultHandler extends DefaultHandler {
        // Flag, ob Daten ausgegeben werden sollen oder nicht
        private boolean printData = false;

        /**
         * Methode, die am Anfang eines Elements aufgerufen wird.
         */
        public void startElement(String namespaceURI, String localName,
                String qualifiedName, Attributes attrs) {
            // Elementnamen ermitteln
            String elementName = localName.equals("") ? qualifiedName
                    : localName;
            // Pruefen, ob Element zu den Kontaktdaten gehoert
            if (elementName.equals("Vorname") || elementName.equals("Nachname")
                    || elementName.equals("Strasse")
                    || elementName.equals("PLZ") || elementName.equals("Ort")
                    || elementName.equals("Land")) {
                // Flag fuer Ausgabe der Daten auf true setzen
                printData = true;
            }

            // Schleife ueber alle Attribute des Elements
            for (int i = 0; i < (attrs == null ? 0 : attrs.getLength()); i++) {
                String attribut = attrs.getLocalName(i);
                if (attribut.equals(""))
                    attribut = attrs.getQName(i);
                // Daten ausgeben wenn Attribut "typ" ist
                if (attribut.equals("typ")) {
                    System.out.println("*************************");
                    System.out.println("Kontakttyp: " + attrs.getValue(i));
                }
            }
        }

        /**
         * Methode, die fuer Zeichendaten innerhalb eines Elements aufgerufen
         * wird.
         */
        public void characters(char[] chars, int start, int len) {
            // Daten ausgeben wenn Ausgabeflag gesetzt ist
            if (printData) {
                System.out.println(new String(chars, start, len));
                printData = false;
            }
        }
    }
}

```


# 3 XML-Dateien einlesen und verarbeiten mit DOM

In diesem Abschnitt lernen Sie, wie Sie ein existierendes XML-Dokument mit dem DOM-Parser einlesen.
Folgende Schritte sind für das Parsen eines XML-Dokuments mit DOM notwendig:
1. Erzeugen eines DocumentBuilderFactory-Objekts.
2. Erzeugen eines DocumentBuilder-Objekts.
3. Starten des Parsens des XML-Dokuments durch den Aufruf der Methode parse(File f) des DocumentBuilder-Objekts.


Da das DOM-Dokument nach dem Parsen als Baum vorliegt, kann es nicht linear, sondern muss für die Verarbeitung rekursiv durchlaufen werden (mehr zum Thema Rekursion finden Sie in der Lerneinheit **Rekursion**). Diesen Vorgang nennt man auch **traversieren**. Beim Traversieren werden üblicherweise die einzelnen Elemente des XML-Dokuments verarbeitet. Die folgenden wichtigen Methoden werden dafür verwendet.


Methode der Schnittstelle (interface) Element:

|Methoden|Beschreibung|
|---|---|
|getChildNodes()|Gibt alle Kindelemente (child elements) des aufrufenden Elements in einem Objekt vom Typ NodeList zurück.|
|getElementsByTagName(String name)|Gibt alle Kindelemente des aufrufenden Elements, deren Elementnamen dem übergebenen Parameter name entsprechen, in einem Objekt vom Typ NodeList zurück|

Methode der Schnittstelle NodeList

|Methoden|Beschreibung|
|---|---|
|item(int index)|Gibt das Element, das am übergebenen index in der NodeList steht, als Objekt vom Typ Node zurück.|



## 3.1 Beispiel


1
DOMAusgabe.java 
Das nachfolgende Quellcodebeispiel parst das XML-Dokument adressen2.xml mit dem DOM-Parser und traversiert anschließend das geparste Dokument. Im Traversieren-Prozess werden die Inhalte der einzelnen Elemente in der Eclipse-Konsole ausgegeben

```java
package jml;

import java.io.File;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

/**
 * Klasse zum Ausgeben eines XML-Dokuments auf der Konsole unter der Benutzung
 * des DOM Parsers.
 *
 * @author Prof. Dr. Andreas Solymosi, Sandra Kaltofen
 */
public class DOMAusgabe {
    // Pfad und Name der XML Datei die ausgegeben werden soll
    private static String XMLDateiName = "adressen2.xml";

    /**
     * Main Methode.
     *
     * @param args Uebergabeparameter
     */
    public static void main(String[] args) throws Exception {
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        DocumentBuilder builder = factory.newDocumentBuilder();
        // Uebergebene XML-Datei parsen
        Document dokument = builder.parse(new File(XMLDateiName));
        // DOM Baum durchlaufen und ausgeben
        traversieren(dokument.getDocumentElement(), "");
    }

    /**
     * Methode zum Durchlaufen und Ausgeben des DOM Baums.
     *
     * @param e aktuelles Element
     * @param ebene String fuer die Einrueckung der Ebenen
     */
    private static void traversieren(Element e, String ebene) {
        System.out.println(ebene + e.getNodeName());
        // Kindelemente des uebergebenen Elements ermitteln
        NodeList children = e.getChildNodes();
        for (int i = 0; i < children.getLength(); i++) {
            Node knoten = children.item(i);
            // Knoten ist ein ELEMENT_NODE
            if (knoten.getNodeType() == Node.ELEMENT_NODE)
                // Rekursiver Aufruf von traversieren fuer aktuelles Element
                traversieren((Element) knoten, ebene + " ");
            // Knoten ist ein TEXT_NODE
            else if (knoten.getNodeType() == Node.TEXT_NODE) {
                String inhalt = knoten.getTextContent();
                if (inhalt.trim().length() > 0) // leerer Inhalt?
                    System.out.println(ebene + " :" + inhalt);
            } else
                // kein ELEMENT_NODE, kein TEXT_NODE
                System.out.println(ebene + knoten);
        }
    }
}
```

2
DOMAusgabe_UXML06.java 
Ändern Sie den Quellcode von DOMAusgabe.java dementsprechend, dass der Benutzer durch einen Übergabeparameter, der dem Programm beim Start übergeben wird, festlegen kann, ob er alle Kontaktdatensätze, nur die privaten oder nur die geschäftlichen ausgegeben haben will. 

```java
package jml;

import java.io.File;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.NamedNodeMap;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

/**
 * Klasse zum Ausgeben der Kontaktdaten in dem Adressen XML-Dokument,
 * entsprechend Ihres Typs, auf der Konsole unter der Benutzung des 
 * DOM Parsers.
 * 
 * @author Prof. Dr. Andreas Solymosi, Sandra Kaltofen
 */
public class DOMAusgabe_UXML06 {
    // Pfad und Name der XML Datei, die ausgegeben werden soll
    private static String XMLDateiName = "adressen2.xml";
    // Gueltige Werte fuer die Uebergabeparameter
    private static char ARG_PRIVAT = 'p';
    private static char ARG_GESCHAEFT = 'g';
    // XML Werte Konstanten
    private static String XML_TYP_ATTRIBUT = "typ";
    private static String XML_TYP_PRIVAT = "privat";
    private static String XML_TYP_GESCHAEFT = "geschaeftlich";
    // Flag, ob Datensatz ausgegeben werden soll
    private static boolean DS_ausgeben = false;

    /**
     * Main Methode.
     * 
     * @param args 
     *            Typ der auszugebenden Datensaetze 
     *            kein = alle, p = privat, g = geschaeftlich.  
     */
    public static void main(String[] args) throws Exception {
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        DocumentBuilder builder = factory.newDocumentBuilder();
        // Uebergebene XML-Datei parsen
        Document dokument = builder.parse(new File(XMLDateiName));
        // Uebergabeparameter fuer Kontakt-Typ pruefen
        char kontakt_typ = args.length > 0 ? args[0].charAt(0) : '-';
        if (kontakt_typ == ARG_PRIVAT) {
            System.out.println("*** Private Kontakte ***");
        } else if (kontakt_typ == ARG_GESCHAEFT) {
            System.out.println("*** Geschaeftliche Kontakte ***");
        } else {
            System.out.println("*** Alle Kontakte ***");
        }

        // DOM-Baum durchlaufen und ausgeben
        traversieren(dokument.getDocumentElement(), "", kontakt_typ);
    }

    /**
     * Methode zum Durchlaufen und Ausgeben des DOM-Baums.
     * 
     * @param e
     *            aktuelles Element
     * @param ebene
     *            String fuer die Einrueckung der Ebenen
     * @param kontakt
     *            Typ der Kontaktdaten, die ausgegeben werden sollen
     */
    private static void traversieren(Element e, String ebene, char kontakt_typ) {
        System.out.println(ebene + e.getNodeName());
        // Kindelemente des uebergebenen Elements ermitteln
        NodeList children = e.getChildNodes();
        for (int i = 0; i < children.getLength(); i++) {
            Node knoten = children.item(i);
            // Knoten ist ein ELEMENT_NODE
            if (knoten.getNodeType() == Node.ELEMENT_NODE) {
                // Attribut fuer Kontakt-Typ auslesen, wenn gueltiger Kontakt-Typ
                // uebergeben wurde
                if (kontakt_typ == ARG_PRIVAT || kontakt_typ == ARG_GESCHAEFT) {
                    // Liste der Attribute ermitteln
                    NamedNodeMap attribute = knoten.getAttributes();
                    if (attribute != null) {
                        // Schleife ueber alle Attribute
                        for (int k = 0; k < attribute.getLength(); k++) {
                            Node attributNode = attribute.item(k);
                            // Pruefen auf privaten Kontakt
                            if ((((attributNode.getNodeName()
                                    .equals(XML_TYP_ATTRIBUT))
                                    && (kontakt_typ == ARG_PRIVAT) && (attributNode
                                    .getNodeValue().equals(XML_TYP_PRIVAT))))
                                    ||
                                    // Pruefen auf geschaeftlichen Kontakt
                                    ((attributNode.getNodeName()
                                            .equals(XML_TYP_ATTRIBUT))
                                            && (kontakt_typ == ARG_GESCHAEFT) && (attributNode
                                            .getNodeValue()
                                            .equals(XML_TYP_GESCHAEFT)))) {
                                System.out.println("-------------------");
                                // Wenn Kontaktdaten dem gesetzten Typ
                                // entsprechen, Flag fuer Ausgabe auf true setzen
                                DS_ausgeben = true;
                            // Kontakdaten entsprechen nicht dem uebergebenen Typ
                            } else {
                                // Flag fuer Ausgabe auf false setzen
                                DS_ausgeben = false;
                            }
                        }
                    }
                // Alle Datensaetze ausgeben, wenn kein gueltiger Kontakt-Typ
                // uebergeben wurde
                } else {
                    DS_ausgeben = true;
                }

                // Kontaktdaten-Element traversieren, wenn Flag fuer Ausgabe
                // gesetzt ist
                if (DS_ausgeben) {
                    // Rekursiver Aufruf von traversieren fuer aktuelles Element
                    traversieren((Element) knoten, ebene + " ", kontakt_typ);
                }
            }

            // Knoten ist ein TEXT_NODE
            else if (knoten.getNodeType() == Node.TEXT_NODE) {
                String inhalt = knoten.getTextContent();
                if (inhalt.trim().length() > 0) // leerer Inhalt?
                    System.out.println(ebene + " :" + inhalt);
            } else
                // kein ELEMENT_NODE, kein TEXT_NODE
                System.out.println(ebene + knoten);
        }
    }
}

```


# 4 XML-Dateien erstellen (mit DOM )


folgenden Abschnitt lernen Sie, wie Sie ein XML-Dokument mit Java erstellen, Daten einfügen (insert) und das Dokument in einer Datei speichern.<br><br>Folgende Schritte sind für das Erstellen eines neuen XML-Dokuments mit DOM notwendig:<br><br>1. Erzeugen eines DocumentBuilderFactory-Objekts.<br>2. Erzeugen eines DocumentBuilder-Objekts.<br>3. Durch den Aufruf der Methode newDocument des DocumentBuilder-Objekts wird ein neues XML-Dokument und ein Objekt der Klasse Document erzeugt.


## 4.1 **Wichtige Methoden für das Erstellen neuer XML-Dokumente**

Methoden der Klasse Document

|Methoden|Beschreibung|
|---|---|
|**createElement(String tagname)**|Erstellen eines neuen **Element**. Der Elementname enstpricht dem übergebenen **tagname**.|
|**createTextNode(String data)**|Erstellt ein neues **Text**-Element, das als Inhalt von **Element** verwendet wird.|

Methoden der Klasse Element: 

|Methoden|Beschreibung|
|---|---|
|**appendChild(Node newChild)**|Fügt das übergebene Node Objekt in das aufrufende **Element** als Kindelement ein.|
|**setAttribute(String name, String value)**|Erstellt ein Attribut an dem aufrufenden Element mit dem Namen **name** und dem Wert **value**.|


## 4.2 Speichern eines XML-Dokuments in einer Datei
Folgende Schritte sind für das Erstellen eines neuen XML-Dokuments mit DOM notwendig:
1. Erzeugen eines TransformerFactory-Objekts.
2. Erzeugen eines Transformer-Objekts.
3. Erzeugen eines DOMSource-Objekts von dem aktuellen XML-Dokument.
4. Durch den Aufruf der Methode transform(Source xmlSource, Result ouputTarget) kann das XML-Dokument in einem übergebenen File-Objekt gespeichert werden.


## 4.3 Beispiel 

1
XMLErstellen.java


Im nachfolgende Quellcodebeispiel wird ein XML-Dokument erstellt, verschiedene Elemente werden erzeugt und in das Dokument eingefügt. Am Ende wird das erzeugte XML-Dokument in eine Datei gespeichert. Um diese XML-Datei zu öffnen, wählen Sie nicht einen Text-Editor, sondern den XML-Editor von Eclipse (File → Open File... bzw. im Package Exlorer Doppelklick auf die Datei), damit Sie die Struktur der Daten gut erkennen können.

```java
package jml;

import java.io.File;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.Result;
import javax.xml.transform.Source;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerConfigurationException;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

/**
 * Klasse zum Erstellen und Speichern eines XML-Dokuments mit verschiedenen
 * Beispiel-Elementen.
 * 
 * @author Sandra Kaltofen
 */
public class XMLErstellen {

    // Pfad und Name der XML-Datei, die erzeugt werden soll
    private static String XMLDateiName = "testdokument.xml";

    /**
     * Main Methode.
     * 
     * @param args
     *            Uebergabeparameter.
     */
    public static void main(String[] args) {
        try {
            // Neues XML-Dokument erzeugen
            DocumentBuilderFactory factory = DocumentBuilderFactory
                    .newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();
            Document dokument = builder.newDocument();

            // Root Element des XML-Dokuments erstellen
            Element root = dokument.createElement("Root");

            // XML-Element erstellen
            Element kind1 = dokument.createElement("Kind1");
            Element kind2 = dokument.createElement("Kind2");
            Element kind3 = dokument.createElement("Kind3");

            // Kindelement fuer "Kind1" erstellen und anfuegen
            Element kind1_1 = dokument.createElement("Kind1_1");
            // Attribut fuer Element setzen
            kind1_1.setAttribute("attr1", "attribut1");
            // Text Element erstellen und anfuegen
            kind1_1.appendChild(dokument.createTextNode("Inhalt von Kind1_1"));
            // Kindelement and "Kind1" anfuegen
            kind1.appendChild(kind1_1);

            // Erzeugte Kindelemente an RootElement anfuegen
            root.appendChild(kind1);
            root.appendChild(kind2);
            root.appendChild(kind3);

            // Root-Element in XML-Dokument einfuegen
            dokument.appendChild(root);

            // XML-Dokument in Datei speichern
            saveXMLDocumentToFile(dokument, XMLDateiName);

            // Rueckmeldung an den Benutzer
            System.out.println("XML-Datei erfolgreich erstellt.");

        } catch (ParserConfigurationException e) {
            e.printStackTrace();
        }

        return;
    }

    /**
     * Speichert das uebergebene XML-Dokument in der uebergebenen Datei.
     * 
     * @param pXMLdocument
     *            zu speicherndes XML-Dokument
     * @param pFileName
     *            Pfad und Name der Datei
     */
    private static void saveXMLDocumentToFile(Document pXMLdocument,
            String pFileName) {
        try {
            TransformerFactory tFactory = TransformerFactory.newInstance();
            Transformer transformer = tFactory.newTransformer();
            Source src = new DOMSource(pXMLdocument);
            File file = new File(pFileName);
            Result dest = new StreamResult(file);
            transformer.transform(src, dest);
        } catch (TransformerConfigurationException e1) {
            e1.printStackTrace();
        } catch (TransformerException e) {
            e.printStackTrace();
        }
    }
}
```


2 
XMLErstellen_UXML07.java

Laden Sie den Quellcode von XMLErstellen.java in Ihren Editor. Kompilieren und testen Sie das Programm.

Erstellen Sie eine neue Java-Klasse XMLErstellen_UXML07.java, mit der Sie das Adressbuch-XML-Dokument, das Sie in Kapitel 3 manuell in Ihrem Editor erstellt haben (adressen2.xml), mit Java erstellen. Zur Erinnerung nochmal die Adressbuch-Datenstruktur:

![](image/Pasted%20image%2020230618210806.png)

Die Kontaktdaten sollen durch eine eigene Klasse Kontakt.java, die alle Attribute eines Kontaktdatensatzes enthält, abgebildet werden. Nachdem Sie das XML-Dokument mit mindestens drei Beispiel-Kontaktdatensätzen gefüllt haben, soll es in der Datei adressen_neu.xml abgespeichert werden.


```java
import java.io.File;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.Result;
import javax.xml.transform.Source;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerConfigurationException;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Text;

/**
 * Klasse zum Erstellen und Speichern eines XML-Dokuments mit Kontaktdaten.
 * 
 * @author Sandra Kaltofen
 */
public class XMLErstellen_UXML07 {

    // Pfad und Name der XML Datei die erzeugt werden soll
    private static String XMLDateiName = "adressen_neu.xml";

    /**
     * Main Methode.
     * 
     * @param args
     *            Uebergabeparameter.
     */
    public static void main(String[] args) {
        try {
            // Neues XML Dokument erzeugen
            DocumentBuilderFactory factory = DocumentBuilderFactory
                    .newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();
            Document dokument = builder.newDocument();

            // Root Element "Adressbuch" erstellen
            Element root = dokument.createElement("Adressbuch");

            // Privaten Kontakt erstellen
            Kontakt kontakt1 = new Kontakt(Kontakt.KontaktTyp.privat,
                    "Mustermann", "Max", "Musterstrasse 10", "10199", "Berlin",
                    "Deutschland");
            Element kontakt1Element = createNewKontaktElement(dokument,
                    kontakt1);
            root.appendChild(kontakt1Element);

            // Privaten Kontakt erstellen
            Kontakt kontakt2 = new Kontakt(Kontakt.KontaktTyp.privat, "Tester",
                    "Sandra", "Stallvagen 3", "35761", "Alvesta", "Schweden");
            Element kontakt2Element = createNewKontaktElement(dokument,
                    kontakt2);
            root.appendChild(kontakt2Element);

            // Geschaeftlichen Kontakt erstellen
            Kontakt kontakt3 = new Kontakt(Kontakt.KontaktTyp.geschaeftlich,
                    "Makler", "Moritz", "Lindenstrasse 3", "54762",
                    "Frankfurt am Main", "Deutschland");
            Element kontakt3Element = createNewKontaktElement(dokument,
                    kontakt3);
            root.appendChild(kontakt3Element);

            // Root Element in XML Dokument einfuegen
            dokument.appendChild(root);

            // XML Document in Datei speichern
            saveXMLDocumentToFile(dokument, XMLDateiName);

            // Rueckmeldung an den Benutzer
            System.out.println("XML Datei erfolgreich erstellt.");

        } catch (ParserConfigurationException e) {
            e.printStackTrace();
        }

        return;
    }

    /**
     * Erzeugen eines neuen XML Elements entsprechend der uebergebenen
     * Kontaktdaten.
     * 
     * @param dokument
     *            XML Dokument fuer das das Element erzeugt werden soll.
     * @param pKontakt
     *            Kontaktdaten.
     * @return erzeugtes Kontakt Element.
     */
    private static Element createNewKontaktElement(Document dokument,
            Kontakt pKontakt) {
        Element kontakt = dokument.createElement("Kontakt");
        kontakt.setAttribute("typ", pKontakt.getTyp().toString());
        // Name Element
        Element name = dokument.createElement("Name");
        kontakt.appendChild(name);
        // Vorname
        Element vorname = dokument.createElement("Vorname");
        Text vornameText = dokument.createTextNode(pKontakt.getVorname());
        vorname.appendChild(vornameText);
        name.appendChild(vorname);
        // Nachname
        Element nachname = dokument.createElement("Nachname");
        Text nachnameText = dokument.createTextNode(pKontakt.getNachname());
        nachname.appendChild(nachnameText);
        name.appendChild(nachname);
        // Adresse
        Element adresse = dokument.createElement("Adresse");
        kontakt.appendChild(adresse);
        // Strasse
        Element strasse = dokument.createElement("Strasse");
        Text strasseText = dokument.createTextNode(pKontakt.getStrasse());
        strasse.appendChild(strasseText);
        adresse.appendChild(strasse);
        // PLZ
        Element plz = dokument.createElement("PLZ");
        Text plzText = dokument.createTextNode(pKontakt.getPlz());
        plz.appendChild(plzText);
        adresse.appendChild(plz);
        // Ort
        Element ort = dokument.createElement("Ort");
        Text ortText = dokument.createTextNode(pKontakt.getOrt());
        ort.appendChild(ortText);
        adresse.appendChild(ort);
        // Land
        Element land = dokument.createElement("Land");
        Text landText = dokument.createTextNode(pKontakt.getLand());
        land.appendChild(landText);
        adresse.appendChild(land);

        return kontakt;
    }

    /**
     * Speichert das uebergebene XML Dokument in der uebergebenen Datei.
     * 
     * @param pXMLdocument
     *            zu speicherndes XML Dokument
     * @param pFileName
     *            Pfad und Name der Datei
     */
    private static void saveXMLDocumentToFile(Document pXMLdocument,
            String pFileName) {
        try {
            TransformerFactory tFactory = TransformerFactory.newInstance();
            Transformer transformer = tFactory.newTransformer();
            Source src = new DOMSource(pXMLdocument);
            File file = new File(pFileName);
            Result dest = new StreamResult(file);
            transformer.transform(src, dest);
        } catch (TransformerConfigurationException e1) {
            e1.printStackTrace();
        } catch (TransformerException e) {
            e.printStackTrace();
        }
    }
}

```


kontake.java
```java

public class Kontakt {

    public static enum KontaktTyp {
        privat, geschaeftlich
    }

    private String id;
    private KontaktTyp typ;
    private String nachname;
    private String vorname;
    private String strasse;
    private String plz;
    private String ort;
    private String land;

    public Kontakt(String pId, KontaktTyp pTyp, String pNachname,
            String pVorname, String pStrasse, String pPlz, String pOrt,
            String pLand) {
        setId(pId);
        setTyp(pTyp);
        setNachname(pNachname);
        setVorname(pVorname);
        setStrasse(pStrasse);
        setPlz(pPlz);
        setOrt(pOrt);
        setLand(pLand);
    }

    public Kontakt(KontaktTyp pTyp, String pNachname, String pVorname,
            String pStrasse, String pPlz, String pOrt, String pLand) {

        this(null, pTyp, pNachname, pVorname, pStrasse, pPlz, pOrt, pLand);
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public KontaktTyp getTyp() {
        return typ;
    }

    public void setTyp(KontaktTyp typ) {
        this.typ = typ;
    }

    public String getNachname() {
        return nachname;
    }

    public void setNachname(String name) {
        this.nachname = name;
    }

    public String getVorname() {
        return vorname;
    }

    public void setVorname(String vorname) {
        this.vorname = vorname;
    }

    public String getStrasse() {
        return strasse;
    }

    public void setStrasse(String strasse) {
        this.strasse = strasse;
    }

    public String getPlz() {
        return plz;
    }

    public void setPlz(String plz) {
        this.plz = plz;
    }

    public String getLand() {
        return land;
    }

    public void setLand(String land) {
        this.land = land;
    }

    public String getOrt() {
        return ort;
    }

    public void setOrt(String ort) {
        this.ort = ort;
    }
	@Override
    public String toString() {
	return ("(" + id + ", " + typ.toString() + ", " + vorname + ", "
	        + nachname + ", " + strasse + ", " + plz + ", " + ort + ", "
	        + land + ")");
    }
}

```


# 5 XML Dateien verändern (mit Dom)
  	

Im folgenden Abschnitt sehen Sie, mit welchen Klassen und Methoden Sie bestehende XML-Dokumente ändern (edit) sowie Elemente und Inhalte löschen (delete). In der Übungsaufgabe sollen Sie Ihr erstelltes Kontaktdaten-XML-Dokument ändern.


Methoden der Klasse Element:

|Methoden|Beschreibung|
|---|---|
|replaceChild(Node newChild, Node oldChild)|Ersetzt das übergebene Kindelement (child element) oldChild durch das Element newChild.|
|setNodeValue(String nodeValue)|Setzt den übergebenen Wert in dem aufrufenden Element.|

Methoden der Klasse Element:

|Methoden|Beschreibung|
|--- |--- |
|removeChild(Node oldChild)|Löscht die übergebene Kindelement aus dem aufrufenden Element.|
|removeAttribute(String name)|Löscht das Attribut mit dem Namen name aus dem aufrufenden Element.|


## 5.1 Beispiel

XML-Dokument ändern und Elemente löschen

In Übung JML-07 haben Sie mit Java ein XML-Dokument erstellt und gespeichert. In dieser Übung soll das in adressen_neu.xml gespeicherte XML-Dokument eingelesen und verändert werden. Um die Daten besser verwalten zu können, soll ein neues Attribut id an dem Kontakt-Element eingefügt werden, das mit einer fortlaufenden Nummer gefüllt werden soll.

Erstellen Sie zusätzlich noch eine Methode, die es erlaubt, Kontaktdaten zu einer als Parameter übergebenen id zu löschen.
Speichern Sie das veränderte XML-Dokument unter einem neuen Namen ab.
Versuchen Sie erst, die Aufgabe selbständig zu lösen. Danach können Sie den Quellcode vergleichen.

Hinweise zur Musterlösung:
Es werden die drei Kontakt-Objekte aus der Datei adressen_neu.xml gelesen, ein Attribut Id wird hinzugefügt, das Kontakt-Objekt mit der Id „2“ wird gelöscht und die übrigen Kontakt-Objekte in die Datei adressen_id.xml geschrieben.

XMLAendern_UXML08.java
```java
import java.io.File;
import java.io.IOException;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.Result;
import javax.xml.transform.Source;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerConfigurationException;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import org.w3c.dom.Element;
import org.w3c.dom.Document;
import org.w3c.dom.NamedNodeMap;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.SAXException;

public class XMLAendern_UXML08 {

    // Pfad und Name der XML Datei die erzeugt werden soll
    private static String XMLDateiName = "adressen_neu.xml";
    private static String XMLDateiNameID = "adressen_id.xml";

    /**
     * Main Methode.
     * 
     * @param args
     *            Uebergabeparameter.
     */
    public static void main(String[] args) {
        try {
            DocumentBuilderFactory factory = DocumentBuilderFactory
                    .newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();
            // Uebergebene XML-Datei parsen
            Document dokument;
            dokument = builder.parse(new File(XMLDateiName));

            // Root Element ermitteln
            Element root = dokument.getDocumentElement();

            // Kontakte mit ID Attribut versehen
            addIdToKontakts(root);

            // Kontakt zur uebergeben ID loeschen
            removeKontaktForID(root, "2");

            // XML Dokument in Datei speichern
            saveXMLDocumentToFile(dokument, XMLDateiNameID);

            // Rueckmeldung an den Benutzer
            System.out.println("XML Datei erfolgreich erstellt.");
        } catch (SAXException e2) {
            e2.printStackTrace();
        } catch (IOException e2) {
            e2.printStackTrace();
        } catch (ParserConfigurationException e) {
            e.printStackTrace();
        }
        return;
    }

    /**
     * Einfuegen eines Attributes ID fuer die Kontakt Elemente.
     * 
     * @param pRoot
     *            Root Element des XML Dokuments.
     */
    private static void addIdToKontakts(Element pRoot) {
        // Alle KontaktElemente ermitteln
        NodeList list = pRoot.getElementsByTagName("Kontakt");
        // Schleife ueber alle Kontakte
        for (int i = 0; i < list.getLength(); i++) {
            Element kontakt = (Element) list.item(i);
            kontakt.setAttribute("id", String.valueOf(i + 1));
        }
    }

    /**
     * Speichert das uebergebene XML Dokument in der uebergebenen Datei.
     * 
     * @param pXMLdocument
     *            zu speicherndes XML Dokument.
     * @param pFileName
     *            Pfad und Name der Datei.
     */
    private static void saveXMLDocumentToFile(Document pXMLdocument,
            String pFileName) {
        try {
            // XML Document in Datei speichern
            TransformerFactory tFactory = TransformerFactory.newInstance();
            Transformer transformer = tFactory.newTransformer();
            Source src = new DOMSource(pXMLdocument);
            File file = new File(pFileName);
            Result dest = new StreamResult(file);
            transformer.transform(src, dest);
        } catch (TransformerConfigurationException e1) {
            e1.printStackTrace();
        } catch (TransformerException e) {
            e.printStackTrace();
        }
    }

    /**
     * Loescht den Kontakt mit der uebergebenen ID aus dem uebergebenen XML
     * Element.
     * 
     * @param pRoot
     *            Element aus dem Kontakt geloescht werden soll.
     * @param pId
     *            ID des Kontakt Elements.
     */
    private static void removeKontaktForID(Element pRoot, String pId) {
        // Alle KontaktElemente ermitteln
        NodeList list = pRoot.getElementsByTagName("Kontakt");
        for (int i = 0; i < list.getLength(); i++) {
            Node kontakt = list.item(i);
            NamedNodeMap attr = kontakt.getAttributes();
            Node typ = attr.getNamedItem("id");
            if (typ.getNodeValue().equals(pId)) {
                pRoot.removeChild(kontakt);
            }
        }
    }

}

```

# 6 XML-Dateien validieren (mit SAX)

Die Funktionalitäten des SAX Parser haben Sie bereits im Abschnitt 5.2 dieses Kapitels kennengelernt. Mit dem SAX Parser ist es möglich ein XML-Dokument gegen eine DTD zu validieren. Dafür muss lediglich der Methode:  
  
setValidating(boolean validating)

der erzeugten SAXParserFactory der Parameter true übergeben werden.

Um die Fehlermeldungen (error messages) des Parser zu behandeln, müssen im implementierten Ereignishandler (event handler) die folgenden Methoden überschrieben (overwrite) werden:

Methoden der Klasse DefaultHandler:

|Methoden|Beschreibung|
|---|---|
|fatalError(SAXParserException e)|Wird aufgerufen, wenn beim Parsen des XML-Dokuments ein fataler Fehler auftritt. Die Abarbeitung des Programms sollte in jedem Fall abgebrochen werden, wenn dieser Fehler auftritt.|
|error(SAXParserException e)|Wird aufgerufen, wenn beim Parsen des XML-Dokuments ein behebarer Fehler auftritt. Fehler dieser Art sollten in eine Log-Datei geschrieben werden oder dem Benutzer angezeigt werden.|
|warning(SAXParserException e)|Wird aufgerufen, wenn der Parser Warnungen meldet. Warnungen sollten in eine Log-Datei geschrieben werden oder dem Benutzer angezeigt werden.|


```java
package jml;

import java.io.FileInputStream;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import org.xml.sax.SAXParseException;
import org.xml.sax.helpers.DefaultHandler;

/**
 * Klasse zum Valisieren eines XML-Dokuments gegen eine DTD.
 *
 * @author Agathe Merceron. (merceron@tfh-berlin.de), Sandra Kaltofen
 * @version 2, 24.02.2010
 **/
public class XMLDTDValidator {
    // Pfad und Name der XML-Datei, die validiert werden soll
    private static String XMLDateiName = "adressen2_dtd.xml";

    /**
     * Main Methode.
     *
     * @param args
     *            Uebergabeparameter
     */
    static public void main(String[] args) {
        readAndDTDValidate(XMLDateiName);
    }

    /**
     * Die Methode liest die uebergebene XML-Datei und validiert sie gegen eine
     * DTD die in der XML-Datei ueber das DOCTYPE statement angegeben ist.
     * Validierungsfehler und Warnungen werden auf der Konsole ausgegeben.
     *
     * @param pathname
     *            Pfad inkl. Dateiname der zu validierenden XML-Datei.
     */
    static private void readAndDTDValidate(String pathname) {
        try {
            // Datei-Eingabestrom erzeugen
            FileInputStream fis = new FileInputStream(pathname);
            // SAX-Parserfactory
            SAXParserFactory myFactory = SAXParserFactory.newInstance();
            // Flag fuer die Validierung auf true setzen
            myFactory.setValidating(true);
            // Flag fuer den Support von Namespaces setzen
            myFactory.setNamespaceAware(true);
            // SAX-Parser erzeugen
            SAXParser myParser = myFactory.newSAXParser();
            // XML-Dokument parsen unter Verwendung des implementierten
            // Ereignishandlers
            System.out.println("Beginn der Validierung");
            myParser.parse(fis, new MyHandler());
            System.out.println("Ende der Validierung");
        }
        // Fangen der IOException und fatal SAXException (XML ist nicht
        // wohlgeformt)
        catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * Implementation des Handlers fuer Sax-Parser-Ereignisse. Ausgabe der
     * Validierungsfehler und -warnungen.
     */
    private static class MyHandler extends DefaultHandler {

        // Ausgabe Validierungsfehler
        @Override
        public void fatalError(SAXParseException e) {
            System.out.println("Fatal error: " + e.getMessage() + " at line : "
                    + e.getLineNumber() + " column " + e.getColumnNumber());
        }

        // Ausgabe Validierungsfehler
        @Override
        public void error(SAXParseException e) {
            System.out.println("Error: " + e.getMessage() + " at line : "
                    + e.getLineNumber() + " column " + e.getColumnNumber());
        }

        // Ausgabe Validierungswarnung
        @Override
        public void warning(SAXParseException e) {
            System.out.println("Warning: " + e.getMessage() + " at line : "
                    + e.getLineNumber() + " column " + e.getColumnNumber());
        }
    }
}
```


## 6.1 Bespiel

Laden Sie den Quellcode von XMLDTDValidator.java in Ihren Editor. Das Programm soll Ihr in Übung JML-04 erstelltes XML-Dokument adressen2_dtd.xml validieren. Achten Sie auf das für die Ausführung notwendige Vorhandensein der Datei adressen.dtd. Kompilieren und testen Sie das Programm.

Verändern Sie das XML-Dokument so, dass es nicht mehr den Vereinbarungen der DTD adressen.dtd entspricht und somit nicht mehr gültig ist. Testen Sie das Programm erneut und schauen SIe sich die vom SAX-Parser erzeugten Fehlermeldungen in der Eclipse-Konsole an.

```java
package jml;

import java.io.FileInputStream;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import org.xml.sax.SAXParseException;
import org.xml.sax.helpers.DefaultHandler;

/**
 * Klasse zum Valisieren eines XML-Dokuments gegen eine DTD.
 * 
 * @author Agathe Merceron. (merceron@tfh-berlin.de), Sandra Kaltofen
 * @version 2, 24.02.2010
 **/
public class XMLDTDValidator {
    // Pfad und Name der XML-Datei, die validiert werden soll
    private static String XMLDateiName = "adressen2_dtd.xml";

    /**
     * Main Methode.
     * 
     * @param args
     *            Uebergabeparameter
     */
    static public void main(String[] args) {
        readAndDTDValidate(XMLDateiName);
    }

    /**
     * Die Methode liest die uebergebene XML-Datei und validiert sie gegen eine
     * DTD die in der XML-Datei ueber das DOCTYPE statement angegeben ist.
     * Validierungsfehler und Warnungen werden auf der Konsole ausgegeben.
     * 
     * @param pathname
     *            Pfad inkl. Dateiname der zu validierenden XML-Datei.
     */
    static private void readAndDTDValidate(String pathname) {
        try {
            // Datei-Eingabestrom erzeugen
            FileInputStream fis = new FileInputStream(pathname);
            // SAX-Parserfactory
            SAXParserFactory myFactory = SAXParserFactory.newInstance();
            // Flag fuer die Validierung auf true setzen
            myFactory.setValidating(true);
            // Flag fuer den Support von Namespaces setzen
            myFactory.setNamespaceAware(true);
            // SAX-Parser erzeugen
            SAXParser myParser = myFactory.newSAXParser();
            // XML-Dokument parsen unter Verwendung des implementierten
            // Ereignishandlers
			System.out.println("Beginn der Validierung");
            myParser.parse(fis, new MyHandler());
			System.out.println("Ende der Validierung");
        }
        // Fangen der IOException und fatal SAXException (XML ist nicht
        // wohlgeformt)
        catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * Implementation des Handlers fuer Sax-Parser-Ereignisse. Ausgabe der
     * Validierungsfehler und -warnungen.
     */
    private static class MyHandler extends DefaultHandler {

        // Ausgabe Validierungsfehler
        @Override
        public void fatalError(SAXParseException e) {
            System.out.println("Fatal error: " + e.getMessage() + " at line : "
                    + e.getLineNumber() + " column " + e.getColumnNumber());
        }

        // Ausgabe Validierungsfehler
        @Override
        public void error(SAXParseException e) {
            System.out.println("Error: " + e.getMessage() + " at line : "
                    + e.getLineNumber() + " column " + e.getColumnNumber());
        }

        // Ausgabe Validierungswarnung
        @Override
        public void warning(SAXParseException e) {
            System.out.println("Warning: " + e.getMessage() + " at line : "
                    + e.getLineNumber() + " column " + e.getColumnNumber());
        }
    }
}

```