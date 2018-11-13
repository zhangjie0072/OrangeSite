---
title : 版本控制之道——添加与提交：Git基础（之三）
---

#### 第一节 添加文件到暂存区

**暂存的变更**就是工作目录树中那些你打算提交到版本库中的变更。暂存操作将会更新Git的内部索引。大家常把该索引称为**暂存区**。

通过暂存区，可以设置哪些变更要啊提交到版本库，哪些先不提交。是通过git add命令来实现的。

git add命令加-i参数。该命令会启动交互命令提示符。

把index.html改了句代码如下

```
$ cat index.html
<html>
        <head>
                <title>hello world in Git</title>
                <meta name = "description" content="hello world in Git"/>
        </head>
        <body>
                <li><a href="bio.html">about</a></li>
                <h1>hello world!</h1>
        </body>
</html>

```

然后执行git add -i命令

```
Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (master)
$ vi index.html

Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (master)
$ git add -i
           staged     unstaged path
  1:    unchanged        +1/-1 index.html

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now> 2
           staged     unstaged path
  1:    unchanged        +1/-1 index.html
Update>> 1
           staged     unstaged path
* 1:    unchanged        +1/-1 index.html
Update>>
updated 1 path

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now> 1
           staged     unstaged path
  1:        +1/-1      nothing index.html

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now> 3
           staged     unstaged path
  1:        +1/-1      nothing index.html
Revert>> 1
           staged     unstaged path
* 1:        +1/-1      nothing index.html
Revert>>
reverted 1 path

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now> 1
           staged     unstaged path
  1:    unchanged        +1/-1 index.html

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now> 5
           staged     unstaged path
  1:    unchanged        +1/-1 index.html
Patch update>> 1
           staged     unstaged path
* 1:    unchanged        +1/-1 index.html
Patch update>>
diff --git a/index.html b/index.html
index 9dbc6fa..48f7176 100644
--- a/index.html
+++ b/index.html
@@ -4,7 +4,7 @@
                <meta name = "description" content="hello world in Git"/>
        </head>
        <body>
-               <li><a href="bio.html">Biography</a></li>
+               <li><a href="bio.html">about</a></li>
                <h1>hello world!</h1>
        </body>
 </html>
Stage this hunk [y,n,q,a,d,e,?]? ?
y - stage this hunk
n - do not stage this hunk
q - quit; do not stage this hunk or any of the remaining ones
a - stage this hunk and all later hunks in the file
d - do not stage this hunk or any of the later hunks in the file
e - manually edit the current hunk
? - print help
@@ -4,7 +4,7 @@
                <meta name = "description" content="hello world in Git"/>
        </head>
        <body>
-               <li><a href="bio.html">Biography</a></li>
+               <li><a href="bio.html">about</a></li>
                <h1>hello world!</h1>
        </body>
 </html>
Stage this hunk [y,n,q,a,d,e,?]? n

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now> 7
Bye.
```

**git add -i**，进入一级菜单，选一种操作，然后进行二级菜单进行对应的操作。

想要添加文件到暂存区，输入2；下边显示出了有1个文件需要添加，所以输入1.该文件前边出现*，表示该文件将暂存。如果想退出该模式，直接回车键就又回到主菜单了。

回到主菜单后，选1，可以看到有一个已暂存的修改，而没有未暂存的修改了。

如果想要取消已暂存的修改，可以使用revert模式。选3，然后1个文件，选1，然后回车键出来回到主菜单。选1查看之前的修改变成了未暂存的状态。

如果想要暂存还没有被git跟踪的文件，一开始选2是update,说明暂存的是已在版本库里的文件。没暂存的选4.

“patch”模式，进入patch模式后，可以选择单个或多个文件，选择后，git会显示这些文件的当前内容与版本库中的差异，然后你可以根据此决定是否添加这些修改到暂存区。操作：5，1，回车。

可以根据需要决定是否添加该文本块。文本块由文件中连续的修改构成，文件中不同的区域作为不同的文本块。

输入y：接受修改； n：忽略修改；a：添加剩余修改 d:放弃剩余修改 ?：帮助

**git add -p**直接进入补丁模式

```
$ git add -p
diff --git a/index.html b/index.html
index 9dbc6fa..48f7176 100644
--- a/index.html
+++ b/index.html
@@ -4,7 +4,7 @@
                <meta name = "description" content="hello world in Git"/>
        </head>
        <body>
-               <li><a href="bio.html">Biography</a></li>
+               <li><a href="bio.html">about</a></li>
                <h1>hello world!</h1>
        </body>
 </html>
Stage this hunk [y,n,q,a,d,e,?]? y
```

输入y选择文件块，然后该文件就处于暂存状态并准备提交。

#### 第二节 提交修改

*Git不跟踪空目录。此前的解决办法，在想要跟踪的目录里添加一个空文件，并把该文件添加到版本控制中。这个空文件的文件名可以是任意的名称，但是大家通常用于以.开头的文件名。。因为大多数文件系统在显示目录内容时，会忽略以句点开头的文件。*

提交并不是把修改发送到某个中央版本库中，而是提交到本地版本库。

但其他人可以从你的本地版本库中拖入修改，或者你从本地版本库推入某个其他的版本库。

git commit命令必须提交留言。 可以使用一个或多个-m。

