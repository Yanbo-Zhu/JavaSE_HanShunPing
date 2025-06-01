




![](image/Pasted%20image%2020250502211329.png)


**lambda-Ausdrücke** sind kurze (concise) Repräsentationen anonymer Funktionen, d.h. sie...
- ... haben keinen Namen,
- ... können wir Werte behandelt werden,
- ... sind nicht Bestandteil einer Klasse (vgl. Methoden),
- ... können als Argument an eine Methode übergeben werden,
- ... können als Ergebnis einer Methode zurückgegeben werden und
- ... können in einer Variable gespeichert werden.


Lambda-Ausdrücke bestehen aus...
- ... einer Liste mit Argumenten (**geschlossene** oder **gebundene Variablen**, **closed variables**), 
- ... einem Operator `**->**` (häufig gesprochen "wird zu" oder "becomes") und
- ... einem **Rumpf** (**Body**), der einen Ausdruck oder eine oder mehrere Anweisungen enthalten kann.


# 1 Die Syntax der Lambda-Ausdrücke


|Variante|Beschreibung|Beispiel|
|---|---|---|
|Ohne Argumente|Wenn keine Eingabe nötig ist|`**() -> System.out.println("Hi")**`|
|Ein Argument, ohne Klammern|Klammern können weggelassen werden|`**x -> x+1**`|
|Ein Argument, mit Klammern|Optional|`**(x) -> x+1**`|
|Ein Argument mit Typangabe|Klammern obligatorisch|**`(int x) -> x+1`**|
|Mehrere Argumente|Klammern und Kommas nötig|`**(x,y) -> x+y**`|
|Mehrere Argumente mit Typangabe|Klammern und Kommas nötig|`**(int x, int y) -> x+y**`|
|Block mit Rückgabe|Wenn mehrere Anweisungen nötig sind|`**(x,y) -> {int sum=x+y; return sum;}**`|
|Ohne Rückgabe (void)|Für Seiteneffekte wie Logging|`**x -> System.out.println(x)**`|

- Lambda-Ausdrücke verwenden in der Regel Typableitungen mit Hilfe des zugrundeliegenden Interfaces, d.h. der Datentyp der Argumente muss nicht angegeben werden
- Allerdings erhöht die explizite Typangabe die Lesbarkeit des Codes


# 2 Functional Descriptor 


Funktionstyp 	Aufruf-Methode
Function 	.apply()
UnaryOperator 	.apply()
Predicate 	.test()
Supplier 	.get()
Consumer 	.accept()

## 2.1 Functional Descriptor am Beispiel von Predicate


![](image/Pasted%20image%2020250502211713.png)

- Lambda-Ausdrücke sind Instanzen vom Typ eines funktionalen Interfaces
- Die Verwendung eines Lambda-Ausdrucks setzt voraus, dass es ein entsprechendes Functional Interface gibt, dessen Methode eine Signatur (**Functional Descriptor**) aufweist, die der Signatur des Lambda-Ausdrucks entsprechen muss
- Methodenargumente, welchen Lambda-Ausdrücke bei Aufruf übergeben werden, und Variablen, die Lambda-Ausdrücke speichern, müssen vom Typ des assoziierten Functional Interface sein
- Java liefert eine umfangreiche Bibliothek vorgefertigter Functional Interfaces


- **Lambda 表达式是函数式接口类型的实例**
- **使用 Lambda 表达式的前提是：必须存在一个对应的函数式接口，该接口的方法具有与 Lambda 表达式相匹配的方法签名（即函数描述符）**
- **将 Lambda 表达式作为参数传递给方法，或者将其赋值给变量时，这些方法的参数或变量的类型必须是相应的函数式接口类型**
- **Java 提供了一个包含多种预定义函数式接口的丰富库**


---


