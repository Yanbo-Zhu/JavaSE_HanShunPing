

# 1 Annotation 


Annotations can provide meta-information about elements in the Java source code. This information can be used by the compiler, during deployment, or at runtime.
Annotations themselves don’t add functionality, they don’t have a “functional behavior” –annotations are essentially data containers which can be accessed by other components.
We can annotate classes, methods, constructors, fields, etc. –what can be annotated depends on the respective annotation.


@Override–the annotated element is meant to override element in superclass. If its not found in one of the superclasses, cause a compilation error
@SuppressWarnings–ignore compile-time warnings for this element
@Deprecated–marks a method as deprecated. Causes a compiler warning if the method is used.


## 1.1 Annotation and refelction 



Annnotationscanbea simple marker(e.g., @Override) ortheycanbeusedtoaddspecificvaluestothetarget(in bracketsafter theannotationname):

```
@Author(name="Jane Doe", date="1970-01-01")
public class HelloWorld {
	// ...
}
```


Using reflections (next topic), we can programmatically query annotations. In the example above, such queries could tell us that 
i) HelloWorld has an Author annotation, 
ii) the element name has the value “Jane Doe“, and 
iii) the element date has the value “1970-01-01“


## 1.2 develop our own annotations.


General syntax:
`<access modifier> @interface AnnotationName{…}`

Example:
`public @interface Deprecated {…}`
Annotations can have elements (which look like methods and work like fields)…


As discussed, annotations can “mark” their annotation target but are also a form of “data container” for meta-data.
For such data, we define so-called elements:
• Elements look like no-parameter methods whose return type (i.e., the contained data) is either a primitive type, String, an enum, Class, another annotation, or an array of either of these types. These methods only return the contained data value.
• We can optionally define a default value for these methods in case the developer using the annotation does not provide one, e.g.:


![[Pasted image 20250506160940.png]]


![[Pasted image 20250506161113.png]]




## 1.3 The value() element in single-element annotations


If an annotation only contains a single element, its name should be value(). In this case, the element name can be omitted.
Example:
`@SuppressWarnings(value = "unchecked")`

is the same as
`@SuppressWarnings("unchecked")`

But this only works when:
- There is **only one element**.
- That element is **named exactly `value`**.


---


If the element is named something else:
```
@interface MyAnnotation {
    String name();
}
```


Then you must write:
```
@MyAnnotation(name = "example")
```

You **cannot** write `@MyAnnotation("example")` in this case.





## 1.4 @Target

![[Pasted image 20250506161914.png]]

- How can we specify the annotation target (i.e., whether it works for classes or methods or …)?
==@Target is an annotation for annotating definitions of annotation types. It restricts the kind of element the annotation can be used on, i.e., where in the source code an annotation can be written.==

It expects a single parameter of the ElementType enum describing said target:
- ANNOTATION_TYPE : Other annotations
- CONSTRUCTOR : Constructor declarations
- FIELD : Variable declarations in a class or object
- LOCAL_VARIABLE : Local variable declarations in a function
- METHOD : Method declarations
- PACKAGE: Package declarations
- PARAMETER : Parameters of a function
- TYPE : Type declarations (class, interface, enum, or annotation type declarations)
- TYPE_PARAMETER : Generic type parameter declarations (in generic classes, interfaces, methods,
constructors)

## 1.5 @Retention

![[Pasted image 20250506161914.png]]

![](e4408faf4496468a34cf9fbe6b2b8b7.jpg)

- Can we control during which phase of the development cycle annotations can be queried?

==@Retention is an annotation used on the definition of annotations. It indicates how long annotations should be retained by the JVM. It takes a RetentionPolicy as parameter:==
- SOURCE: the annotation should only be saved in the source level.
- CLASS: the annotation should be retained at compile time but is ignored by the JVM.
- RUNTIME: retained by the JVM so it can be used by the runtime environment.

Example:
```
@Retention(RetentionPolicy.RUNTIME)
public @interface FooBar {…}
```

## 1.6 @Repeatable

- Can we specify how many times an annotation can be used on a specific target?

When we want to use an annotation A multiple times on the same target, we have to mark the definition of that annotation A as @Repeatable. 

When marking A as @Repeatable, we need another annotation B whose value() method returns an `A[]`. As a parameter of @Repeatable, we then pass a reference to B.class.

```
@Repeatable(B.class)
public @interface A{String value();}
public @interface B{A[] value();}
```

During compilation, the repeated annotation A is turned into a B (containing the specified `A []`)


To do this, **two annotations** are involved:
1. A **repeated annotation** (e.g., `@A`)
2. A **container annotation** (e.g., `@B`) that holds an array of `@A`



![[Pasted image 20250506162522.png]]


### 1.6.1 Example

```java
import java.lang.annotation.*;

@Repeatable(B.class)       // Reference to the container
public @interface A {
    String value();
}

@Retention(RetentionPolicy.RUNTIME)
public @interface B {
    A[] value();           // Container must have a method returning an array of the repeated annotation
}

```


```java
@A("one")
@A("two")
public class MyClass {}
```

---

At Compile Time:
The compiler rewrites the multiple `@A` annotations into a **single** `@B` annotation:

```
@B({
    @A("one"),
    @A("two")
})
```


----

Accessing via Reflection:

```
A[] annotations = MyClass.class.getAnnotationsByType(A.class);
```

This will return both `@A("one")` and `@A("two")`.


### 1.6.2 example 2 

![[Pasted image 20250506162917.png]]

# 2 Reflections


==A program can know and change its own behaviour during its own runtime.==
With object-oriented programming languages, this means that information about classes can be queried during runtime (e.g., its name), and that their behaviour can be modified (e.g., setting a field to private).

