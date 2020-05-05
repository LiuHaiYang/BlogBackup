---
title: RUNOOB-java基础回顾3
date: 2020-03-23 20:48:31
tags: java
---

![image](https://images.pexels.com/photos/3047316/pexels-photo-3047316.jpeg?auto=compress&cs=tinysrgb&dpr=2&w=500)

---

12. java Number & Number 类
我们使用数字的时候，通常用的内置数据类型， 如 byte int long double 等

在实际开发过程中，我们经常会遇到需要使用对象，而不是内置数据类型的情形。为了解决这个问题，Java 语言为每一个内置数据类型提供了对应的包装类。

所有的包装类（Integer、Long、Byte、Double、Float、Short）都是抽象类 Number 的子类。

这种由编译器特别支持的包装称为装箱，所以当内置数据类型被当作对象使用的时候，编译器会把内置类型装箱为包装类。相似的，编译器也可以把一个对象拆箱为内置类型。Number 类属于 java.lang 包。

Java 的 Math 包含了用于执行基本数学运算的属性和方法，如初等指数、对数、平方根和三角函数。

Math 的方法都被定义为 static 形式，通过 Math 类可以在主函数中直接调用。

```
public class Test {  
    public static void main (String []args)  
    {  
        System.out.println("90 度的正弦值：" + Math.sin(Math.PI/2));  
        System.out.println("0度的余弦值：" + Math.cos(0));  
        System.out.println("60度的正切值：" + Math.tan(Math.PI/3));  
        System.out.println("1的反正切值： " + Math.atan(1));  
        System.out.println("π/2的角度值：" + Math.toDegrees(Math.PI/2));  
        System.out.println(Math.PI);  
    }  
}
```

以上实例编译运行结果如下：
```
90 度的正弦值：1.0
0度的余弦值：1.0
60度的正切值：1.7320508075688767
1的反正切值： 0.7853981633974483
π/2的角度值：90.0
3.141592653589793
```

Number & Math 类方法
下面的表中列出的是 Number & Math 类常用的一些方法：

 - xxxValue() 将 Number 对象转换为xxx数据类型的值并返回。
 - compareTo() 将number对象与参数比较。
 - equals() 判断number对象是否与参数相等。
 - valueOf() 返回一个 Number 对象指定的内置数据类型
 - toString() 以字符串形式返回值。
 - parseInt() 将字符串解析为int类型。
 - abs() 返回参数的绝对值。
 - ceil() 返回大于等于( >= )给定参数的的最小整数，类型为双精度浮点型。
 - floor() 返回小于等于（<=）给定参数的最大整数 。
 - rint() 返回与参数最接近的整数。返回类型为double。
 - round() 它表示四舍五入，算法为 Math.floor(x+0.5)，即将原来的数字加上 0.5 后再向下取整，所以，Math.round(11.5) 的结果为12，Math.round(-11.5) 的结果为-11。
 - min() 返回两个参数中的最小值。
 - max() 返回两个参数中的最大值。
 - exp() 返回自然数底数e的参数次方。
 - log() 返回参数的自然数底数的对数值。
 - pow() 返回第一个参数的第二个参数次方。
 - sqrt() 求参数的算术平方根。
 - sin() 求指定double类型参数的正弦值。
 - cos() 求指定double类型参数的余弦值。
 - tan() 求指定double类型参数的正切值。
 - asin() 求指定double类型参数的反正弦值。
 - acos() 求指定double类型参数的反余弦值。
 - atan() 求指定double类型参数的反正切值。
 - atan2() 将笛卡尔坐标转换为极坐标，并返回极坐标的角度值。
 - toDegrees() 将参数转化为角度。
 - toRadians() 将角度转换为弧度。
 - random() 返回一个随机数。


```
Math.floor(1.4)=1.0
Math.round(1.4)=1
Math.ceil(1.4)=2.0
Math.floor(1.5)=1.0
Math.round(1.5)=2
Math.ceil(1.5)=2.0
Math.floor(1.6)=1.0
Math.round(1.6)=2
Math.ceil(1.6)=2.0
Math.floor(-1.4)=-2.0
Math.round(-1.4)=-1
Math.ceil(-1.4)=-1.0
Math.floor(-1.5)=-2.0
Math.round(-1.5)=-1
Math.ceil(-1.5)=-1.0
Math.floor(-1.6)=-2.0
Math.round(-1.6)=-2
Math.ceil(-1.6)=-1.0
```

13. javaCharacter 类

Character 类用于对单个字符进行操作。

Character 类在对象中包装一个基本类型 char 的值

实例
```
char ch = 'a';
// Unicode 字符表示形式
char uniChar = '\u039A'; 
// 字符数组
char[] charArray ={ 'a', 'b', 'c', 'd', 'e' };
```

在实际开发过程中，我们经常会遇到需要使用对象，而不是内置数据类型的情况。为了解决这个问题，Java语言为内置数据类型char提供了包装类Character类。

Character类提供了一系列方法来操纵字符。你可以使用Character的构造方法创建一个Character类对象，例如：

```
Character ch = new Character('a');
在某些情况下，Java编译器会自动创建一个Character对象。
```

例如，将一个char类型的参数传递给需要一个Character类型参数的方法时，那么编译器会自动地将char类型参数转换为Character对象。 这种特征称为装箱，反过来称为拆箱。

转义序列
前面有反斜杠（\）的字符代表转义字符，它对编译器来说是有特殊含义的。

下面列表展示了Java的转义序列：

| 转义序列	| 描述 |
| --- | ---  | 
| \t	| 在文中该处插入一个tab键 | 
| \b	| 在文中该处插入一个后退键 | 
| \n	| 在文中该处换行 | 
| \r	| 在文中该处插入回车 | 
| \f	| 在文中该处插入换页符 | 
| \'	| 在文中该处插入单引号 | 
| \"	| 在文中该处插入双引号 | 
| \\	| 在文中该处插入反斜杠 | 


Character 方法
下面是Character类的方法：
 - isLetter() 是否是一个字母
 - isDigit() 是否是一个数字字符
 - isWhitespace() 是否是一个空白字符
 - isUpperCase() 是否是大写字母
 - isLowerCase() 是否是小写字母
 - toUpperCase() 指定字母的大写形式
 - toLowerCase() 指定字母的小写形式
 - toString() 返回字符的字符串形式，字符串的长度仅为1