在 Java 中，一个函数式接口的 **函数描述符（Functional Descriptor）** 就是它**唯一抽象方法**的签名。  
对于 `Predicate<T>` 接口，其函数描述符是： `boolean test(T t);`
这意味着任何可以匹配这个方法签名的 Lambda 表达式都可以被赋值给一个 `Predicate<T>` 类型的变量。


```
import java.util.function.Predicate;

public class PredicateExample {
    public static void main(String[] args) {
        // Lambda 表达式，符合函数描述符：boolean test(T t)
        Predicate<String> isLongerThan5 = s -> s.length() > 5;

        System.out.println(isLongerThan5.test("Hello"));     // false
        System.out.println(isLongerThan5.test("Functional")); // true
    }
}

```

```
s -> s.length() > 5 
```

`与 Predicate<String> 接口的方法：`

```
boolean test(String s);
```

完全一致 —— 即这个 Lambda 表达式实现了 test 方法的签名（这就是所谓的 Functional Descriptor）。



## 2.2 Functional Descriptor am Beispiel von Consumer

![](image/Pasted%20image%2020250502212153.png)


`Consumer<T> `是 Java 提供的一个函数式接口，它表示一个接受一个参数但不返回值的操作。

它的 函数描述符（Functional Descriptor） 是： `void accept(T t);`

```
import java.util.function.Consumer;

public class ConsumerExample {
    public static void main(String[] args) {
        // Lambda 表达式符合 Consumer 的函数描述符：void accept(T t)
        Consumer<String> printUpperCase = s -> System.out.println(s.toUpperCase());

        // 使用 Consumer
        printUpperCase.accept("hello");  // 输出：HELLO
    }
}
```


``s -> System.out.println(s.toUpperCase());``
符合 `Consumer<String>` 的抽象方法：
`void accept(String s);`
这说明该 Lambda 表达式可以作为 `Consumer<String>` 的实现。



## 2.3 Functional Descriptor am Beispiel von Function

![](image/Pasted%20image%2020250502212251.png)

在 Java 中，`Function<T, R>` 是一个常用的函数式接口，表示接收一个类型为 `T` 的参数，并返回一个类型为 `R` 的结果。`Function` 接口的抽象方法是 `R apply(T t)`，它接收一个输入并返回一个输出。


对于 `Function<T, R>`，其功能是接收一个类型为 `T` 的输入，并返回类型为 `R` 的输出。下面我们来看一个实际的例子。

```
import java.util.function.Function;

public class FunctionExample {
    public static void main(String[] args) {
        // Function: 将字符串转换为大写
        Function<String, String> toUpperCase = s -> s.toUpperCase();
        
        // 调用 apply 方法
        String result = toUpperCase.apply("hello");
        System.out.println(result);  // 输出：HELLO
    }
}
```

- `Function<String, String>` 表示接收一个 `String` 类型的输入，并返回一个 `String` 类型的输出。
    
- `apply` 方法执行 `toUpperCase` 操作，将 "hello" 转换为 "HELLO"。


---

链式调用多个 Function

我们还可以使用 andThen 或 compose 方法将多个 Function 连接在一起，实现链式调用。
使用 andThen 链接多个函数：


```
import java.util.function.Function;

public class FunctionChainingExample {
    public static void main(String[] args) {
        // 第一个 Function: 将字符串转换为大写
        Function<String, String> toUpperCase = s -> s.toUpperCase();
        
        // 第二个 Function: 在字符串后加上 " World"
        Function<String, String> appendWorld = s -> s + " World";
        
        // 使用 andThen 链接两个 Function
        Function<String, String> resultFunction = toUpperCase.andThen(appendWorld);
        
        // 调用 apply 方法
        String result = resultFunction.apply("hello");
        System.out.println(result);  // 输出：HELLO World
    }
}
```

- `toUpperCase.andThen(appendWorld)`：先将字符串转换为大写，再在结果后添加 " World"。
    
- `apply` 方法执行链式操作，最终输出 `HELLO World`。



## 2.4 预定义的函数式接口库 in java.util.function