如果不输入-m参数，Git将启动编辑器来编辑提交留言。为启动编辑器，Git会按照以下顺序来查找编辑器的设置：

1. 环境变量GIT_EDITOR的值；
2. Git的设置 core.editor的值
3. 环境变量VISUAL的值
4. 环境变量EDITOR的值
5. 如果上述值均为空工，Git会尝试启动vi编辑器

下面时git启动vi编辑器的情况。

```
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# Changes to be committed:
#       modified:   index.html
#
# Untracked files:
#       mysite-1.0.tar.gz
#
~
```

下面学习三种提交方法

1. 【提交暂存后的修改】 git add（/git add -p）加入暂存区； git commit -m 完成提交。

2. 【提交工作目录树中的所有修改】git commit -m "changes" -a

   Git会把工作目录树中当前所有的修改提交到版本库中。这种情况下，git只会把已纳入版本控制的文件添加到版本库，而不会添加尚未被跟踪的文件。

3. 【提交工作目录树中指定的修改】git commit -m "changes" some-file-name

   提交指定的文件或文件列表

   第一种适用于提交文件的一部分修改时，用先暂存后提交的方法比较合适。后两种时直接提交。

   #### 第三节 查看修改内容

   查看当前状态 **git status**

   该命令的输出结果是暂存区内要提交的内容，工作目录树中未纳入暂存区的改动，以及尚未纳入Git版本控制的新文件。以下显示的是（待提交变更）部分。

   ```
   $ git status
   On branch master
   Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)
   
           modified:   index.html
   ```

   下面修改index.xml，插入了<li><a href="contact.html">Contact</a></li>这句话。

   ```
   $ cat index.html
   <html>
           <head>
                   <title>hello world in Git</title>
                   <meta name = "description" content="hello world in Git"/>
           </head>
           <body>
                   <li><a href="bio.html">about</a></li>
                   <li><a href="contact.html">Contact</a></li>
                   <h1>hello world!</h1>
           </body>
   </html>
   ```

   这时候再执行git status命令

   ```
   $ git status
   On branch master
   Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)
   
           modified:   index.html
   
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git checkout -- <file>..." to discard changes in working directory)
   
           modified:   index.html
   
   ```

   可以看到index.html文件显示了两遍。Changes to be committed（待提交变更）部分中的那个对应之前已经暂存的修改；和Changed but not updated（未更新到索引的变更）部分中的那个对应还未暂存的修改。颜色分别为绿色和红色。

   用**git diff** 命令来查看文件的改动：git diff命令不加参数，比较的是工作目录树和暂存区之间的区别。

   ```
   $ git diff
   diff --git a/index.html b/index.html
   index 48f7176..60d0512 100644
   --- a/index.html
   +++ b/index.html
   @@ -5,6 +5,7 @@
           </head>
           <body>
                   <li><a href="bio.html">about</a></li>
   +               <li><a href="contact.html">Contact</a></li>
                   <h1>hello world!</h1>
           </body>
    </html>
   ```

   **git diff --cached**命令，比较暂存区和版本库中的区别: -表示该行被删除了；git删除的内容标记为红色，添加的内容则为绿色。

   ```
   $ git diff --cached
   diff --git a/index.html b/index.html
   index 9dbc6fa..48f7176 100644
   --- a/index.html
   +++ b/index.html
   @@ -4,7 +4,7 @@
                   <meta name = "description" content="hello world in Git"/>
           </head>
           <body>
   -               <li><a href="bio.html">Biography</a></li>
   +               <li><a href="bio.html">about</a></li>
                   <h1>hello world!</h1>
           </body>
    </html>
   ```

   git diff HEAD比较工作目录树（包括暂存和未暂存的修改）与版本库中的差别： HEAD表示当前所在分支末梢的最新提交。

   ```
   $ git diff HEAD
   diff --git a/index.html b/index.html
   index 9dbc6fa..60d0512 100644
   --- a/index.html
   +++ b/index.html
   @@ -4,7 +4,8 @@
                   <meta name = "description" content="hello world in Git"/>
           </head>
           <body>
   -               <li><a href="bio.html">Biography</a></li>
   +               <li><a href="bio.html">about</a></li>
   +               <li><a href="contact.html">Contact</a></li>
                   <h1>hello world!</h1>
           </body>
    </html>
   ```

#### 第四节 管理文件

1. 文件重命名和移动 git mv <原文件名> <新文件名>，该命令告诉Git使用原文件的内容来创建新文件，新文件保留原文件的历史修改记录，并删除原文件。

```
$ git mv index.html hello.html
   
Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (master)
$ git status
On branch master
Changes to be committed:
	(use "git reset HEAD <file>..." to unstage)
   
        renamed:    index.html -> hello.html
   
	Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)
   
        modified:   hello.html
```

2. git并不跟踪管理文件，而是跟踪文件内容。理由：文件名只是附在文件内容上的元数据，以便比较容易地组织文件目录树。Git可以询问文件系统以获得文件名，但Git真正关心的是文件本身的内容。

既然Git仅跟踪文件内容，那么他是如何管理文件复制的呢？实际上，无需额外告诉Git存在文件复制，Git自己就可以监测到他。

3. 忽略文件

.gitignore 来设置版本库级别的忽略； .git/info/exclude来设置本地级别的忽略。

完结（2018.11.12）