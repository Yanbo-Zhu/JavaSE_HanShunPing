
# 1 Depenedency Injection 


==Dependency Injection is a design pattern where object creation and using the object are separated from each other.==

An object receives all other objects it depends on as parameter, so the object itself does not need to “know”
how to construct the object it depends on. Instead, the objects are provided by an Injector.


It sounds complicated but is pretty easy: give an object its instance variables without writing explicit newcalls. Dependency Injection has multiple advantages:
- Decreased coupling between classes and their dependencies
- Changing behavior of dependencies without changing classes that use them
- Changing behavior of components during testing


**Dependency Injection（依赖注入）** 是 Java 中一种 **设计模式**，其核心思想是：**将一个类所依赖的对象（即“依赖”）从外部传入，而不是由这个类自己创建依赖**。这提高了代码的灵活性、可测试性和可维护性。


为什么使用依赖注入？

    降低耦合（Car 不再依赖于具体的 Engine 实现）

    易于测试（可以用 Mock 对象替代真实依赖）

    更灵活地管理对象生命周期（特别是在大型项目中）


----

In larger code bases, dependency injection (and coding to an interface) are especially useful, since two teams can work in parallel on components that depend on each other.
Instead of passing the dependency in the constructor, there are frameworks that automate dependency injection with annotations:
=> Depending on the runtime environment and stage (testing vs. production), the dependency injection framework can automatically inject different objects*.

```
publicclassBurgerShop {
    @Autowired() private BurgerMakerbm;
}
```

## 1.1 例子 

假设你有一个类 `Car`，它依赖于 `Engine`：
```
public class Engine {
    public void start() {
        System.out.println("Engine started");
    }

```

不使用依赖注入（紧耦合）：

```
public class Car {
    private Engine engine;

    public Car() {
        this.engine = new Engine();  // Car 自己创建 Engine
    }

    public void start() {
        engine.start();
    }
}
```

在这个例子中，`Car` tightly depends on `Engine`，我们很难在测试中替换 `Engine`。

使用构造函数注入（推荐方式）：
```
public class Car {
    private Engine engine;

    public Car(Engine engine) {
        this.engine = engine;  // Engine 从外部传入
    }

    public void start() {
        engine.start();
    }
}

```

```
Engine myEngine = new Engine();
Car myCar = new Car(myEngine);  // 注入 Engine
```

## 1.2 例子 2

```
public class BurgerShop {
    private BurgerMakerbm;
    BurgerShop(BurgerMakerbm) {
        this.bm = bm;
    }
    publicBurger takeOrder() {
        System.out.println("Welcome totheBurgerShop");
        System.out.println("Try out ourtastysugardrink!");
        Burger b = bm.makeBurger();
        System.out.println("Try thistastyburger! " + b.toString());
        returnb;
    }
}
```

```
public interface BurgerMaker {
    public Burger makeBurger();
}
class ProfessionalBurger Makerimplements BurgerMaker {
    @OverridepublicBurger makeBurger() {
        returnnewBurger("High Quality Burger!");
    }
}
class AmateurBurgerMaker implements BurgerMaker {
    @OverridepublicBurger makeBurger() {
        returnnewBurger("I am an Amateur Burger!");
    }
}
```


This way, it’s easy to use the BurgerShopin another context with another BurgerMaker, e.g., during testing.

```
publicstaticvoidmain(String[] args) {
    BurgerMakerbm = newProfessionalBurgerMaker();
    BurgerShopbs = newBurgerShop(bm);
    Burger b = bs.takeOrder();
    System.out.println(b);
}
```


print 

```
Welcome to the BurgerShop
Try out our tasty sugar drink!
Try this tasty burger! Burger(text=High Quality Burger!)
Burger(text=High Quality Burger!)
```