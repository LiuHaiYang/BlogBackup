---
title: RUNOOB-java基础回顾2
date: 2020-03-19 20:44:08
tags: java
---

7. java 变量类型

java 支持的变量类型有：
- 类变量： 独立于方法之外的变量，用static 修饰
- 实例变量： 独立于方法之外的变量，不过没有static修饰
- 局部变量： 类的方式的变量。

java 局部变量
- 局部变量声明在方法，构造方法或者语句块中；
- 局部变量在方法，构造方法，或者语句块被执行的时候创建，当它们执行完成后，变量将会被销毁。
- 访问修饰符不能用于局部变量。
- 局部变量只在声明他的方法，构造方法或者语句块中可见；
- 局部变量是栈上分配的。
- 局部变量没有默认值，所以局部变量被声明后，必须经过初始化，才可以使用。

实例变量
 - 实例变量声明在一个类中，但在方法、构造方法和语句块之外；
 - 当一个对象被实例化之后，每个实例变量的值就跟着确定；
 - 实例变量在对象创建的时候创建，在对象被销毁的时候销毁；
 - 实例变量的值应该至少被一个方法、构造方法或者语句块引用，使得外部能够通过这些方式获取实例变量信息；
 - 实例变量可以声明在使用前或者使用后；
 - 访问修饰符可以修饰实例变量；
 - 实例变量对于类中的方法、构造方法或者语句块是可见的。一般情况下应该把实例变量设为私有。通过使用访问修饰符可以使实例变量对子类可见；
 - 实例变量具有默认值。数值型变量的默认值是0，布尔型变量的默认值是false，引用类型变量的默认值是null。变量的值可以在声明时指定，也可以在构造方法中指定；
 - 实例变量可以直接通过变量名访问。但在静态方法以及其他类中，就应该使用完全限定名：ObejectReference.VariableName。

类变量（静态变量）
 - 类变量也称为静态变量，在类中以 static 关键字声明，但必须在方法之外。
 - 无论一个类创建了多少个对象，类只拥有类变量的一份拷贝。
 - 静态变量除了被声明为常量外很少使用。常量是指声明为public/private，final和static类型的变量。常量初始化后不可改变。
 - 静态变量储存在静态存储区。经常被声明为常量，很少单独使用static声明变量。
 - 静态变量在第一次被访问时创建，在程序结束时销毁。
 - 与实例变量具有相似的可见性。但为了对类的使用者可见，大多数静态变量声明为public类型。
 - 默认值和实例变量相似。数值型变量默认值是0，布尔型默认值是false，引用类型默认值是null。变量的值可以在声明的时候指定，也可以在构造方法中指定。此外，静态变量还可以在静态语句块中初始化。
 - 静态变量可以通过：ClassName.VariableName的方式访问。
 - 类变量被声明为public static final类型时，类变量名称一般建议使用大写字母。如果静态变量不是public和final类型，其命名方式与实例变量以及局部变量的命名方式一致

8. java 运算符

计算机的最基本用途之一就是执行数学运算，作为一门计算机语言，Java也提供了一套丰富的运算符来操纵变量。我们可以把运算符分成以下几组：
 - 算术运算符
 - 关系运算符
 - 位运算符
 - 逻辑运算符
 - 赋值运算符
 - 其他运算符


 自增自减运算符
1、自增（++）自减（--）运算符是一种特殊的算术运算符，在算术运算符中需要两个操作数来进行运算，而自增自减运算符是一个操作数。
2、前缀自增自减法(++a,--a): 先进行自增或者自减运算，再进行表达式运算。
3、后缀自增自减法(a++,a--): 先进行表达式运算，再进行自增或者自减运算。


位运算符

- Java定义了位运算符，应用于整数类型(int)，长整型(long)，短整型(short)，字符型(char)，和字节型(byte)等类型。
- 位运算符作用在所有的位上，并且按位运算。假设a = 60，b = 13;它们的二进制格式表示将如下：

```
A = 0011 1100
B = 0000 1101
-----------------
A&B = 0000 1100
A | B = 0011 1101
A ^ B = 0011 0001
~A= 1100 0011
```

