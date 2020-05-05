---
title: RUNOOB-java基础回顾4
date: 2020-03-24 20:47:51
tags:
---

![image](https://images.pexels.com/photos/2439563/pexels-photo-2439563.jpeg?auto=compress&cs=tinysrgb&dpr=2&w=500)

---

14. java string 类
- 字符串广泛应用 在 Java 编程中，在 Java 中字符串属于对象，Java 提供了 String 类来创建和操作字符串。
- String 类是不可改变的，所以你一旦创建了 String 对象，那它的值就无法改变了
- 连接字符串 String 类提供了连接两个字符串的方法：
	- string1.concat(string2); 返回 string2 连接 string1 的新字符串。也可以对字符串常量使用 concat() 方法，如：
	- 更常用的是使用'+'操作符来连接字符串，如： "Hello," + " runoob" + "!"
- 创建格式化字符串
	- 输出格式化数字可以使用 printf() 和 format() 方法。
	- String 类使用静态方法 format() 返回一个String 对象而不是 PrintStream 对象。
	- String 类的静态方法 format() 能用来创建可复用的格式化字符串，而不仅仅是用于一次打印输出。
	```
	System.out.printf("浮点型变量的值为 " +
                  "%f, 整型变量的值为 " +
                  " %d, 字符串变量的值为 " +
                  "is %s", floatVar, intVar, stringVar);
	String fs;
	fs = String.format("浮点型变量的值为 " +
                   "%f, 整型变量的值为 " +
                   " %d, 字符串变量的值为 " +
	```
- String 方法
	- char charAt(int index) 返回指定索引处的 char 值。
	- int compareTo(Object o) 把这个字符串和另一个对象比较。
	- int compareTo(String anotherString) 按字典顺序比较两个字符串。
	- int compareToIgnoreCase(String str) 按字典顺序比较两个字符串，不考虑大小写。
	- String concat(String str) 将指定字符串连接到此字符串的结尾。
	- boolean contentEquals(StringBuffer sb) 当且仅当字符串与指定的StringBuffer有相同顺序的字符时候返回真。
	- static String copyValueOf(char[] data) 返回指定数组中表示该字符序列的 String。
	- static String copyValueOf(char[] data, int offset, int count) 返回指定数组中表示该字符序列的 String。
	- boolean endsWith(String suffix) 测试此字符串是否以指定的后缀结束。
	- boolean equals(Object anObject) 将此字符串与指定的对象比较。
	- boolean equalsIgnoreCase(String anotherString) 将此 String 与另一个 String 比较，不考虑大小写。
	- byte[] getBytes() 使用平台的默认字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。
	- byte[] getBytes(String charsetName) 使用指定的字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。
	- void getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin) 将字符从此字符串复制到目标字符数组。
	- int hashCode() 返回此字符串的哈希码。
	- int indexOf(int ch) 返回指定字符在此字符串中第一次出现处的索引。
	- int indexOf(int ch, int fromIndex) 返回在此字符串中第一次出现指定字符处的索引，从指定的索引开始搜索。
	- int indexOf(String str) 返回指定子字符串在此字符串中第一次出现处的索引。
	- int indexOf(String str, int fromIndex) 返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始。
	- String intern() 返回字符串对象的规范化表示形式。
	- int lastIndexOf(int ch) 返回指定字符在此字符串中最后一次出现处的索引。
	- int lastIndexOf(int ch, int fromIndex) 返回指定字符在此字符串中最后一次出现处的索引，从指定的索引处开始进行反向搜索。
	- int lastIndexOf(String str) 返回指定子字符串在此字符串中最右边出现处的索引。
	- int lastIndexOf(String str, int fromIndex) 返回指定子字符串在此字符串中最后一次出现处的索引，从指定的索引开始反向搜索。
	- int length() 返回此字符串的长度。
	- boolean matches(String regex) 告知此字符串是否匹配给定的正则表达式。
	- boolean regionMatches(boolean ignoreCase, int toffset, String other, int ooffset, int len) 测试两个字符串区域是否相等。
	- boolean regionMatches(int toffset, String other, int ooffset, int len) 测试两个字符串区域是否相等。
	- String replace(char oldChar, char newChar) 返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 得到的。
	- String replaceAll(String regex, String replacement) 使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串。
	- String replaceFirst(String regex, String replacement) 使用给定的 replacement 替换此字符串匹配给定的正则表达式的第一个子字符串。
	- String[] split(String regex) 根据给定正则表达式的匹配拆分此字符串。
	- String[] split(String regex, int limit) 根据匹配给定的正则表达式来拆分此字符串。
	- boolean startsWith(String prefix) 测试此字符串是否以指定的前缀开始。
	- boolean startsWith(String prefix, int toffset) 测试此字符串从指定索引开始的子字符串是否以指定前缀开始。
	- CharSequence subSequence(int beginIndex, int endIndex) 返回一个新的字符序列，它是此序列的一个子序列。
	- String substring(int beginIndex) 返回一个新的字符串，它是此字符串的一个子字符串。
	- String substring(int beginIndex, int endIndex) 返回一个新字符串，它是此字符串的一个子字符串。
	- char[] toCharArray() 将此字符串转换为一个新的字符数组。
	- String toLowerCase() 使用默认语言环境的规则将此 String 中的所有字符都转换为小写。
	- String toLowerCase(Locale locale) 使用给定 Locale 的规则将此 String 中的所有字符都转换为小写。
	- String toString() 返回此对象本身（它已经是一个字符串！）。
	- String toUpperCase() 使用默认语言环境的规则将此 String 中的所有字符都转换为大写。
	- String toUpperCase(Locale locale) 使用给定 Locale 的规则将此 String 中的所有字符都转换为大写。
	- String trim() 返回字符串的副本，忽略前导空白和尾部空白。
	- static String valueOf(primitive data type x) 返回给定data type类型x参数的字符串表示形式。


