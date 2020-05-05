---
title: RUNOOB-java高级教程-4
date: 2020-05-04 23:11:26
tags: java
---

### 6. java 发送邮件

使用Java应用程序发送 E-mail 十分简单，但是首先你应该在你的机器上安装 JavaMail API 和Java Activation Framework (JAF) 。
- 可以从 Java 网站下载最新版本的 JavaMail，打开网页右侧有个 Downloads 链接，点击它下载。
- 可以从 Java 网站下载最新版本的 JAF（版本 1.1.1）。

```
// 文件名 SendEmail.java
 
import java.util.*;
import javax.mail.*;
import javax.mail.internet.*;
import javax.activation.*;
 
public class SendEmail
{
   public static void main(String [] args)
   {   
      // 收件人电子邮箱
      String to = "abcd@gmail.com";
 
      // 发件人电子邮箱
      String from = "web@gmail.com";
 
      // 指定发送邮件的主机为 localhost
      String host = "localhost";
 
      // 获取系统属性
      Properties properties = System.getProperties();
 
      // 设置邮件服务器
      properties.setProperty("mail.smtp.host", host);
 
      // 获取默认session对象
      Session session = Session.getDefaultInstance(properties);
 
      try{
         // 创建默认的 MimeMessage 对象
         MimeMessage message = new MimeMessage(session);
 
         // Set From: 头部头字段
         message.setFrom(new InternetAddress(from));
 
         // Set To: 头部头字段
         message.addRecipient(Message.RecipientType.TO,
                                  new InternetAddress(to));
 
         // Set Subject: 头部头字段
         message.setSubject("This is the Subject Line!");
 
         // 设置消息体
         message.setText("This is actual message");
 
         // 发送消息
         Transport.send(message);
         System.out.println("Sent message successfully....");
      }catch (MessagingException mex) {
         mex.printStackTrace();
      }
   }
}
```

下面是对于参数的描述：
- type:要被设置为 TO, CC 或者 BCC，这里 CC 代表抄送、BCC 代表秘密抄送。举例：Message.RecipientType.TO
- addresses: 这是 email ID 的数组。在指定电子邮件 ID 时，你将需要使用 InternetAddress() 方法。


#### 发送一封 HTML E-mail
下面是一个发送 HTML E-mail 的例子。假设你的本地主机已经连接到网络。

和上一个例子很相似，除了我们要使用 setContent() 方法来通过第二个参数为 "text/html"，来设置内容来指定要发送HTML 内容。

```
         // 发送 HTML 消息, 可以插入html标签
         message.setContent("<h1>This is actual message</h1>",
                            "text/html" );
```

#### 发送分带有附件的邮件
```
// 消息
         messageBodyPart.setText("This is message body");
        
         // 创建多重消息
         Multipart multipart = new MimeMultipart();
 
         // 设置文本消息部分
         multipart.addBodyPart(messageBodyPart);
 
         // 附件部分
         messageBodyPart = new MimeBodyPart();
         String filename = "file.txt";
         DataSource source = new FileDataSource(filename);
         messageBodyPart.setDataHandler(new DataHandler(source));
         messageBodyPart.setFileName(filename);
         multipart.addBodyPart(messageBodyPart);
 
         // 发送完整消息
         message.setContent(multipart );
 
         //   发送消息
         Transport.send(message);
 ```












