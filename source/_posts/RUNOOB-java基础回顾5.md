---
title: RUNOOB-java基础回顾5
date: 2020-03-25 10:11:28
tags: java
---

17. Java 日期时间

- java.util 包提供了 Date 类来封装当前的日期和时间。 Date 类提供两个构造函数来实例化 Date 对象。

	第一个构造函数使用当前日期和时间来初始化对象。`Date( )`
	第二个构造函数接收一个参数，该参数是从1970年1月1日起的毫秒数。`Date(long millisec)`

- Date对象创建以后，可以调用下面的方法。

| 序号 | 	方法  | 描述 | 
 | --- | --- | --- | 
 | 1 | 	boolean after(Date date) | 若当调用此方法的Date对象在指定日期之后返回true,否则返回false。 | 
 | 2 | 	boolean before(Date date) | 若当调用此方法的Date对象在指定日期之前返回true,否则返回false。 | 
 | 3 | 	Object clone( ) | 返回此对象的副本。 | 
 | 4 | 	int compareTo(Date date) | 比较当调用此方法的Date对象和指定日期。两者相等时候返回0。调用对象在指定日期之前则返回负数。调用对象在指定日期之后则返回正数。 | 
 | 5 | 	int compareTo(Object obj) | 若obj是Date类型则操作等同于compareTo(Date) 。否则它抛出ClassCastException。 | 
 | 6 | 	boolean equals(Object date) | 当调用此方法的Date对象和指定日期相等时候返回true,否则返回false。 | 
 | 7 | 	long getTime( ) | 返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 Date 对象表示的毫秒数。 | 
 | 8 | 	int hashCode( ) | 返回此对象的哈希码值。 | 
 | 9 | 	void setTime(long time) | 用自1970年1月1日00:00:00 GMT以后time毫秒数设置时间和日期。 | 
 | 10 | 	String toString( ) | 把此 Date 对象转换为以下形式的 String： dow mon dd hh:mm:ss zzz yyyy 其中： dow 是一周中的某一天 (Sun, Mon, Tue, Wed, Thu, Fri, Sat)。 | 

- 获取当前日期时间

Java中获取当前日期和时间很简单，使用 Date 对象的 toString() 方法来打印当前日期和时间，如下所示：

实例
```
import java.util.Date;
  
public class DateDemo {
   public static void main(String args[]) {
       // 初始化 Date 对象
       Date date = new Date();
        
       // 使用 toString() 函数显示日期时间
       System.out.println(date.toString());
   }
}
```

- 日期比较
Java使用以下三种方法来比较两个日期：
	- 使用 getTime() 方法获取两个日期（自1970年1月1日经历的毫秒数值），然后比较这两个值。
	- 使用方法 before()，after() 和 equals()。例如，一个月的12号比18号早，则 new Date(99, 2, 12).before(new Date (99, 2, 18)) 返回true。
	- 使用 compareTo() 方法，它是由 Comparable 接口定义的，Date 类实现了这个接口。

- 使用 SimpleDateFormat 格式化日期
SimpleDateFormat 是一个以语言环境敏感的方式来格式化和分析日期的类。SimpleDateFormat 允许你选择任何用户自定义日期时间格式来运行。例如：

实例
```
import  java.util.*;
import java.text.*;
 
public class DateDemo {
   public static void main(String args[]) {
 
      Date dNow = new Date( );
      SimpleDateFormat ft = new SimpleDateFormat ("yyyy-MM-dd hh:mm:ss");
 
      System.out.println("当前时间为: " + ft.format(dNow));
   }
}
```
这一行代码确立了转换的格式，其中 yyyy 是完整的公元年，MM 是月份，dd 是日期，HH:mm:ss 是时、分、秒。

注意:有的格式大写，有的格式小写，例如 MM 是月份，mm 是分；HH 是 24 小时制，而 hh 是 12 小时制。

- 解析字符串为时间
SimpleDateFormat 类有一些附加的方法，特别是parse()，它试图按照给定的SimpleDateFormat 对象的格式化存储来解析字符串。例如：