| 接口名                 | 参数类型   | 返回类型      | 方法名                 | 功能说明                           | 示例 Lambda 表达式                         |
| ------------------- | ------ | --------- | ------------------- | ------------------------------ | ------------------------------------- |
| `Predicate<T>`      | `T`    | `boolean` | `test(T t)`         | 判断条件（返回 true/false）            | `x -> x > 10`                         |
| `Function<T,R>`     | `T`    | `R`       | `apply(T t)`        | 输入 T，返回 R. 可以返回不同的数据类型的值       | `x -> x.length()`                     |
| `Consumer<T>`       | `T`    | `void`    | `accept(T t)`       | 消费一个值（仅执行，无返回）                 | `x -> System.out.println(x)`          |
| `Supplier<T>`       | 无参数    | `T`       | `get()`             | 提供一个值（返回，无输入）                  | `() -> "Hello"`                       |
| `UnaryOperator<T>`  | `T`    | `T`       | `apply(T t)`        | 一元操作（输入输出类型相同） . 必须返回同一个数据类型的值 | `x -> x * x`                          |
| `BinaryOperator<T>` | `T, T` | `T`       | `apply(T t1, T t2)` | 二元操作                           | `(a, b) -> a + b`                     |
| `BiFunction<T,U,R>` | `T, U` | `R`       | `apply(T, U)`       | 两个输入参数，返回结果                    | `(a, b) -> a + b.length()`            |
| `BiPredicate<T,U>`  | `T, U` | `boolean` | `test(T, U)`        | 判断两个输入条件                       | `(a, b) -> a > b`                     |
| `BiConsumer<T,U>`   | `T, U` | `void`    | `accept(T,U)`       | 消费两个输入值                        | `(a, b) -> System.out.println(a + b)` |

```
Predicate<String> checkLength = s -> s.length() > 5;
Function<String, Integer> getLength = s -> s.length();
Consumer<String> print = s -> System.out.println(s);
Supplier<String> greet = () -> "Hello World";
UnaryOperator<Integer> square = x -> x * x;
BinaryOperator<Integer> sum = (a, b) -> a + b;

```


---

Consumer、Predicate 和 Function 都是 Java 中的 函数式接口，它们有不同的用途和签名。下面详细比较它们的不同点：

|项目|`Consumer<T>`|`Predicate<T>`|`Function<T, R>`|
|---|---|---|---|
|作用|接收一个输入参数，**执行操作但不返回值**|接收一个输入参数，**返回布尔值**|接收一个输入参数，**返回另一个值**|
|常见用途|执行副作用操作（如打印、存储数据）|用于判断（如过滤、条件判断）|用于转换数据（如映射、计算）|

函数描述符（Functional Descriptor）

| 接口               | 抽象方法签名              |
| ---------------- | ------------------- |
| `Consumer<T>`    | `void accept(T t)`  |
| `Predicate<T>`   | `boolean test(T t)` |
| `Function<T, R>` | `R apply(T t)`      |


- **`Consumer`**：接收一个输入参数，但**不返回任何值**。适用于那些执行某些操作（例如打印、修改变量、存储数据等）而不需要返回结果的场景。
    
- **`Predicate`**：接收一个输入参数，**返回一个布尔值**。通常用于条件判断、过滤操作等。
    
- **`Function`**：接收一个输入参数，**返回一个输出值**。它通常用于将一个值映射到另一个值，常见于数据转换、处理等。


![](image/Pasted%20image%2020250502215445.png)

# 3 primitive specialization