条件运算符（?:）
 - 条件运算符也被称为三元运算符。该运算符有3个操作数，并且需要判断布尔表达式的值。该运算符的主要是决定哪个值应该赋值给变量。
	`variable x = (expression) ? value if true : value if false`

Test.java 文件代码：
```
public class Test {
   public static void main(String[] args){
      int a , b;
      a = 10;
      // 如果 a 等于 1 成立，则设置 b 为 20，否则为 30
      b = (a == 1) ? 20 : 30;
      System.out.println( "Value of b is : " +  b );
 
      // 如果 a 等于 10 成立，则设置 b 为 20，否则为 30
      b = (a == 10) ? 20 : 30;
      System.out.println( "Value of b is : " + b );
   }
}
```
以上实例编译运行结果如下：
```
Value of b is : 30
Value of b is : 20
```

instanceof 运算符
 - 该运算符用于操作对象实例，检查该对象是否是一个特定类型（类类型或接口类型）。
 - instanceof运算符使用格式如下：

	( Object reference variable ) instanceof  (class/interface type)
	如果运算符左侧变量所指的对象，是操作符右侧类或接口(class/interface)的一个对象，那么结果为真。

下面是一个例子：
	```
	String name = "James";
	boolean result = name instanceof String; // 由于 name 是 String 类型，所以返回真
	```


Java运算符优先级
 - 当多个运算符出现在一个表达式中，谁先谁后呢？这就涉及到运算符的优先级别的问题。在一个多运算符的表达式中，运算符优先级不同会导致最后得出的结果差别甚大。

 - 例如，（1+3）＋（3+2）*2，这个表达式如果按加号最优先计算，答案就是 18，如果按照乘号最优先，答案则是 14。

 - 再如，x = 7 + 3 * 2;这里x得到13，而不是20，因为乘法运算符比加法运算符有较高的优先级，所以先计算3 * 2得到6，然后再加7。

9. Java 循环结构

Java 循环结构 - for, while 及 do...while
顺序结构的程序语句只能被执行一次。如果您想要同样的操作执行多次,，就需要使用循环结构。

Java中有三种主要的循环结构：
 - while 循环
 - do…while 循环
 - for 循环

 - break 关键字
break 主要用在循环语句或者 switch 语句中，用来跳出整个语句块。

break 跳出最里层的循环，并且继续执行该循环下面的语句。

 - continue 关键字
continue 适用于任何循环控制结构中。作用是让程序立刻跳转到下一次循环的迭代。

在 for 循环中，continue 语句使程序立即跳转到更新语句。

在 while 或者 do…while 循环中，程序立即跳转到布尔表达式的判断语句。

10. 条件判断
- if 语句
- if... else 语句
- if... if else ... else 语句
- 嵌套的 if…else 语句

11. Java switch case 语句

语法
switch case 语句语法格式如下：
```
switch(expression){
    case value :
       //语句
       break; //可选
    case value :
       //语句
       break; //可选
    //你可以有任意数量的case语句
    default : //可选
       //语句
}
```
switch case 语句有如下规则：

 - switch 语句中的变量类型可以是： byte、short、int 或者 char。从 Java SE 7 开始，switch 支持字符串 String 类型了，同时 case 标签必须为字符串常量或字面量。
 - switch 语句可以拥有多个 case 语句。每个 case 后面跟一个要比较的值和冒号。 
 - case 语句中的值的数据类型必须与变量的数据类型相同，而且只能是常量或者字面常量。
 - 当变量的值与 case 语句的值相等时，那么 case 语句之后的语句开始执行，直到 break 语句出现才会跳出 switch 语句。
 - 当遇到 break 语句时，switch 语句终止。程序跳转到 switch 语句后面的语句执行。case 语句不必须要包含 break 语句。如果没有 break 语句出现，程序会继续执行下一条 case 语句，直到出现 break 语句。
 - switch 语句可以包含一个 default 分支，该分支一般是 switch 语句的最后一个分支（可以在任何位置，但建议在最后一个）。default 在没有 case 语句的值和变量值相等的时候执行。default 分支不需要 break 语句。
 - switch case 执行时，一定会先进行匹配，匹配成功返回当前 case 的值，再根据是否有 break，判断是否继续输出，或是跳出判断。

























