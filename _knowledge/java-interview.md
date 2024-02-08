---
title: "Q&A4Android Development-Java"
permalink: /knowledge/Android-Java
date: 2024-02-09
excerpt: 'Some common questions and answers for Android development job interviews.'
---

# java相关知识总结
***
## 基础知识
#### java类加载过程
* 加载
类加载器包括 BootClassLoader、ExtClassLoader、APPClassLoader
* 链接
将常量池里面的符号引用（变量名）替换成直接引用（内存地址）过程，在解析阶段，jvm会把所有的类名、方法名、字段名、这些符号引用替换成具体的内存地址或者偏移量
* 初始化
初始化是类加载的最后阶段，主要针对类变量进行初始化，执行类构造器的过程。静态变量或语句在这个阶段被初始化
#### ==和`equals`区别
`equals` 方法使用 == 比较对象的引用，但你可以根据需要自定义 `equals` 方法以执行内容比较。在`string`中`equals`被重写为比较内容
```
public static void main(String[] args) {
        //基本数据类型的比较
        int num1 = 10;
        int num2 = 10;
        System.out.println(num1 == num2);   //true

        //引用数据类型的比较
        //String类（重写了equals方法）中==与equals的比较
        
        String s1 = "hello";
        String s2 = "hello";
        System.out.println(s1 == s2);    //true，比较地址值：内容相同，因为常量池中只有一个“hello”，所以它们的地址值相同
        System.out.println(s1.equals(s2));//true，比较内容：内容相同，因为常量池中只有一个“hello”，所以它们的地址值相同
        System.out.println(s1.equals("hello")); //true

       
        String s3 = new String("hello");
        String s4 = new String("hello");
        System.out.println(s3 == s4);        //false,比较地址值：s3和s4在堆内存中的地址值不同
        System.out.println(s3.equals(s4));    //true，比较内容：内容相同

        //没有重写equals方法的类中==与equals的比较 
        People p1 = new People();
        People p2 = new People();
        People p = p2;
        System.out.println(p1);//People@135fbaa4
        System.out.println(p2);//People@45ee12a7
        System.out.println(p); //People@45ee12a7
        System.out.println(p1.equals(p2));       //false，p1和p2的地址值不同
        System.out.println(p.equals(p2));        //true，p和p2的地址值相同
    }

```
***
#### int和Interger区别

- `int` 是Java的基本数据类型，用于存储整数值。
- `Integer` 是 `int` 的包装类，它是一个引用类型，用于将 `int` 包装成对象。

- `int` 是一个基本数据类型，它不具有可空性，不能表示 `null`。一个 `int` 变量总是具有一个有效的整数值。
- `Integer` 是一个包装类，它具有可空性，可以表示 `null`。这对于需要处理可能不存在值的情况很有用。

Java支持自动装箱和拆箱。自动装箱是指将基本数据类型转换为相应的包装类，而自动拆箱是将包装类转换为基本数据类型。这允许你在需要 `Integer` 的地方使用 `int`，反之亦然，而编译器会自动执行转换。

例如：

```java
Integer i = 42; // 自动装箱
int x = i; // 自动拆箱
```
***
#### java多态
多态性是面向对象编程的一个关键概念，它允许不同类的对象对相同的消息（方法调用）做出不同的响应。多态性是通过继承和接口实现来实现的，并提供了一种更灵活和可扩展的代码组织方式。
* 定义
多态性是指同一方法调用可以在不同的对象上产生不同的行为。它允许在编译时不知道确切类型的情况下，调用方法并在运行时根据对象的实际类型执行相应的操作。
* 子类可以继承父类的方法，然后根据需要重写这些方法以提供自己的实现。通过父类引用指向子类对象，可以实现多态性。这意味着可以使用父类类型的引用调用子类对象的方法。

以下是一个简单的Java示例，用于演示多态性的概念：