| Functional Interface    | Function Descriptor  | Primitive Specializations                                                                                                                                                                                                                                                                                             |
| ----------------------- | -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `**Predicate<T>**`      | `**T->boolean**`     | `**IntPredicate**`, `**LongPredicate**`, `**DoublePredicate**`                                                                                                                                                                                                                                                        |
| `**Consumer<T>**`       | `**T->void**`        | `**IntConsumer**`, `**LongConsumer**`, `**DoubleConsumer**`                                                                                                                                                                                                                                                           |
| `**Function<T, R>**`    | `**T->R**`           | `**IntFunction<R>**`, `**DoubleFunction<R>**`, `**LongFunction<R>**`, `**ToIntFunction<T>**`, `**ToDoubleFunction<T>**`, `**ToLongFunction<T>**`, `**IntToDoubleFunction**`, `**IntToLongFunction**`,  <br>`**DoubleToIntFunction**`, `**DoubleToLongFunction**`, `**LongToDoubleFunction**`, `**LongToIntFunction**` |
| `**Supplier<T>**`       | `**()->T**`          | `**BooleanSupplier**`, `**IntSupplier**`, `**LongSupplier**`, `**DoubleSupplier**`                                                                                                                                                                                                                                    |
| `**UnaryOperator<T>**`  | `**T->T**`           | `**IntUnaryOperator**`, `**LongUnaryOperator**`, `**DoubleUnaryOperator**`                                                                                                                                                                                                                                            |
| `**BinaryOperator<T>**` | `**(T,T)->T**`       | `**IntBinaryOperator**`, `**LongBinaryOperator**`, `**DoubleBinaryOperator**`                                                                                                                                                                                                                                         |
| `**BiPredicate<L,R>**`  | `**(L,R)->boolean**` |                                                                                                                                                                                                                                                                                                                       |
| `**BiConsumer<T,U>**`   | `**(T,U)->void**`    | `**ObjIntConsumer<T>**`, `**ObjLongConsumer<T>**`, `**ObjDoubleConsumer<T>**`                                                                                                                                                                                                                                         |
| `**BiFunction<T,U,R>**` | `**(T,U)->R**`       | `**ToIntBiFunction<T,U>**`, `**ToLongBiFunction<T,U>**`, `**ToDoubleBiFunction<T,U>**`                                                                                                                                                                                                                                |



- enerische Parameter (`**<R>**`, `**<S>**`, `**<T>**` usw.) können in Java nur an Referenztypen gebunden werden
- **Boxing** wandelt primitive Typen in Referenztypen um, z.B. `**int**` zu `**Integer**`, **Unboxing** bezeichnet den umgekehrten Vorgang
- **Autoboxing** führt je nach Bedarf Boxing oder Unboxing automatisch durch
- Problem: "Boxed Values" benötigen mehr Speicherplatz und verursachen mehr Speicherzugriffe als Werte primitiver Typen
- **Primitive Spezialisierungen** sind funktionale Interfaces, die nur auf primitiven Datentypen basieren und Autoboxing nicht zulassen

---


In **Java**, _primitive specializations of functional interfaces_ refer to specialized versions of the standard functional interfaces (like `Function`, `Predicate`, etc.) that are optimized for **primitive types** such as `int`, `double`, and `long`.

This specialization exists to **avoid boxing and unboxing overhead**, which can negatively impact performance when using primitive types in lambda expressions or functional programming patterns.


Problem with Generic Functional Interfaces: 
Standard functional interfaces like `Function<T, R>` only work with **reference types** (`Integer`, `Double`, etc.), not primitives (`int`, `double`):
```
Function<Integer, Integer> f = x -> x + 1;  // boxing occurs here
```

This leads to:
- `int` → `Integer` (boxing)    
- `Integer` → `int` (unboxing)


---

Java’s Solution: Primitive Specializations
Java provides primitive-specific versions of functional interfaces in java.util.function, such as:


|Use Case|Generic Interface|Primitive Specialized Version|
|---|---|---|
|`int` → `int`|`Function<Integer, Integer>`|`IntUnaryOperator`|
|`int` → `double`|`Function<Integer, Double>`|`IntToDoubleFunction`|
|`double` → `boolean`|`Predicate<Double>`|`DoublePredicate`|
|`int` → void|`Consumer<Integer>`|`IntConsumer`|
|no input → `int`|`Supplier<Integer>`|`IntSupplier`|

