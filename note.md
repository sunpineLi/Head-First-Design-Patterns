# 策略模式
> 定义算法簇,分别封装起来，让他们之间可以相互替换。  
此模式让算法的实现独立于使用算法的客户。

## 设计原则
> -  封装变化  
> -  多用组合，少用继承
> -  针对接口编程，不针对实现编程

## 示例
鸭子：鸭子的飞行和叫的行为进行封装，不同种类的鸭子采用不同的实现。

```java
// 鸭子的抽象类
public abstract class Duck {

    FlyBehavior flyBehavior;
    QuackBehavior quackBehavior;

    public Duck() {
    }

    public void setFlyBehavior(FlyBehavior fb) {
        flyBehavior = fb;
    }

    public void setQuackBehavior(QuackBehavior qb) {
        quackBehavior = qb;
    }

    abstract void display();

    public void performFly() {
        flyBehavior.fly();
    }

    public void performQuack() {
        quackBehavior.quack();
    }

    public void swim() {
        System.out.println("All ducks float, even decoys!");
    }
}
```

```java
public interface FlyBehavior {
	public void fly();
}
```
```java
public interface QuackBehavior {
	public void quack();
}
```
```java
public class ModelDuck extends Duck {
    // 在构造时指定具体的实现
	public ModelDuck() {
		flyBehavior = new FlyNoWay();
		quackBehavior = new Quack();
	}

	public void display() {
		System.out.println("I'm a model duck");
	}
}
```

# 观察者模式
一个对象状态改变时，其他对象都会受到通知。

## 示例

```java
public interface Subject {
	public void registerObserver(Observer o);
	public void removeObserver(Observer o);
	public void notifyObservers();
}
```

```java
public interface Observer {
	public void update(int value); 
}
```

可直接用jdk中的观察这模式
```
java.util.Observable
java.util.Observer

缺点：Observable是一个类，必须设计一个类继承它。关键方法被保护，无法组合到自己的对象。
不能满足需求时自己设计。
```

#  装饰者模式
动态的将责任附加到对象上，若要扩展功能，装饰者提供了比继承更有弹性的替代方案

![装饰者模式](https://upload-images.jianshu.io/upload_images/3985563-a0d0ac0c5bdf5c93.png)

## 示例：

![java IO](https://img-blog.csdn.net/20150714211311360?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

> InputStream是被包装的组件，FilterInputStream是一个抽象的装饰者.  
> BufferedInputStream是一个具体的装饰者，它加入两种行为，利用缓冲输入改进性能, 用readline()方法（一次读一行）增强接口。  
> LineNumberInputStream加上了计算行数的功能。

# 简单工厂模式
又叫做静态工厂方法（Static Factory Method）模式，由一个工厂对象根据外界给定的信息,决定究竟应该创建哪个具体类的对象.

## 示例：
```java
// 产品类
abstract class Product{
    public abstract void Show();
}
```

```java
//具体产品类A
class  ProductA extends  Product{

    @Override
    public void Show() {
        System.out.println("生产出了产品A");
    }
}

//具体产品类B
class  ProductB extends  Product{

    @Override
    public void Show() {
        System.out.println("生产出了产品C");
    }
}
```
```java
//工厂类
class  Factory {
    //工厂类控制生产哪种商品, 使用者只需要调用工厂类的静态方法就可以实现产品类的实例化。
    public static Product Manufacture(String ProductName){
        switch (ProductName){
            case "A":
                return new ProductA();

            case "B":
                return new ProductB();

            default:
                return null;
        }
    }
}
```

## 要点
> - 将创建实例的工作与使用实例的工作分开，使用者不必关心类对象如何创建，实现了解耦；  
> - 工厂类违背了“开闭原则”，对系统的维护和扩展不利，在工厂方法模式中得到了一定的克服。

# 工厂方法模式
> 定义了一个创建对象的接口，子类决定实例化哪一个，把实例化推迟到了子类。
[参考文档](https://www.jianshu.com/p/d0c444275827)

## 示例

```java
// 抽象工厂类
abstract class Factory{
    public abstract Product Manufacture();
}

// 抽象产品类
abstract class Product{
    public abstract void Show();
}

//具体产品A类
class  ProductA extends  Product{
    @Override
    public void Show() {
        System.out.println("生产出了产品A");
    }
}

//具体产品B类
class  ProductB extends  Product{

    @Override
    public void Show() {
        System.out.println("生产出了产品B");
    }
}

//工厂A类 - 生产A类产品
class  FactoryA extends Factory{
    @Override
    public Product Manufacture() {
        return new ProductA();
    }
}

//工厂B类 - 生产B类产品
class  FactoryB extends Factory{
    @Override
    public Product Manufacture() {
        return new ProductB();
    }
}

//生产工作流程
public class FactoryPattern {
    public static void main(String[] args){
        //客户要产品A
        FactoryA mFactoryA = new FactoryA();
        mFactoryA.Manufacture().Show();

        //客户要产品B
        FactoryB mFactoryB = new FactoryB();
        mFactoryB.Manufacture().Show();
    }
}
```
## 要点
> - 更符合开-闭原则, 新增一种产品时，只需要增加相应的具体产品类和相应的工厂子类即可。
> - 每个具体工厂类只负责创建对应的产品
> - 依赖倒置原则：依赖抽象，不要依赖具体类。

# 抽象工厂模式
> 提供一个接口，用来创建有相互关系的一组对象。

## 示例：
![抽象工厂模式](https://www.runoob.com/wp-content/uploads/2018/07/1530601916-7298-DP-AbstractFactory.png)

##要点：
> - 当一个产品族中的多个对象被设计成一起工作时，它能保证客户端始终只使用同一个产品族中的对象。
> - 主要解决接口选择的问题。
> - 对于新的产品族符合开-闭原则；对于新的产品种类不符合开-闭原则.

# 单例模式
3种写法：
内部静态类
双检查锁 
枚举（最好的方式）
[参考](https://www.jianshu.com/p/7053217b73cc)


# 适配器模式
> 将一个类的接口，转换成客户期望的另一个接口。

# 外观模式 
> 提供一个统一的接口，用来访问子系统的一群接口。外观定义了一个高层接口，让子系统更易使用。

## 设计原则
> 最少知识原则：只和你的密友谈话。

## 模式比较

模式|意图
-|-
装饰者|不改变接口，但加入责任
适配器|将一个接口转换成另一个接口
外观| 让接口更简单

# 模板方法
 >在一个方法中定义了一个算法的骨架，而将具体的实现步骤延迟到子类中。  
 模板方法使得子类在不改变算法结构的情况下重新定义算法中的某些步骤。

 ## 设计原则
> 好莱坞原则: don't call us, we'll call you   
  决策权放在高层模块，以便决定何时调用底层模块。

## 要点
- 模板方法的抽象类可以定义具体方法、抽象方法和钩子。
- 钩子是一种方法，它在抽象类中不做事或只做默认的事，子类可以选择要不要覆盖它
- 策略模式和模板方法都封装算法，一个用组合,一个用继承。
- 工厂方法是模板方法的一种特殊版本。

# 迭代器模式
> 提供一个方法顺序访问一个集合对象的各个元素，而不用暴露其内部的表示。

## 要点
- 把遍历的任务放在了迭代器上，而不是集合上。简化了集合的接口和实现。  
- 集合对象要负责实例化一个具体迭代器。

## 设计原则
> 单一职责  
集合类的职责是管理某种聚合，如何进行遍历是另外一种职责。