```java
class Animal {
    void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    void makeSound() {
        System.out.println("Dog barks");
    }
}

class Cat extends Animal {
    void makeSound() {
        System.println("Cat meows");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myDog = new Dog();
        Animal myCat = new Cat();

        myDog.makeSound(); // 调用的是Dog类的makeSound
        myCat.makeSound(); // 调用的是Cat类的makeSound
    }
}
```
***
#### String / StringBuffer / StringBuilder区别
在Java中，String、StringBuffer和StringBuilder是用于处理字符串的不同类，它们有以下区别：
* 不可变性（Immutability）:
String: 字符串是**不可变**的，一旦创建，它的值不能被更改。任何对String的操作都会创建一个新的String对象。
StringBuffer: StringBuffer是可变的，它可以通过追加、插入、删除字符等操作来修改字符串内容。
StringBuilder: StringBuilder也是可变的，类似于StringBuffer，但不是线程安全的。
* 线程安全性:
String: 由于字符串是不可变的，它们是线程安全的。
StringBuffer: **StringBuffer被设计为线程安全**的，因此多个线程可以安全地访问和修改同一个StringBuffer对象。
StringBuilder: StringBuilder不是线程安全的，如果多个线程同时访问和修改同一个StringBuilder对象，可能会导致问题。
* 性能:
String: 由于字符串不可变，对String进行频繁的连接和修改会产生大量临时对象，可能会导致性能问题。
StringBuffer: StringBuffer是为了解决String的性能问题而设计的，它的操作不会创建新对象，但由于线程安全的开销，相对较慢。
StringBuilder: StringBuilder不是线程安全的，因此**在单线程环境中性能更好**。
* 用途
String: 适用于不需要频繁修改字符串内容的情况，例如**字符串常量**、文本处理等。
StringBuffer: 适用于多线程环境或需要频繁修改字符串内容的情况，例如拼接大量字符串。
StringBuilder: 适用于单线程环境，需要频繁修改字符串内容的情况，在速度要求较高的情况下使用。
****
#### 抽象类和接口
抽象类和接口是Java中用于实现多态性和抽象性的两种不同机制，它们有以下主要区别：

**抽象类（Abstract Class）：**

- 抽象类可以包含抽象方法和具体方法。抽象方法是没有实际实现的方法，而具体方法有实现。
- 抽象类**可以有构造方法**，可以用于初始化对象的状态。
- 抽象类可以包含成员变量，可以是私有的，受保护的，公共的...
- 一个类只能继承一个抽象类，即**Java中不支持多继承**。
- 抽象类用于描述一种 "是什么" 的关系，即它通常代表一种基本类型或通用行为。

示例：

```java
public abstract class Animal {
    protected String name;

    public Animal(String name) {
        this.name = name;
    }

    public abstract void makeSound();

    public void eat() {
        System.out.println(name + " is eating.");
    }
```
**接口（Interface）：**

- 接口只能包含抽象方法，这些方法没有实际的实现。Java 8之后，接口也可以包含默认方法和静态方法，这些方法有默认实现。
- 接口不能包含成员变量，除了静态常量。接口中的变量默认是public static final，不需要显式声明。
- 一个类可以**实现多个接口**，从而支持多继承。
- 接口用于描述一种 "能做什么" 的关系，即它定义了一组可用的方法。

示例：

```java
public interface Vehicle {
    void start();
    void stop();
}

public class Car implements Vehicle {
    @Override
    public void start() {
        System.out.println("Car starts.");
    }

    @Override
    public void stop() {
        System.out.println("Car stops.");
    }
```
****
#### extends和super在泛型中的作用
在Java中，泛型中的`extends`和`super`用于限定泛型类型参数的范围，它们有不同的用途和限制：

**`extends`（上限通配符）：**

- `extends` 用于限定泛型类型参数的上限边界。它表示泛型类型参数必须是指定类型或其子类型。
- 使用 `extends` 通常用于读取数据，即从泛型对象中获取数据。
- 例如，`<? extends Number>` 表示类型参数可以是 `Number` 或其子类，例如 `Integer` 或 `Double`。

***示例：***
```java
List<? extends Number> numbers = new ArrayList<>();
Number num = numbers.get(0); // 可以获取 Number 或其子类
```
**`super`**相反