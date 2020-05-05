---
title: RUNOOB-java基础回顾6
date: 2020-03-26 22:46:19
tags: java
---

19. Java 方法

我们经常使用到 System.out.println()，那么它是什么呢？
 - println() 是一个方法。
 - System 是系统类。
 - out 是标准输出对象。
这句话的用法是调用系统类 System 中的标准输出对象 out 中的方法 println()。

那么什么是方法呢？
 - Java方法是语句的集合，它们在一起执行一个功能。
 - 方法是解决一类问题的步骤的有序组合
 - 方法包含于类或对象中
 - 方法在程序中被创建，在其他地方被引用

方法的优点
 - 使程序变得更简短而清晰。
 - 有利于程序维护。
 - 可以提高程序开发的效率。
 - 提高了代码的重用性。

方法的命名规则
 - 方法的名字的第一个单词应以小写字母作为开头，后面的单词则用大写字母开头写，不使用连接符。例如：addPerson。
 - 下划线可能出现在 JUnit 测试方法名称中用以分隔名称的逻辑组件。一个典型的模式是：`test<MethodUnderTest>_<state>，例如 testPop_emptyStack。`

方法包含一个方法头和一个方法体。下面是一个方法的所有部分：
 - 修饰符：修饰符，这是可选的，告诉编译器如何调用该方法。定义了该方法的访问类型。
 - 返回值类型 ：方法可能会返回值。returnValueType 是方法返回值的数据类型。有些方法执行所需的操作，但没有返回值。在这种情况下，returnValueType 是关键字void。
 - 方法名：是方法的实际名称。方法名和参数表共同构成方法签名。
 - 参数类型：参数像是一个占位符。当方法被调用时，传递值给参数。这个值被称为实参或变量。参数列表是指方法的参数类型、顺序和参数的个数。参数是可选的，方法可以不包含任何参数。
 - 方法体：方法体包含具体的语句，定义该方法的功能。

构造方法
 - 当一个对象被创建时候，构造方法用来初始化该对象。构造方法和它所在类的名字相同，但构造方法没有返回值。
 - 通常会使用构造方法给一个类的实例变量赋初值，或者执行其它必要的步骤来创建一个完整的对象。
 - 不管你是否自定义构造方法，所有的类都有构造方法，因为Java自动提供了一个默认构造方法，默认构造方法的访问修改符和类的访问修改符相同(类为 public，构造函数也为 public；类改为 protected，构造函数也改为 protected)。
 - 一旦你定义了自己的构造方法，默认构造方法就会失效。

finalize() 方法
 - Java 允许定义这样的方法，它在对象被垃圾收集器析构(回收)之前调用，这个方法叫做 finalize( )，它用来清除回收对象。
 - 例如，你可以使用 finalize() 来确保一个对象打开的文件被关闭了。
 - 在 finalize() 方法里，你必须指定在对象销毁时候要执行的操作。
 finalize() 一般格式是：
 ```
	protected void finalize()
	{
	   // 在这里终结代码
	}
 ```

20. Java 流(Stream)、文件(File)和IO

Java.io 包几乎包含了所有操作输入、输出需要的类。所有这些流类代表了输入源和输出目标。

Java.io 包中的流支持很多种格式，比如：基本类型、对象、本地化字符集等等。

一个流可以理解为一个数据的序列。输入流表示从一个源读取数据，输出流表示向一个目标写数据。

Java 为 I/O 提供了强大的而灵活的支持，使其更广泛地应用到文件传输和网络编程中。


- 读取控制台输入

Java 的控制台输入由 System.in 完成。
为了获得一个绑定到控制台的字符流，你可以把 System.in 包装在一个 BufferedReader 对象中来创建一个字符流。

下面是创建 BufferedReader 的基本语法：
`BufferedReader br = new BufferedReader(new InputStreamReader(System.in));`

BufferedReader 对象创建后，我们便可以使用 read() 方法从控制台读取一个字符，或者用 readLine() 方法读取一个字符串。

每次调用 read() 方法，它从输入流读取一个字符并把该字符作为整数值返回。 当流结束的时候返回 -1。该方法抛出 IOException。

下面的程序示范了用 read() 方法从控制台不断读取字符直到用户输入 "q"。

BRRead.java 文件代码：
//使用 BufferedReader 在控制台读取字符
```
import java.io.*;
public class BRRead {
    public static void main(String args[]) throws IOException {
        char c;
        // 使用 System.in 创建 BufferedReader
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        System.out.println("输入字符, 按下 'q' 键退出。");
        // 读取字符
        do {
            c = (char) br.read();
            System.out.println(c);
        } while (c != 'q');
    }
}
```

- 从控制台读取字符串
从标准输入读取一个字符串需要使用 BufferedReader 的 readLine() 方法。

它的一般格式是：

