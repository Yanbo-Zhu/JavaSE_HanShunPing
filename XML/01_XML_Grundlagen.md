

# 1 解释
XML ist die Abkürzung für Extensible Markup Language
(frei übersetzt: erweiterbare Auszeichnungssprache)

Die Erweiterbarkeit (engl. extensible) bezieht sich auf die Fähigkeit, mit XML eigene Elemente und Attribute definieren und nutzen zu können.

Eine Auszeichnungssprache (engl. Markup language) ist eine Menge von Regeln und Symbolen, um die Struktur eines Dokuments zu definieren

Diese werden in einer Document Type Definition (DTD) oder in einem XML-Schema festgelegt. In der so definierten Sprache sind diese und nur diese Elemente und Attribute "gültig". Dokumente, die diese Sprache verwenden, dürfen nur diese Elemente und Attribute verwenden.

![](image/Pasted%20image%2020230618182648.png)


# 2 Vorteile und Nachteile von XML


- **Vorteile**
	- XML ist erweiterbar.
	- XML ist eindeutig.
	- Eine Trennung von Daten und Darstellung ist möglich.
	- Plattformunabhängigkeit - Programme auf unterschiedlichen Plattformen können mittels XML-Dokumente untereinander kommunizieren.
	- Durch das Textformat ist XML nicht nur für Maschinen sondern auch für den Menschen gut lesbar.
	- Die Syntaxüberprüfung und Validierung von XML-Dokumenten wird von allen modernen Browsern unterstützt.
	- Die Baumstruktur eines XML-Dokuments ist in der internen Darstellung sehr schnell zu bearbeiten.|
- **Nachteile** !
	- Durch die redundante Darstellung (Element-Namen werden immer wiederholt) und die Speicherung im Text und nicht im optimierten binären Format, besteht ein 5- bis 20-fach höherer Speicherplatzbedarf für die Daten. Für die Übertragung sind die Daten allerdings gut komprimierbar.
	- Ein erhöhter Aufwand ergibt sich bei Änderungen in der Elementstruktur.|



# 3 Vergleich zwischen XML, HTML und XHTML


Durch die Verwendung von XML trennt man zwischen den eigentlichen Daten und der Darstellung der Daten. 
Dabei ist XML nur für die Strukturierung der Daten verantwortlich, nicht für deren Formatierung. 

HTML und XHTML werden zur Strukturierung von Inhalten wie Bilder, Texte etc. verwendet. 
 	
XHTML ist die von dem W3C-Konsortium entwickelte Neuformulierung von HTML 4
auf XML-Basis. Bei der Verwendung von XHTML müssen die Syntaxregeln von XML
eingehalten werden.

## 3.1 HTML und XML

" HTML was designed to display data and to focus on how data looks."
"XML was designed to describe data and to focus on what data is." 

In XML ist es möglich eigene, beliebige Elemente und Attribute zu definieren.
HTML ist statisch und kann nicht um eigene Elemente und Attribute erweitert werden.

## 3.2 HTML und XHTML

In HTML und XHTML werden die Daten und ihre Darstellung miteinander vermischt.


# 4 缩写 Akronyme

|x|x|
|---|---|
|DTD|Document Type Definition|
|CSS|Cascading Style Sheets|
|SGML|standard generalized markup languagestandard generalized markup language|
|XHTML|standard generalized markup language|
|XSD|XML Schema Definition|
|DOM|Document Object Model |
|XML|Extensible Markup Language|