We can also use reflections for advanced tasks such as generating a new class at runtime.

Keep in mind: Reflective programming is usually slower than the normal program flow and implies that some bugs will only become visible at runtime (i.e., cannot be checked by the compiler).
=> Use only when necessary.




Java types also have an OOP representation in Java

To represent Java types within Java, there are Java classes/interfaces that represent information about classes and objects, e.g.:
- `Class<T>`: represents classes and interfaces in a running Java application
- `Constructor<T>`: provides information about and access to a single constructor of a class
- Field: represents a field of a class or an interface.
- Annotation: represents an annotation

…
Every Java class has an associated Classobject, every Field has an associated Fieldobject etc.
=> We can use them to ask questions about classes etc. at runtime (e.g., query whether a class A contains a method b() which has been annotated with an annotation @C).



## 2.1 `Class<T>`

Instances of `java.lang.Class<T>`are used as an “entry point” for reflection. They can be accessed with .getClass() on all reference types, or ClassName.class if there is no instance. An alternative is Class.forName(classname). The generic type parameter is the actual class used, i.e., String.class returns an instance of `Class<String>`.

A Classinstance has instance methods to provide access to other Classobjects:
- getSuperclass(): super class
- getClasses(): all public classes and interfaces that are members of the class
- getDeclaredClasses(): all classes and interfaces declared as members of the class

and other properties of the class, e.g.:
- getDeclaredConstructors(): returns one Constructorobject for every constructor
- getDeclaredFields(): returns one Fieldobject for every declared field
- getDeclaredMethods(): returns one Methodobject for every declared field

We can also call newInstance() to get a new instance of that class.


`java.lang.Class<T>` 类的实例被用作反射的“入口点”。你可以通过以下几种方式获取它：

- 对任意引用类型的对象调用 `.getClass()` 方法；
    
- 对于没有实例的类，可以使用 `ClassName.class`；
    
- 另外，也可以通过 `Class.forName(classname)` 方式来获取。
    

`Class` 的泛型类型参数表示实际使用的类，例如：`String.class` 返回一个 `Class<String>` 实例。

一个 `Class` 实例有一些方法，用来访问与该类相关的其他 `Class` 对象：

- `getSuperclass()`：获取父类
    
- `getClasses()`：获取该类中所有的 **公共** 成员类和接口
    
- `getDeclaredClasses()`：获取该类中声明的所有（包括私有）成员类和接口
    

以及该类的其他结构信息，比如：

- `getDeclaredConstructors()`：返回一个 `Constructor` 对象数组，包含所有构造方法
    
- `getDeclaredFields()`：返回一个 `Field` 对象数组，包含所有声明的字段
    
- `getDeclaredMethods()`：返回一个 `Method` 对象数组，包含所有声明的方法
    

你也可以通过调用 `newInstance()` 来创建该类的一个新实例。


---

例子 

当然可以，下面是一个使用 Java 反射 API 的简单示例，展示如何：
1. 获取一个类的 `Class` 对象，
2. 查看其字段和方法，
3. 创建实例并调用方法。


示例类：`Person`
```
public class Person {
    private String name;

    public Person() {
        this.name = "Unknown";
    }

    public Person(String name) {
        this.name = name;
    }

    public void sayHello() {
        System.out.println("Hello, my name is " + name);
    }

    private void secret() {
        System.out.println("This is a secret method.");
    }
}

```



使用反射操作该类
```
import java.lang.reflect.*;

public class ReflectionDemo {
    public static void main(String[] args) throws Exception {
        // 1. 获取 Class 对象
        Class<?> clazz = Class.forName("Person");

        // 2. 创建实例（调用无参构造函数）
        Object person = clazz.getDeclaredConstructor().newInstance();

        // 3. 获取并调用公共方法
        Method sayHello = clazz.getMethod("sayHello");
        sayHello.invoke(person);  // 输出: Hello, my name is Unknown

        // 4. 获取并访问私有方法
        Method secret = clazz.getDeclaredMethod("secret");
        secret.setAccessible(true); // 打破私有访问限制
        secret.invoke(person);      // 输出: This is a secret method.

        // 5. 获取字段
        Field nameField = clazz.getDeclaredField("name");
        nameField.setAccessible(true);
        nameField.set(person, "Alice"); // 修改私有字段

        // 再次调用 sayHello()
        sayHello.invoke(person);  // 输出: Hello, my name is Alice
    }
}

```




这个例子展示了反射可以做到：
- **动态获取类信息**
- **创建对象**
- **调用方法（包括私有方法）**
- **访问和修改私有字段**
    
要运行这个例子，你需要将 `Person` 类和 `ReflectionDemo` 放在同一个包或文件中，或调整包名。

需要我解释哪个部分的原理或者写一个使用构造方法带参数的例子吗？


## 2.2 Accessing private fields , methods, and constructors

Accessing private fields , methods, and constructors
All reflected objects (Field, Method, Constructor) extend the class AccessibleObject.

With an AccesibleObject, the checks for normal access control (e.g., private, public) can be suppressed for reflected objects by calling setAccesible(true).

Afterwards, (originally private) Methods can be called by calling Method.invoke(obj, args…), and Fields can be set with Field.set(obj, value)


![[Pasted image 20250506171746.png]]



# 3 Working with annotations and reflections


The annotations of a class can be accessed via the interface Annotation, which every annotation automatically extends. The method `AccessibleObject.getAnnotations()` returns all Annotations of an entity, and `getAnnotation ( Class<T> annotationClass)` returns a specific annotation. The values from the annotation can be extracted by calling the corresponding elements like methods:


![[Pasted image 20250506171842.png]]



![[Pasted image 20250506172128.png]]