---

Example: With and Without Specialization

Without Specialization (boxing involved):
```
Function<Integer, Integer> addOne = x -> x + 1;
int result = addOne.apply(5);  // Boxed to Integer
```


With Primitive Specialization:
```
IntUnaryOperator addOne = x -> x + 1;
int result = addOne.applyAsInt(5);  // No boxing
```
`applyAsInt(int operand)` is the specialized method that operates directly on primitives.


---

Why it Matters
- ✅ **Performance**: Avoids runtime overhead caused by boxing.
- ✅ **Memory-efficient**: Less garbage generated (no unnecessary wrapper objects).
- ✅ **Cleaner code**: Still uses lambdas and functional style, but with better efficiency.


## 3.1 例子

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.*;


// Lässt sich jedes Predicate auch durch eine Function realisieren? Wenn ja, können wir die folgende Methode mit einer Function aufrufen (wie im zweiten Beispielaufruf unten)


public class DifferencePredicateAndFunction {
    // Methode
    public static List<Integer> filterList(List<Integer> list, Predicate<Integer>
            predicate) {
        List<Integer> result = new ArrayList<>();
        for (Integer i : list) {
            if (predicate.test(i)) {
                result.add(i);
            }
        }
        return result;
    }



    // Aufrufe
    public static void main(String[] args) {

        // Parameter
        List<Integer> list = Arrays.asList(1,2,3,4,5);
        Predicate<Integer> p = x -> x % 2 == 0;
        Function<Integer,Boolean> f = x -> x % 2 == 0;

        // Using Predicate directly. In Predicate, the test method is used to check the condition.
        //filterList(list, p);

        // Using Function directly. In Function, the apply method is used to check the condition. there is no test method. Therefore. (predicate.test(i)) does not work.
        //filterList(list,f);

        // Valid: using Predicate
        List<Integer> filteredByPredicate = filterList(list, p);
        System.out.println("Filtered (Predicate): " + filteredByPredicate);

        // Invalid: Function is not a Predicate
        // This line will NOT compile:
        // List<Integer> filteredByFunction = filterList(list, f);

        // Convert Function to Predicate explicitly if needed:
        Predicate<Integer> convertedPredicate = f::apply;
        List<Integer> filteredByConvertedFunction = filterList(list, convertedPredicate);
        System.out.println("Filtered (Function as Predicate): " + filteredByConvertedFunction);

    }

}
```

# 4 Methodenreferenzen

- Eine **Methodenreferenz** ist eine kompakte Schreibweise für einen Lambda-Ausdruck, der einfach nur eine Methode aufruft, ohne weitere Logik
- Eine Methodenreferenz kann anstelle eines Lambda-Ausdrucks immer dann verwendet werden, wenn das Lambda nur eine bestimmte Methode weiterreicht, z.b. als Predicate, Function oder Consumer
- Links vom `**::**` steht entweder ein Klassenname oder ein Objekt – je nachdem, was referenziert wird

- **方法引用是一种紧凑的写法，用于表示仅调用一个方法、不包含其他逻辑的 Lambda 表达式**
- **只要 Lambda 表达式仅是转交一个特定方法（例如用于 `Predicate`、`Function` 或 `Consumer`），就可以使用方法引用来替代它**
- **在 `::` 左边的是类名或对象名——取决于引用的是哪种类型的方法**


![](image/Pasted%20image%2020250502212451.png)


![](image/Pasted%20image%2020250502212557.png)



## 4.1 例子 

引用静态方法

```
public class Utils {
    public static boolean isEven(int number) {
        return number % 2 == 0;
    }
}


// Lambda 写法：
Predicate<Integer> lambda = (n) -> Utils.isEven(n);

// 方法引用写法：
Predicate<Integer> reference = Utils::isEven;
```



引用实例方法（通过对象）
```
public class Printer {
    public void print(String message) {
        System.out.println(message);
    }
}

