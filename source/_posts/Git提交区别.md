---
title: Git提交区别
date: 2019-09-13 14:48:40
tags: Git
---
#### git commit -m 和 -am 的区别

理论：
- git commit -m  用于提交暂存区的文件， git commit -am 用于提交跟踪过的文件
- git commit -am 可以写成 git commit -a -m 
- 定义中出现了暂存区，跟踪过的文件等术语，如果要理解他们，就需要了解Git的文件状态变化周期

工作目录下面的所有文件都不外乎这两种状态：已跟踪或未跟踪。
已跟踪的文件是指本来就被纳入版本控制管理的文件，在上次快照中有他们的记录，工作一段事件后，他们的状态可能是未更新(unmodified),已修改(modifled)或者已放入暂存区。


实例说明：
1、在项目文件夹中新增一个文件如'a.txt'时，该文件处于未跟踪状态(untracked)。未跟踪状态的文件是无法提交的

2、接下来，使用git add a.txt，使其变成已跟踪状态(tracked)

3、这时，如果使用git commit -m 'add a.txt'就可以顺利提交了

4、但是，git commit -m 和 git commit -am的区别在哪里？在于a.txt文件修改之后的处理

下面，向a.txt添加内容'a'。可以看出，文件a.txt处于已跟踪(tracked)，但未暂存状态(unstaged)

5、这时，如果使用git commit -m是无法提交最新版本的a.txt的，提交的只是最开始空内容的旧版本a.txt

6、而如果使用git commit -am，则可以省略git add a.txt这一步，因为git commit -am可以提交跟踪过的文件，而a.txt一开始已经被跟踪过了

### 总结

　　这两个命令的区别的关键就是git add命令

　　git add命令是个多功能命令，根据目标文件的状态不同，此命令的效果也不同：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等

　　我们需要用git add命令来跟踪新文件，但如果使用git commit -am可以省略使用git add命令将已跟踪文件放到暂存区的功能

===

好的代码像粥一样，都是用时间熬出来的。

![image](https://images.pexels.com/photos/2726478/pexels-photo-2726478.jpeg?auto=compress&cs=tinysrgb&dpr=2&w=500)
