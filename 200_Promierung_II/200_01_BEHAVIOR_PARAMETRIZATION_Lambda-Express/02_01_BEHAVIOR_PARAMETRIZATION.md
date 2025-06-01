
# 1 理论 


![](image/Pasted%20image%2020250502205949.png)



Value Parametrization
- Bezieht sich auf das Übergeben von konkreten Werten an eine Funktion oder Methode
- Diese Werte können Argumente einer Methode sein, die das Verhalten der Methode beeinflussen, aber die Logik selbst bleibt unverändert

 Behavior Parametrization
- Bedeutet, dass das Verhalten einer Methode durch eine übergebene Funktion verändert werden kann
- Anstatt nur Werte zu übergeben, wird eine Strategie oder ein Verhalten als Argument übergeben
- Dies ermöglicht eine höhere Wiederverwendbarkeit und Flexibilität, da der Code nicht für jede spezifische Operation angepasst werden muss

Warum Konstanten in der funktionalen Programmierung so wichtig sind
![](image/Pasted%20image%2020250601173714.png)


# 2 Beispiel

```java
// Interface für ein Verhalten
interface ApplePredicate {
    boolean test(Apple apple);
}

// Methode, die ein Verhalten erwartet
List<Apple> filterApples(List<Apple> inventory, ApplePredicate p) {
    List<Apple> result = new ArrayList<>();
    for (Apple apple : inventory) {
        if (p.test(apple)) {
            result.add(apple);
        }
    }
    return result;
}
```

Aufruf sähe so aus:
```java
// Konkrete Implementierung des Verhaltens
class GreenApplePredicate implements ApplePredicate {
    public boolean test(Apple apple) {
        return "green".equals(apple.getColor());
    }
}


// Aufruf der filterApples-Methode
List<Apple> greenApples = filterApples(inventory, new GreenApplePredicate());
```


Oder in super kurz (ab Java 8, mit Lambda):
```java
List<Apple> greenApples = filterApples(inventory, apple -> "green".equals(apple.getColor()));
```


---

![](image/Pasted%20image%2020250502210043.png)


![](image/Pasted%20image%2020250502210053.png)


![](image/Pasted%20image%2020250502210112.png)


![](image/Pasted%20image%2020250502210130.png)


---

![](image/Pasted%20image%2020250502210142.png)


![](image/Pasted%20image%2020250502210157.png)


![](image/Pasted%20image%2020250502210206.png)



![](image/Pasted%20image%2020250502210219.png)


---


![](image/Pasted%20image%2020250502210311.png)


---

![](image/Pasted%20image%2020250502210303.png)

![](image/Pasted%20image%2020250502210327.png)



# 3 Beispiel 2 


In einem Java Programm soll eine Liste von Büchern nach bestimmten Kriterien gefiltert werden, z.B. nach Autor oder Erscheinungsjahr. In dieser Aufgabe geht es darum, wie man solche Filter flexibel gestalten kann, ohne den Code bei jeder neuen Anforderung ändern zu müssen.

1. Erstellen Sie eine Klasse Book mit den Eigenschaften title, author und year. Implementieren Sie dazu die passenden Konstruktoren und Getter-Methoden.

2. Implementieren Sie die folgenden Methoden:
// Filtert Bücher, deren Erscheinungsjahr nach dem angegebenen Jahr liegt.
`static List<Book> filterBooksAfterYear(List<Book> books, int year)`
// Filtert Bücher eines bestimmten Autors.
`static List<Book> filterBooksByAuthor(List<Book> books, String author)`

3. Was passiert, wenn Sie mehrere Filterkriterien gleichzeitig anwenden möchten? Welche Änderungen sind notwendig, um beispielsweise nur Bücher mit dem Titel "Java" und
einem Erscheinungsjahr nach 2018 zu finden?


4. Betrachten Sie das folgende Interface:
```
public interface BookFilterStrategy {
boolean test(Book book);
}
```

Erstellen Sie die folgenden Klassen, die dieses Interface implementieren:
• AuthorFilter – Filtert Bücher eines bestimmten Autors.
• YearAfterFilter – Filtert Bücher ab einem bestimmten Erscheinungsjahr.
Wie können wir dieses Interface nutzen, um die Flexibilitätsprobleme aus der vorherigen Frage zu lösen? Implementieren Sie eine Methode filterBooks, die es ermöglicht, beliebige Filterkriterien (können auch mehrere sein) anzuwenden.


--- 

解答

```
public interface BookFilterStrategy {
    boolean test(Book book);

}
```


```
public class YearAfterFilter implements BookFilterStrategy {
    private int year;

    public YearAfterFilter(int year) {
        this.year = year;
    }

    @Override
    public boolean test(Book book) {
        return book.getYear() > year;
    }
}
```


```
public class AuthorFilter implements BookFilterStrategy {
    private String author;

    public AuthorFilter(String author) {
        this.author = author;
    }

    @Override
    public boolean test(Book book) {
        return book.getAuthor().equalsIgnoreCase(author) ;
    }
}
```


```

import java.util.ArrayList;
import java.util.List;

public class Book {
    private String title;
    private String author;
    private int year;

    public Book(String title, String author, int year) {
        this.title = title;
        this.author = author;
        this.year = year;
    }

    public String getTitle() {
        return title;
    }

    public String getAuthor() {
        return author;
    }

    public int getYear() {
        return year;
    }

    @Override
    public String toString() {
        return "Book{" +
                "title='" + title + '\'' +
                ", author='" + author + '\'' +
                ", year=" + year +
                '}';
    }

    // Filtert Bücher, deren Erscheinungsjahr nach dem angegebenen Jahr liegt.
    static List<Book> filterBooksAfterYear(List<Book> books, int year) {
        List<Book> filteredBooks = new ArrayList<>();
        for (Book book : books) {
            if (book.getYear() > year) {
                filteredBooks.add(book);
            }
        }
        return filteredBooks;
    }


    // Filtert Bücher eines bestimmten Autors.
    static List<Book> filterBooksByAuthor(List<Book> books, String author) {
        List<Book> filteredBooks = new ArrayList<>();
        for (Book book : books) {
            if (book.getAuthor().equalsIgnoreCase(author)) {
                filteredBooks.add(book);
            }
        }
        return filteredBooks;
    }

    // varrags mit BookFilterStrategy
    // 重点理解 varrags 
    // BookFilterStrategy ist eine funktionale Schnittstelle, die eine Methode test() definiert.
    // Book.filterBookks (Buecher1, new YearAfterFilter(1940), new AuthorFilter("George Orwell"));
    public static ArrayList<Book> filterBooks(List<Book> books, BookFilterStrategy... filters) {
        ArrayList<Book> filteredBooks = new ArrayList<>();
        for (Book book : books) {
            boolean matchesAll = true; // variable, wird buch in die List oder nicht
            for (BookFilterStrategy filter : filters) {
                if (!filter.test(book)) {
                    matchesAll = false;
                    break;
                }
            }
            if (matchesAll) {
                filteredBooks.add(book);
            }
        }

        return filteredBooks;
    }

    public static void main(String[] args) {
        ArrayList<Book> books = new ArrayList<>();
        books.add(new Book("1984", "George Orwell", 1949));
        books.add(new Book("Brave New World", "Aldous Huxley", 1932));
        books.add(new Book("Fahrenheit 451", "Ray Bradbury", 1953));

        BookFilterStrategy authorFilter = new AuthorFilter("George Orwell");

        BookFilterStrategy yearAfterFilter = new YearAfterFilter(1940);

        //filterBooksAfterYear(books, 1940);
    }

}


```