Printer printer = new Printer();


// Lambda 写法：
Consumer<String> lambda = (msg) -> printer.print(msg);

// 方法引用写法：
Consumer<String> reference = printer::print;

```


引用实例方法（通过类名）
```
// 适用于 List<String>，想用 String 的 equals 方法来比较某值：
List<String> names = List.of("Alice", "Bob", "Charlie");

// Lambda 写法：
names.stream().filter(name -> name.equals("Alice")).forEach(System.out::println);

// 方法引用写法：
names.stream().filter("Alice"::equals).forEach(System.out::println);
```


构造器引用
```
// Lambda 写法：
Supplier<List<String>> lambda = () -> new ArrayList<>();

// 构造器引用写法：
Supplier<List<String>> reference = ArrayList::new;
```


示例程序：Methodenreferenz in Java
```java
import java.util.*;
import java.util.function.*;
import java.util.stream.*;

public class MethodenreferenzBeispiel {

    // 静态方法
    public static boolean isEven(int number) {
        return number % 2 == 0;
    }

    // 一个普通的类，包含实例方法
    static class Printer {
        public void print(String message) {
            System.out.println("打印内容: " + message);
        }
    }

    public static void main(String[] args) {

        // 🔹 1. 静态方法引用
        // 方法引用写法:
        Predicate<Integer> staticRef = MethodenreferenzBeispiel::isEven;

        // 等价 Lambda 表达式:
        // Predicate<Integer> staticRef = n -> MethodenreferenzBeispiel.isEven(n);

        System.out.println("8 ist gerade: " + staticRef.test(8));  // 输出: true

        // 🔹 2. 实例方法引用（通过对象）
        Printer printer = new Printer();

        // 方法引用写法:
        Consumer<String> objectRef = printer::print;

        // 等价 Lambda 表达式:
        // Consumer<String> objectRef = msg -> printer.print(msg);

        objectRef.accept("Hallo Welt!");  // 输出: 打印内容: Hallo Welt!

        // 🔹 3. 实例方法引用（通过类名）
        List<String> namen = List.of("Alice", "Bob", "Anna", "Alex");

        // 这里不能用 "A"::equalsIgnoreCase 来过滤以 "A" 开头的字符串，
        // 因为 equalsIgnoreCase 是一个对象比较，而不是以字母开头。

        // 正确做法，用 Lambda 表达式过滤以 "A" 开头：
        System.out.println("Filter mit Lambda:");
        namen.stream()
             .filter(name -> name.startsWith("A"))  // 不能用方法引用
             .forEach(System.out::println);

        // 🔹 4. 构造器引用
        // 方法引用写法:
        Supplier<List<String>> constructorRef = ArrayList::new;

        // 等价 Lambda 表达式:
        // Supplier<List<String>> constructorRef = () -> new ArrayList<>();

        List<String> neueListe = constructorRef.get();
        neueListe.add("Neu");
        System.out.println("Neue Liste: " + neueListe);  // 输出: Neue Liste: [Neu]
    }
}

```


output
```
8 ist gerade: true
打印内容: Hallo Welt!
Filter: Namen, die mit 'A' beginnen:
Filter mit Lambda:
Alice
Anna
Alex
Neue Liste: [Neu]
```



|方法引用形式|示例|等价 Lambda 表达式|
|---|---|---|
|静态方法引用|`Klasse::statischeMethode`|`x -> Klasse.statischeMethode(x)`|
|对象的实例方法引用|`objekt::instanzMethode`|`x -> objekt.instanzMethode(x)`|
|类的实例方法引用|`Klasse::instanzMethode`|`(obj) -> obj.instanzMethode()`|
|构造器引用|`Klasse::new`|`() -> new Klasse()`|


## 4.2 例子2

```
import java.util.function.Predicate;


