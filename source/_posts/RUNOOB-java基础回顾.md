---
title: RUNOOB-java基础回顾
date: 2020-03-19 10:46:15
tags: java
---

1. HelloWorld

```
public class Helloworld{
	public static void main(String[] args){
		System.out.println("Hello World!");
	}
}
```

`注：String args[] 与 String[] args 都可以执行，但推荐使用 String[] args，这样可以避免歧义和误读。`

2. 命令解析
 - javac 后面跟着的是java文件的文件名，该命令用于将java 源文件编译为 class 字节码文件， 如：javac HelloWorld.java
 	运行javac 命令后，如果编译成功，会出现一个HelloWorld.class 的文件

 - java 后面跟着的是 java文件中的类名，例如 HelloWorld 就是类名， 如： java HelloWorld
	注意： java 命令后面不要加 .class

3. 简介

Java分为三个体系：
	- JavaSE(J2SE） (Java2 Platform Standard Edition，java平台标准版）
	- JavaEE(J2EE)  (Java2 Platform,Enterprise Edition，java平台企业版)
	- JavaME(J2ME)  (Java2 Platform Micro Edition，java平台微型版)。 

4. 基础语法
java 程序可以认为是一系列的集合
 - 对象： 对象是类的一个实例，有状态和行为，
 - 类： 类是一个模版，它描述一类对象的行为和状态。
 - 方法： 方法就是行为，一个类可以有很多的方法，逻辑运算、数据修改以及所有动作都是在方法中完成的。
 - 实例变量： 每个对象都有独特的实例变量，对象的状态由这些实例变量的值决定的。

基本语法
- 大小写敏感：java是大小写敏感的。
- 类名： 对于所有类来说，类名的首字母应该大写。如果类名是若干单词组成，那么每个单词的首字母都应该大写。
- 方法名： 所有的方法名都应该小写字母开头。如果若干单词，则后面的单词首字母大写。
- 主方法入口： 所有的java 程序由 public static void main (String[] args) 方法开始执行。

java 修饰符
 - 访问控制修饰符 : default, public , protected, private
 - 非访问控制修饰符 : final, abstract, static, synchronized

java 变量
 - 局部变量
 - 类变量 (静态变量)
 - 成员变量（非静态变量） 

继承
 - 在java 中，一个类可以由其他类派生，如果你要创建一个类，而且已经存在一个类具有你所需要的属性和方法，，那么你可以将新创建的类继承该类。
 - 利用继承的方法，可以重用已存在类的方法和属性，而不用重写这些代码，被继承的类称为超类（super class），派生类称为子类（subclass）

接口
 - 在java中，接口可理解为对象间相互通信的协议，接口在继承中扮演着重要的角色。
 - 接口只定义派生要用到的方法，但是方法的具体实现完全取决于派生类。

5. java 对象和类
java作为一个面向对象语言。
- 多态
- 继承
- 封装
- 抽象
- 类
- 对象
- 实例
- 方法
- 重载

6. java 基本数据类型
变量就是申请内存来存储值，也就是说，当创建变量的时候，就需要在内存中申请空间。
内存管理系统就是根据变量的类型为变量分配存储空间，分配的空间只能用来存储该类型数据。

java 中两大数据类型：
 - 内置数据类型
 - 引用数据类型

 内置数据类型
 java内置提供了八种基本类型。六种数据类型（四个整数型，两个浮点型），一个字符串，一个布尔型。

 byte，（-2^7 ~ 2^7-1 ）short (-2^15 ~ 2^15-1 ）， int，(-2^31 ~ 2^31-1 ）long (-2^63 ~ 2^63-1 ）
 float ， double
 boolean
 char

 为什么byte的取值范围是-128到127?
 一个byte由八个位组成，如00000000，其中，符号位+数值位，前7位表示数值，第8位是符号位（0为正，1为负）。
 这样 +1就是00000001，-1就是10000001。
 最大的正数就是0 1111111，即2^0+2^1+……+2^6=127；
 最小的负数，同理，为1 1111111，即-127。

为什么负数会到-128。这不得不崇拜伟大的印度阿三们。
就是0，会出现一个+0和一个-0。印度人他们规定-0为-128，这样就与计算机的补码（程序都是按补码运行的）完美的结合在一起。

隐含强制类型转换
 - 整数的默认类型是 int。
 - 浮点型不存在这种情况，因为在定义 float 类型时必须在数字后面跟上 F 或者 f。

