15. Java StringBuffer 和 StringBuilder 类
 - 当对字符串进行修改的时候，需要使用 StringBuffer 和 StringBuilder 类。和 String 类不同的是，StringBuffer 和 StringBuilder 类的对象能够被多次的修改，并且不产生新的未使用对象。
 - StringBuilder 类在 Java 5 中被提出，它和 StringBuffer 之间的最大不同在于 StringBuilder 的方法不是线程安全的（不能同步访问）。
 - 由于 StringBuilder 相较于 StringBuffer 有速度优势，所以多数情况下建议使用 StringBuilder 类。然而在应用程序要求线程安全的情况下，则必须使用 StringBuffer 类。
```
Test.java 文件代码：
public class Test{
  public static void main(String args[]){
    StringBuffer sBuffer = new StringBuffer("菜鸟教程官网：");
    sBuffer.append("www");
    sBuffer.append(".runoob");
    sBuffer.append(".com");
    System.out.println(sBuffer);  
  }
}
```

- StringBuffer 方法
	- public StringBuffer append(String s) 将指定的字符串追加到此字符序列。
	- public StringBuffer reverse() 将此字符序列用其反转形式取代。
	- public delete(int start, int end) 移除此序列的子字符串中的字符。
	- public insert(int offset, int i) 将 int 参数的字符串表示形式插入此序列中。
	- replace(int start, int end, String str) 使用给定 String 中的字符替换此序列的子字符串中的字符。

- 和 String 类的方法类似：
	- int capacity() 返回当前容量。
	- char charAt(int index) 返回此序列中指定索引处的 char 值。
	- void ensureCapacity(int minimumCapacity) 确保容量至少等于指定的最小值。
	- void getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin) 将字符从此序列复制到目标字符数组 dst。
	- int indexOf(String str) 返回第一次出现的指定子字符串在该字符串中的索引。
	- int indexOf(String str, int fromIndex) 从指定的索引处开始，返回第一次出现的指定子字符串在该字符串中的索引。
	- int lastIndexOf(String str) 返回最右边出现的指定子字符串在此字符串中的索引。
	- int lastIndexOf(String str, int fromIndex) 返回 String 对象中子字符串最后出现的位置。
	- int length() 返回长度（字符数）。
	- void setCharAt(int index, char ch) 将给定索引处的字符设置为 ch。
	- void setLength(int newLength) 设置字符序列的长度。
	- CharSequence subSequence(int start, int end) 返回一个新的字符序列，该字符序列是此序列的子序列。
	- String substring(int start) 返回一个新的 String，它包含此字符序列当前所包含的字符子序列。
	- String substring(int start, int end) 返回一个新的 String，它包含此序列当前所包含的字符子序列。
	- String toString() 返回此序列中数据的字符串表示形式。