String readLine( ) throws IOException
下面的程序读取和显示字符行直到你输入了单词"end"。

BRReadLines.java 文件代码：
```
//使用 BufferedReader 在控制台读取字符
import java.io.*;
public class BRReadLines {
    public static void main(String args[]) throws IOException {
        // 使用 System.in 创建 BufferedReader
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str;
        System.out.println("Enter lines of text.");
        System.out.println("Enter 'end' to quit.");
        do {
            str = br.readLine();
            System.out.println(str);
        } while (!str.equals("end"));
    }
}
```

- 控制台输出
控制台的输出由 print( ) 和 println() 完成。这些方法都由类 PrintStream 定义，System.out 是该类对象的一个引用。

PrintStream 继承了 OutputStream类，并且实现了方法 write()。这样，write() 也可以用来往控制台写操作。

PrintStream 定义 write() 的最简单格式如下所示：

void write(int byteval)
该方法将 byteval 的低八位字节写到流中。

实例
下面的例子用 write() 把字符 "A" 和紧跟着的换行符输出到屏幕：
WriteDemo.java 文件代码：
```
import java.io.*;
//演示 System.out.write().
public class WriteDemo {
    public static void main(String args[]) {
        int b;
        b = 'A';
        System.out.write(b);
        System.out.write('\n');
    }
}
```

注意：write() 方法不经常使用，因为 print() 和 println() 方法用起来更为方便。

### 读写文件

![image](https://www.runoob.com/wp-content/uploads/2013/12/iostream2xx.png)

- 下面将要讨论的两个重要的流是 FileInputStream 和 FileOutputStream：

FileInputStream

	该流用于从文件读取数据，它的对象可以用关键字 new 来创建。

	可以使用字符串类型的文件名来创建一个输入流对象来读取文件：

	InputStream f = new FileInputStream("C:/java/hello");
	也可以使用一个文件对象来创建一个输入流对象来读取文件。我们首先得使用 File() 方法来创建一个文件对象：

	File f = new File("C:/java/hello");
	InputStream out = new FileInputStream(f);

FileOutputStream

	该类用来创建一个文件并向文件中写数据。

	如果该流在打开文件进行输出前，目标文件不存在，那么该流会创建该文件。

	有两个构造方法可以用来创建 FileOutputStream 对象。

	使用字符串类型的文件名来创建一个输出流对象：

	OutputStream f = new FileOutputStream("C:/java/hello")
	也可以使用一个文件对象来创建一个输出流来写文件。我们首先得使用File()方法来创建一个文件对象：

	File f = new File("C:/java/hello");
	OutputStream f = new FileOutputStream(f);


文件和I/O

	File Class(类)
	FileReader Class(类)
	FileWriter Class(类)

Java中的目录

创建目录：File类中有两个方法可以用来创建文件夹：

	mkdir( )方法创建一个文件夹，成功则返回true，失败则返回false。失败表明File对象指定的路径已经存在，或者由于整个路径还不存在，该文件夹不能被创建。
	mkdirs()方法创建一个文件夹和它的所有父文件夹。


读取目录

一个目录其实就是一个 File 对象，它包含其他文件和文件夹。

如果创建一个 File 对象并且它是一个目录，那么调用 isDirectory() 方法会返回 true。

可以通过调用该对象上的 list() 方法，来提取它包含的文件和文件夹的列表。

下面展示的例子说明如何使用 list() 方法来检查一个文件夹中包含的内容：
DirList.java 文件代码：

```
import java.io.File;
public class DirList {
    public static void main(String args[]) {
        String dirname = "/tmp";
        File f1 = new File(dirname);
        if (f1.isDirectory()) {
            System.out.println("目录 " + dirname);
            String s[] = f1.list();
            for (int i = 0; i < s.length; i++) {
                File f = new File(dirname + "/" + s[i]);
                if (f.isDirectory()) {
                    System.out.println(s[i] + " 是一个目录");
                } else {
                    System.out.println(s[i] + " 是一个文件");
                }
            }
        } else {
            System.out.println(dirname + " 不是一个目录");
        }
    }
}
```

删除目录或文件

删除文件可以使用 java.io.File.delete() 方法。需要注意的是当删除某一目录时，必须保证该目录下没有其他文件才能正确删除，否则将删除失败。

```
import java.io.File;
public class DeleteFileDemo {
    public static void main(String args[]) {
        // 这里修改为自己的测试目录
        File folder = new File("/tmp/java/");
        deleteFolder(folder);
    }
 
    // 删除文件及目录
    public static void deleteFolder(File folder) {
        File[] files = folder.listFiles();
        if (files != null) {
            for (File f : files) {
                if (f.isDirectory()) {
                    deleteFolder(f);
                } else {
                    f.delete();
                }
            }
        }
        folder.delete();
    }
}
```