// Implementieren Sie eine Klasse namens TestOdd, die einen Integer daraufhin überprüfen
//soll, ob dieser ungerade ist. Wählen Sie hierfür ein passendes funktionales Interface, das
//von TestOdd implementiert wird.

public class TestOdd implements Predicate<Integer> {
    @Override
    public boolean test(Integer integer) {
        return integer % 2 != 1;
    }
    
}

// Equivalent Lambda function i -> i % 2 == 1
```


```
import java.util.function.BinaryOperator;
import java.util.function.UnaryOperator;

// mplementieren Sie nun eine Klasse namens Multiply, die zwei übergebene Integer
//miteinander multipliziert und das Ergebnis zurückgibt. Wählen Sie hierfür ein passendes
//funktionales Interface, das von Multiply implementiert wird.

public class Multiply implements BinaryOperator<Double> {
    
    @Override
    public Double apply (Double a, Double b) {
        return a * b;
    }
}

// Equivalent Lambda function (a, b) -> a * b

```


```
import java.util.function.UnaryOperator;


// Implementieren Sie eine Klasse namens MapPlusThree, die einen Integer erhält, diesen
//um 3 erhöht und anschließend zurückgibt. Wählen Sie hierfür ein passendes funktionales
//Interface, das von MapPlusThree implementiert wird

public class MapPlusThree implements UnaryOperator<Integer> { 
    @Override
    public Integer apply(Integer integer) {
        return integer + 3;
    }
    
    
}

// Equivalent Lambda function  i -> i + 3

```

# 5 funktional interface 


在 Java 中，**funktionale Interfaces**（英文：**functional interfaces**）是只包含一个 **abstrakten Methoden**（抽象方法）的接口。它们是 Java 8 引入 Lambda 表达式时的核心概念。

一个 **Functional Interface** 是一个接口，其中 **只有一个抽象方法**。它可以有多个 `default` 或 `static` 方法，但只能有一个抽象方法。

你可以用注解 `@FunctionalInterface` 来标记它，帮助编译器检查：

```
@FunctionalInterface
public interface MyFunction {
    int apply(int x, int y);
}
```


它们用于：
- Lambda 表达式
- 方法引用
- 函数式编程风格（Streams、Optional、Predicate 等）

Lambda 使用 Functional Interface

```
Function<String, Integer> stringLength = s -> s.length();
System.out.println(stringLength.apply("hello")); // 输出 5
```


## 5.1 常见的 Java 内置 Functional Interfaces：

|接口|抽象方法签名|用途|
|---|---|---|
|`Runnable`|`void run()`|无参无返回值的任务|
|`Callable<T>`|`T call()`|无参但有返回值的任务|
|`Consumer<T>`|`void accept(T t)`|接收一个参数，没有返回值|
|`Supplier<T>`|`T get()`|提供一个值，没有参数|
|`Function<T,R>`|`R apply(T t)`|输入一个参数，返回一个结果|
|`Predicate<T>`|`boolean test(T t)`|判断一个条件，返回 true/false|
|`BiFunction<T,U,R>`|`R apply(T t, U u)`|两个参数输入，一个结果输出|

# 6 function Interface 的例子


```java
package functions;  
  
public class FunctionsJava {  
  
    // A)  
    public int multiply(int x, int y) {  
        return x * y;  
    }  
  
    // B)  
    public double divide(double x, double y) {  
        if (y == 0) {  
            return 0; // eigentlich würden wir hier gerne eine Warnung ausgeben, statt eine Zahl zurückzugeben  
        }  
  
        return x / y;  
    }  
  
    // C)  
    public void printSum(int x, int y) {  
        System.out.println(x + " + " + y + " = " + (x+y));  
    }  
  