16. java 数组

- Java 语言中提供的数组是用来存储固定大小的同类型元素。声明一个数组变量，如 numbers[100] 来代替直接声明 100 个独立变量 number0，number1，....，number99。
- 创建数组
	Java语言使用new操作符来创建数组，语法如下：
	arrayRefVar = new dataType[arraySize];
	上面的语法语句做了两件事：
	```
		一、使用 dataType[arraySize] 创建了一个数组。
		二、把新创建的数组的引用赋值给变量 arrayRefVar。
	```
	数组变量的声明，和创建数组可以用一条语句完成，如下所示：
	```
	dataType[] arrayRefVar = new dataType[arraySize];
	另外，你还可以使用如下的方式创建数组。
	dataType[] arrayRefVar = {value0, value1, ..., valuek};
	```

- 数组的元素是通过索引访问的。数组索引从 0 开始，所以索引值从 0 到 arrayRefVar.length-1。
- 多维数组
	- 多维数组可以看成是数组的数组，比如二维数组就是一个特殊的一维数组，其每一个元素都是一个一维数组，例如：
	- String str[][] = new String[3][4];
	- 多维数组的动态初始化（以二维数组为例）
	```
	1. 直接为每一维分配空间，格式如下：
	type[][] typeName = new type[typeLength1][typeLength2];
	type 可以为基本数据类型和复合数据类型，arraylength1 和 arraylength2 必须为正整数，arraylength1 为行数，arraylength2 为列数。
	例如： int a[][] = new int[2][3];
	解析：二维数组 a 可以看成一个两行三列的数组。
	2. 从最高维开始，分别为每一维分配空间，例如：
	String s[][] = new String[2][];
	s[0] = new String[2];
	s[1] = new String[3];
	s[0][0] = new String("Good");
	s[0][1] = new String("Luck");
	s[1][0] = new String("to");
	s[1][1] = new String("you");
	s[1][2] = new String("!");
	解析：
	s[0]=new String[2] 和 s[1]=new String[3] 是为最高维分配引用空间，也就是为最高维限制其能保存数据的最长的长度，然后再为其每个数组元素单独分配空间 s0=new String("Good") 等操作。
	多维数组的引用（以二维数组为例） 对二维数组中的每个元素，引用方式为 arrayName[index1][index2]，例如：num[1][0];
	```

- Arrays 类
	- java.util.Arrays 类能方便地操作数组，它提供的所有方法都是静态的。
	- 具有以下功能：
		- 给数组赋值：通过 fill 方法。
		- 对数组排序：通过 sort 方法,按升序。
		- 比较数组：通过 equals 方法比较数组中元素值是否相等。
		- 查找数组元素：通过 binarySearch 方法能对排序好的数组进行二分查找法操作。

具体说明请查看下表：

 | 序号	 | 方法 | 说明 | 
  | --- | --- |  --- | 
 | 1	 | public static int binarySearch(Object[] a, Object key)  | 用二分查找算法在给定数组中搜索给定值的对象(Byte,Int,double等)。数组在调用前必须排序好的。如果查找值包含在数组中，则返回搜索键的索引；否则返回 (-(插入点) - 1)。 | 
 | 2	 | public static boolean equals(long[] a, long[] a2) | 如果两个指定的 long 型数组彼此相等，则返回 true。如果两个数组包含相同数量的元素，并且两个数组中的所有相应元素对都是相等的，则认为这两个数组是相等的。换句话说，如果两个数组以相同顺序包含相同的元素，则两个数组是相等的。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。 | 
 | 3	 | public static void fill(int[] a, int val) | 将指定的 int 值分配给指定 int 型数组指定范围中的每个元素。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。 | 
 | 4 	 | public static void sort(Object[] a) | 对指定对象数组根据其元素的自然顺序进行升序排列。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。 | 




