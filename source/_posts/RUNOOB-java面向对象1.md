---
title: RUNOOB-java面向对象1
date: 2020-03-28 22:19:07
tags: java
---

1. Java 继承

继承的概念
 - 继承是java面向对象编程技术的一块基石，因为它允许创建分等级层次的类。
 - 继承就是子类继承父类的特征和行为，使得子类对象（实例）具有父类的实例域和方法，或子类从父类继承方法，使得子类具有父类相同的行为。

继承类型
 - 需要注意的是 Java 不支持多继承，但支持多重继承。

 ![image](https://www.runoob.com/wp-content/uploads/2013/12/types_of_inheritance-1.png)


继承的特性
 - 子类拥有父类非 private 的属性、方法。
 - 子类可以拥有自己的属性和方法，即子类可以对父类进行扩展。
 - 子类可以用自己的方式实现父类的方法。
 - Java 的继承是单继承，但是可以多重继承，单继承就是一个子类只能继承一个父类，多重继承就是，例如 A 类继承 B 类，B 类继承 C 类，所以按照关系就是 C 类是 B 类的父类，B 类是 A 类的父类，这是 Java 继承区别于 C++ 继承的一个特性。
 - 提高了类之间的耦合性（继承的缺点，耦合度高就会造成代码之间的联系越紧密，代码独立性越差）。

继承关键字
 - 继承可以使用 extends 和 implements 这两个关键字来实现继承，而且所有的类都是继承于 java.lang.Object，当一个类没有继承的两个关键字，则默认继承object（这个类在 java.lang 包中，所以不需要 import）祖先类。

extends关键字
 - 在 Java 中，类的继承是单一继承，也就是说，一个子类只能拥有一个父类，所以 extends 只能继承一个类。
 - extends 关键字
	```
	public class Animal { 
	    private String name;   
	    private int id; 
	    public Animal(String myName, String myid) { 
	        //初始化属性值
	    } 
	    public void eat() {  //吃东西方法的具体实现  } 
	    public void sleep() { //睡觉方法的具体实现  } 
	} 
 	public class Penguin  extends  Animal{ 
	}
```

implements关键字
 - 使用 implements 关键字可以变相的使java具有多继承的特性，使用范围为类继承接口的情况，可以同时继承多个接口（接口跟接口之间采用逗号分隔）。

implements 关键字
```
public interface A {
    public void eat();
    public void sleep();
}
 
public interface B {
    public void show();
}
 
public class C implements A,B {
}

```

super 与 this 关键字
- super关键字：我们可以通过super关键字来实现对父类成员的访问，用来引用当前对象的父类。
- this关键字：指向自己的引用。

实例
```
class Animal {
  void eat() {
    System.out.println("animal : eat");
  }
}
 
class Dog extends Animal {
  void eat() {
    System.out.println("dog : eat");
  }
  void eatTest() {
    this.eat();   // this 调用自己的方法
    super.eat();  // super 调用父类方法
  }
}
 
public class Test {
  public static void main(String[] args) {
    Animal a = new Animal();
    a.eat();
    Dog d = new Dog();
    d.eatTest();
  }
}
```

final关键字
 - final 关键字声明类可以把类定义为不能继承的，即最终类；或者用于修饰方法，该方法不能被子类重写：
 - 声明类：final class 类名 {//类体}
 - 声明方法：修饰符(public/private/default/protected) final 返回值类型 方法名(){//方法体}
注:实例变量也可以被定义为 final，被定义为 final 的变量不能被修改。被声明为 final 类的方法自动地声明为 final，但是实例变量并不是 final


构造器
 - 子类是不继承父类的构造器（构造方法或者构造函数）的，它只是调用（隐式或显式）。如果父类的构造器带有参数，则必须在子类的构造器中显式地通过 super 关键字调用父类的构造器并配以适当的参数列表。
 - 如果父类构造器没有参数，则在子类的构造器中不需要使用 super 关键字调用父类构造器，系统会自动调用父类的无参构造器。

2. Java 重写(Override)与重载(Overload)

重写(Override)
 - 重写是子类对父类的允许访问的方法的实现过程进行重新编写, 返回值和形参都不能改变。即外壳不变，核心重写！
 - 重写的好处在于子类可以根据需要，定义特定于自己的行为。 也就是说子类能够根据需要实现父类的方法。
 - 重写方法不能抛出新的检查异常或者比被重写方法申明更加宽泛的异常。例如： 父类的一个方法申明了一个检查异常 IOException，但是在重写这个方法的时候不能抛出 Exception 异常，因为 Exception 是 IOException 的父类，只能抛出 IOException 的子类异常。
 - 在面向对象原则里，重写意味着可以重写任何现有方法。实例如下：

TestDog.java 文件代码：
```
class Animal{
   public void move(){
      System.out.println("动物可以移动");
   }
}
 
class Dog extends Animal{
   public void move(){
      System.out.println("狗可以跑和走");
   }
}
 
public class TestDog{
   public static void main(String args[]){
      Animal a = new Animal(); // Animal 对象
      Animal b = new Dog(); // Dog 对象
 
      a.move();// 执行 Animal 类的方法
 
      b.move();//执行 Dog 类的方法
   }
}
```

方法的重写规则
 - 参数列表必须完全与被重写方法的相同。
 - 返回类型与被重写方法的返回类型可以不相同，但是必须是父类返回值的派生类（java5 及更早版本返回类型要一样，java7 及更高版本可以不同）。
 - 访问权限不能比父类中被重写的方法的访问权限更低。例如：如果父类的一个方法被声明为 public，那么在子类中重写该方法就不能声明为 protected。
 - 父类的成员方法只能被它的子类重写。
 - 声明为 final 的方法不能被重写。
 - 声明为 static 的方法不能被重写，但是能够被再次声明。
 - 子类和父类在同一个包中，那么子类可以重写父类所有方法，除了声明为 private 和 final 的方法。
 - 子类和父类不在同一个包中，那么子类只能够重写父类的声明为 public 和 protected 的非 final 方法。
 - 重写的方法能够抛出任何非强制异常，无论被重写的方法是否抛出异常。但是，重写的方法不能抛出新的强制性异常，或者比被重写方法声明的更广泛的强制性异常，反之则可以。
 - 构造方法不能被重写。
 - 如果不能继承一个方法，则不能重写这个方法。

Super 关键字的使用
 - 当需要在子类中调用父类的被重写方法时，要使用 super 关键字。

TestDog.java 文件代码：
```
class Animal{
   public void move(){
      System.out.println("动物可以移动");
   }
}
 
class Dog extends Animal{
   public void move(){
      super.move(); // 应用super类的方法
      System.out.println("狗可以跑和走");
   }
}
 
public class TestDog{
   public static void main(String args[]){
 
      Animal b = new Dog(); // Dog 对象
      b.move(); //执行 Dog类的方法
 
   }
}
```

重载(Overload)

	重载(overloading) 是在一个类里面，方法名字相同，而参数不同。返回类型可以相同也可以不同。

	每个重载的方法（或者构造函数）都必须有一个独一无二的参数类型列表。

	最常用的地方就是构造器的重载。

重载规则:
 - 被重载的方法必须改变参数列表(参数个数或类型不一样)；
 - 被重载的方法可以改变返回类型；
 - 被重载的方法可以改变访问修饰符；
 - 被重载的方法可以声明新的或更广的检查异常；
 - 方法能够在同一个类中或者在一个子类中被重载。
 - 无法以返回值类型作为重载函数的区分标准。

实例
Overloading.java 文件代码：
```
public class Overloading {
    public int test(){
        System.out.println("test1");
        return 1;
    }
 
    public void test(int a){
        System.out.println("test2");
    }   
 
    //以下两个参数类型顺序不同
    public String test(int a,String s){
        System.out.println("test3");
        return "returntest3";
    }   
 
    public String test(String s,int a){
        System.out.println("test4");
        return "returntest4";
    }   
 
    public static void main(String[] args){
        Overloading o = new Overloading();
        System.out.println(o.test());
        o.test(1);
        System.out.println(o.test(1,"test3"));
        System.out.println(o.test("test4",1));
    }
}
```

重写与重载之间的区别

 | 区别点	重 | 载方法 | 	重写方法 | 
 | ---- | ---- | 
 | 参数列表	 | 必须修改 | 	一定不能修改 | 
 | 返回类型	 | 可以修改	 | 一定不能修改 | 
 | 异常	 | 可以修改	 | 可以减少或删除，一定不能抛出新的或者更广的异常 | 
 | 访问	 | 可以修改	 | 一定不能做更严格的限制（可以降低限制） | 

总结

方法的重写(Overriding)和重载(Overloading)是java多态性的不同表现，重写是父类与子类之间多态性的一种表现，重载可以理解成多态的具体表现形式。
 - 方法重载是一个类中定义了多个方法名相同,而他们的参数的数量不同或数量相同而类型和次序不同,则称为方法的重载(Overloading)。
 - 方法重写是在子类存在方法与父类的方法的名字相同,而且参数的个数与类型一样,返回值也一样的方法,就称为重写(Overriding)。
 - 方法重载是一个类的多态性表现,而方法重写是子类与父类的一种多态性表现。

![image](https://www.runoob.com/wp-content/uploads/2013/12/overloading-vs-overriding.png)
![image](https://www.runoob.com/wp-content/uploads/2013/12/20171102-1.png)