    public static void main(String[] args) {  
        FunctionsJava f = new FunctionsJava();  
  
        // A)  
        System.out.println("A");  
        System.out.println(f.multiply(10, 5));  
        System.out.println(f.multiply(10, -5));  
  
        // B)  
        System.out.println("B");  
        System.out.println(f.divide(10, 5));  
        System.out.println(f.divide(10, 0));  
  
        // C)  
        System.out.println("C");  
        f.printSum(10, 5);  
        f.printSum(10, -5);  
    }  
}
```

Multiply.java
```
import java.util.function.BinaryOperator;  
import java.util.function.UnaryOperator;  
  
// mplementieren Sie nun eine Klasse namens Multiply, die zwei übergebene Integer  
//miteinander multipliziert und das Ergebnis zurückgibt. Wählen Sie hierfür ein passendes  
//funktionales Interface, das von Multiply implementiert wird.  
  
public class Multiply implements BinaryOperator<Double> {  
      
    @Override  
    public Double apply (Double a, Double b) {  
        return a * b;  
    }  
}  
  
// Equivalent Lambda function (a, b) -> a * b
```


MapPlusThree.java
```
import java.util.function.UnaryOperator;  
  
  
// Implementieren Sie eine Klasse namens MapPlusThree, die einen Integer erhält, diesen  
//um 3 erhöht und anschließend zurückgibt. Wählen Sie hierfür ein passendes funktionales  
//Interface, das von MapPlusThree implementiert wird  
  
public class MapPlusThree implements UnaryOperator<Integer> {   
    @Override  
    public Integer apply(Integer integer) {  
        return integer + 3;  
    }  
      
      
}  
  
// Equivalent Lambda function  i -> i + 3
```

TestOdd.java
```
import java.util.function.Predicate;  
  
  
// Implementieren Sie eine Klasse namens TestOdd, die einen Integer daraufhin überprüfen  
//soll, ob dieser ungerade ist. Wählen Sie hierfür ein passendes funktionales Interface, das  
//von TestOdd implementiert wird.  
  
public class TestOdd implements Predicate<Integer> {  
    @Override  
    public boolean test(Integer integer) {  
        return integer % 2 != 1;  
    }  
      
}  
  
// Equivalent Lambda function i -> i % 2 == 1
```


DifferencePredicateAndFunction.java
```
import java.util.ArrayList;  
import java.util.Arrays;  
import java.util.List;  
import java.util.function.*;  
  
  
// Lässt sich jedes Predicate auch durch eine Function realisieren? Wenn ja, können wir die folgende Methode mit einer Function aufrufen (wie im zweiten Beispielaufruf unten)  
  
  
public class DifferencePredicateAndFunction {  
    // Methode  
    public static List<Integer> filterList(List<Integer> list, Predicate<Integer>  
            predicate) {  
        List<Integer> result = new ArrayList<>();  
        for (Integer i : list) {  
            if (predicate.test(i)) {  
                result.add(i);  
            }  
        }  
        return result;  
    }  
  
  
  
    // Aufrufe  
    public static void main(String[] args) {  
  
        // Parameter  
        List<Integer> list = Arrays.asList(1,2,3,4,5);  
        Predicate<Integer> p = x -> x % 2 == 0;  
        Function<Integer,Boolean> f = x -> x % 2 == 0;  
  
        // Using Predicate directly. In Predicate, the test method is used to check the condition.  
        //filterList(list, p);  
        // Using Function directly. In Function, the apply method is used to check the condition. there is no test method. Therefore. (predicate.test(i)) does not work.        //filterList(list,f);  
        // Valid: using Predicate        List<Integer> filteredByPredicate = filterList(list, p);  
        System.out.println("Filtered (Predicate): " + filteredByPredicate);  
  
        // Invalid: Function is not a Predicate  
        // This line will NOT compile:        // List<Integer> filteredByFunction = filterList(list, f);  
        // Convert Function to Predicate explicitly if needed:        Predicate<Integer> convertedPredicate = f::apply;  
        List<Integer> filteredByConvertedFunction = filterList(list, convertedPredicate);  
        System.out.println("Filtered (Function as Predicate): " + filteredByConvertedFunction);  
  
    }  
  
}
```