实例
```
import java.util.*;
import java.text.*;
  
public class DateDemo {
 
   public static void main(String args[]) {
      SimpleDateFormat ft = new SimpleDateFormat ("yyyy-MM-dd"); 
 
      String input = args.length == 0 ? "1818-11-11" : args[0]; 
 
      System.out.print(input + " Parses as "); 
 
      Date t; 
 
      try { 
          t = ft.parse(input); 
          System.out.println(t); 
      } catch (ParseException e) { 
          System.out.println("Unparseable using " + ft); 
      }
   }
}
```
- Java 休眠(sleep)
sleep()使当前线程进入停滞状态（阻塞当前线程），让出CPU的使用、目的是不让当前线程独自霸占该进程所获的CPU资源，以留一定时间给其他线程执行的机会。

- 测量时间
下面的一个例子表明如何测量时间间隔（以毫秒为单位）` Thread.sleep(5*60*10);`

- Calendar类
我们现在已经能够格式化并创建一个日期对象了，但是我们如何才能设置和获取日期数据的特定部分呢，比如说小时，日，或者分钟? 我们又如何在日期的这些部分加上或者减去值呢? 答案是使用Calendar 类。

Calendar类的功能要比Date类强大很多，而且在实现方式上也比Date类要复杂一些。

Calendar类是一个抽象类，在实际使用时实现特定的子类的对象，创建对象的过程对程序员来说是透明的，只需要使用getInstance方法创建即可。

创建一个代表系统当前日期的Calendar对象 `Calendar c = Calendar.getInstance();//默认是当前日期`
创建一个指定日期的Calendar对象
使用Calendar类代表特定的时间，需要首先创建一个Calendar的对象，然后再设定该对象中的年月日参数来完成。
```
//创建一个代表2009年6月12日的Calendar对象
Calendar c1 = Calendar.getInstance();
c1.set(2009, 6, 12);
```

Calendar类对象字段类型
Calendar类中用以下这些常量表示不同的意义，jdk内的很多类其实都是采用的这种思想

| 常量 | 描述 | 
 | --- | --- | 
 | Calendar.YEAR	 | 年份 | 
 | Calendar.MONTH	 | 月份 | 
 | Calendar.DATE	 | 日期 | 
 | Calendar.DAY_OF_MONTH	 | 日期，和上面的字段意义完全相同 | 
 | Calendar.HOUR	 | 12小时制的小时 | 
 | Calendar.HOUR_OF_DAY	 | 24小时制的小时 | 
 | Calendar.MINUTE	 | 分钟 | 
 | Calendar.SECOND	 | 秒 | 
 | Calendar.DAY_OF_WEEK	 | 星期几 | 

18. java 正则表达式

正则表达式定义了字符串的模式。

正则表达式可以用来搜索、编辑或处理文本。

正则表达式并不仅限于某一种语言，但是在每种语言中有细微的差别。 

- java.util.regex 包主要包括以下三个类：
	- Pattern 类：pattern 对象是一个正则表达式的编译表示。Pattern 类没有公共构造方法。要创建一个 Pattern 对象，你必须首先调用其公共静态编译方法，它返回一个 Pattern 对象。该方法接受一个正则表达式作为它的第一个参数。
	- Matcher 类：Matcher 对象是对输入字符串进行解释和匹配操作的引擎。与Pattern 类一样，Matcher 也没有公共构造方法。你需要调用 Pattern 对象的 matcher 方法来获得一个 Matcher 对象。
	- PatternSyntaxException：PatternSyntaxException 是一个非强制异常类，它表示一个正则表达式模式中的语法错误。

- 正则表达式语法

在其他语言中，\\ 表示：我想要在正则表达式中插入一个普通的（字面上的）反斜杠，请不要给它任何特殊的意义。

在 Java 中，\\ 表示：我要插入一个正则表达式的反斜线，所以其后的字符具有特殊的意义。

所以，在其他的语言中（如Perl），一个反斜杠 \ 就足以具有转义的作用，而在 Java 中正则表达式中则需要有两个反斜杠才能被解析为其他语言中的转义作用。也可以简单的理解在 Java 的正则表达式中，两个 \\ 代表其他语言中的一个 \，这也就是为什么表示一位数字的正则表达式是 \\d，而表示一个普通的反斜杠是 \\\\。

- matches 和 lookingAt 方法

matches 和 lookingAt 方法都用来尝试匹配一个输入序列模式。它们的不同是 matches 要求整个序列都匹配，而lookingAt 不要求。

lookingAt 方法虽然不需要整句都匹配，但是需要从第一个字符开始匹配。

- replaceFirst 和 replaceAll 方法
replaceFirst 和 replaceAll 方法用来替换匹配正则表达式的文本。不同的是，replaceFirst 替换首次匹配，replaceAll 替换所有匹配。