---
title : 用Git创建一个项目
---

#### 预习：Git设置

Git需要用户提供若干信息。Git是分布式的，因此不存在向用户询问姓名和邮件地址的中央版本库。通过命令git config，用户可以把此类信息提供给本地版本库。

须要设定一些全局变量的值。“全局”的意思是，你在这台计算机上使用任何Git版本库时，这些全局变量的值都起作用。

user.name（标识修改是谁提交的）和user.email（方便联系修改者） 两个全局变量是必须设置的。

```
$ git config --global user.name "Travis Swicegood"
$ git config --global user.email "development@domain51.com"
$ git config --global --list
```

以上三个命令分别设置user.name和user.email，以及检查确认global设置。

#### 第一节 **创建版本库**

```
$ mkdir mysite
$ cd mysite
$ git init
```

创建完成。这个Git版本库就可以用来记录和跟踪该项目的代码了。

**git init**命令会创建一个.git目录。这个目录用来存放版本库的全部元数据。mysite目录作为工作目录树，存放从版本库中检出的代码。

#### 第二节 代码修改

在mysite目录中，创建一个index.html文件，内容如下：

```
<html>
        <body>
                <h1>hello world!</h1>
        </body>
</html>
```

要想让Git跟踪这个文件，须先让它知道这个文件要分两步走：首先使用**git add**命令把该文件添加到版本库的索引；然后使用**git commit**命令提交。

```
$ git add index.html
$ git commit -m "add in hello world html"
[master (root-commit) 6e71f4e] add in hello world html
 1 file changed, 6 insertions(+)
 create mode 100644 index.html
```

查看提交记录

```
$ git log
commit 6e71f4eb68c0b94f65bf3ff9c4fbeafec3bfe9b1 (HEAD -> master)
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Thu Nov 8 14:16:21 2018 +0800

    add in hello world html
```

第一行显示提交名称，该名称是Git自动产生的SHA-1码。Git通过它来追踪提交。Git使用哈希码可以保证每个提交的名称都是独一无二的。这在分布式的环境中非常重要。

可以看到git log输出的提交名称的前七位字符和命令git commit输出的相同。（见以上两段git输出）

*什么是SHA-1哈希码？Git为什么要用它？*

*在集中式版本控制系统中，一台特定的服务器会为每个提交分配一个数字（通常是不断增长的自然数），用这些数字区分不同的提交。对于分布式版本控制系统，这个方法就行不通了。*

*当两个人工作在同一套代码上，分别往各自的本地版本库提交时，相同的提交号对应着不同的修改。这样的提交号就不能再唯一标识一个提交了。Git用SHA-1解决了这个问题。*

*SHA的意思是安全哈希算法。他由美国国家安全局（NSA）开发，用来从已知的数据中产生一个短的字符串，这个字符串几乎不可能等同于另外一组数据产生的字符串。*

*Git使用版本库中的元数据产生提交名称。这些元数据包括提交者的个人信息及提交的时间戳。这些数据产生的哈希码，使不同的提交几乎不可能具有相同的提交名称。*

*当然，产生相同哈希码的可能性是存在的，但是这种可能性非常低，可以忽略。从数学角度来讲，这种可能性是2的63次方分之一。*

*40位的哈希码很难记，大多数情况下，7位的哈希码就可以基本保证提交名称具有的唯一性。*

#### 第三节 在项目中工作

下面把index.html文件内容修改为：

```
<html>
        <head>
                <title>hello world in Git</title>
        </head>
        <body>
                <h1>hello world!</h1>
        </body>
</html>
```

修改完毕，用**git status**命令显示工作目录树的状态。

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   index.html

no changes added to commit (use "git add" and/or "git commit -a")
```

Git中有三个地方可以存放代码。

第一个地方是工作目录树，编辑文件时可以直接在这里操作。

第二个是索引，也就是暂存区。暂存区是工作目录树和版本库之间的缓冲区。

第三个，版本库。

**git add**命令可以暂存对index.html刚刚做的修改。跟 前面章节中添加一个新文件时使用的是同一个命令，只不过这次他告诉Git要追踪的是一个新的修改而非新的文件。

```
$ git add index.html
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   index.html
```

暂存修改过的index.html后，执行命令git status可以看到，输出信息中的标题从Change but not updated变成了Change to be committed。如果打开颜色开关，index.html这一行会由红色变为绿色。

```
Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (master)
$ git commit -m "add head and title to index" \
> -m "This allows for a more semantic document"
[master 041a928] add head and title to index
 1 file changed, 3 insertions(+)

Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (master)
$ git log -l
fatal: Option 'l' requires a value

Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (master)
$ git log -1
commit 041a928b39d8a8a24ddf0528320547ca961c2006 (HEAD -> master)
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Thu Nov 8 15:11:36 2018 +0800

    add head and title to index

    This allows for a more semantic document

Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (master)
$ git log -2
commit 041a928b39d8a8a24ddf0528320547ca961c2006 (HEAD -> master)
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Thu Nov 8 15:11:36 2018 +0800

    add head and title to index

    This allows for a more semantic document

commit 6e71f4eb68c0b94f65bf3ff9c4fbeafec3bfe9b1
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Thu Nov 8 14:16:21 2018 +0800

    add in hello world html
```

git commit 命令不要忘记使用-m参数。在参数后边提交留言。看见上边使用了两个-m参数。Git可以接受输入任意多次提交留言的输入，每次另起一段。如果一行命令输入不完，可用\ 回车来继续输入命令。

**git log -1** / git log -2，git log命令加数量参数，可控制显示最近的多少条日志。

#### 第四节 理解并使用分支

有两种分支比较有用：第一种，用来支持项目的不同发布版本的分支；第二种，用来支持一个特定功能的开发的分支。下面介绍第一种：

譬如mysite项目几乎可以发布了，但还需进行测试等工作，直到确认她达到了预期的功能和质量。与此同时，借助分支，可以开始下一个版本的新功能的开发了。

分支可以为要发布的代码保留一份拷贝，所以无需停止正在开发的工作。

创建分支的命令 **git branch**需要两个参数，新分支名称和父分支名称。新创建的分支基于已经存在的父分支。

```
$ git branch RB_1.0 master
```

RB标识release branch。

之后把index.html文件内容修改为：

```
<html>
        <head>
                <title>hello world in Git</title>
        </head>
        <body>
                <li><a href="bio.html">Biography</a></li>
                <h1>hello world!</h1>
        </body>
</html>
```

用如下命令提交这些修改： **git commit -a**

-a参数告诉git提交全部修改过的文件。

但是也要输入提交留言。

现在主分支上有最新的修改，而发布分支上还是原来的代码。用**git checkout**命令切换到发布分支。

```
Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (master)
$ git checkout RB_1.0
Switched to branch 'RB_1.0'

Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (RB_1.0)
$ cat index.html
<html>
        <head>
                <title>hello world in Git</title>
        </head>
        <body>
                <h1>hello world!</h1>
        </body>
</html>
```

可以看到切到发布分支了，并且该分支上index.html文件的内容仍是原来的内容。

这时候在发布分支上修改内容为：

```
$ cat index.html
<html>
        <head>
                <title>hello world in Git</title>
                <meta name = "description" content="hello world in Git"/>
        </head>
        <body>
                <h1>hello world!</h1>
        </body>
</html>
$ git commit -m "can push" -a
```

顺便说一句，在分支上查看log，也是可以看到从第一期开始的log的。

```
$ git log
commit 2f4d16d7370dd81d4a5da7baad4e83a774da11bc (HEAD -> RB_1.0)
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Thu Nov 8 15:37:29 2018 +0800

    can push

commit 041a928b39d8a8a24ddf0528320547ca961c2006
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Thu Nov 8 15:11:36 2018 +0800

    add head and title to index

    This allows for a more semantic document

commit 6e71f4eb68c0b94f65bf3ff9c4fbeafec3bfe9b1
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Thu Nov 8 14:16:21 2018 +0800

    add in hello world html
```

#### 第五节 处理发布

1.0版本可以发布了，用Git中的代码打标签。意味着在版本库历史中标记处特定的点（用人类可以理解的名称而非SHA号）。这样将来就容易找到对应版本的代码。

```
Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (RB_1.0)
$ git tag 1.0 RB_1.0

Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (RB_1.0)
$ git tag
1.0
```

git tag 标签名称 希望打标签的点。

git tag不加参数，可以查看版本库中的标签列表。此时只有1.0

至此，主分支和发布分支都有各自不同的提交。需要把RB_1.0的修改合并到主分支上来。

变基命令git rebase 可以完成这项工作。变基是把一条分支上的修改在另一条分支的末梢重现。*（变基的意思是“改变分支的基底”。不同的版本控制工具，其实现方法不尽相同。假定有分支A和分支B，他们的交叉点是版本Y。Git的实现方式是，如果站在分支B上告诉Git，“我要变基到A的末梢”，那么Git会把从版本Y到分支B的当前末梢之间的所有提交，顺序加到分支A的末梢上去，生成新的分支B及其末梢，而分支A及其末梢则没有任何变化。）*

首先切回主分支 git checkout master

接着运行命令 git rebase 后边跟参数，希望变基到哪条分支的末梢，就使用哪条分支名称做参数。

```
Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (RB_1.0)
$ git checkout master
Switched to branch 'master'

Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (master)
$ git rebase RB_1.0
First, rewinding head to replay your work on top of it...
Applying: add url
Using index info to reconstruct a base tree...
M       index.html
Falling back to patching base and 3-way merge...
Auto-merging index.html

Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (master)
$ cat index.html
<html>
        <head>
                <title>hello world in Git</title>
                <meta name = "description" content="hello world in Git"/>
        </head>
        <body>
                <li><a href="bio.html">Biography</a></li>
                <h1>hello world!</h1>
        </body>
</html>

Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (master)
$ git log
commit 665439cb7a91fa7395f771bc387ecd2b49a17c33 (HEAD -> master)
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Thu Nov 8 15:32:38 2018 +0800

    add url

commit 2f4d16d7370dd81d4a5da7baad4e83a774da11bc (tag: 1.0, RB_1.0)
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Thu Nov 8 15:37:29 2018 +0800

    can push

commit 041a928b39d8a8a24ddf0528320547ca961c2006
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Thu Nov 8 15:11:36 2018 +0800

    add head and title to index

    This allows for a more semantic document

commit 6e71f4eb68c0b94f65bf3ff9c4fbeafec3bfe9b1
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Thu Nov 8 14:16:21 2018 +0800

    add in hello world html
```

此时可以看到，add url结点放入RB_1.0分支最后一个提交的后面。

最后，作为整理工作的一部分，删除发布分支。这样看上去好像很危险，不过不用担心，刚才打过的标签所指向的结点就是RB_1.0分支的末梢。（只要标签还在，从标签到版本树起点的一连串提交记录就在。）这时删除分支只是删除了分支的名字，并不会删除分支上的任何实际内容。

使用命令**git branch -d** 加分支名字可以删除分支。

```
Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (master)
$ git branch -d RB_1.0
Deleted branch RB_1.0 (was 2f4d16d).
```

此时如果想给1.0.x分支打补丁，只需要从打标签的地方再创建一条分支即可。之前的创建分支命令最后一个参数是父分支的名称。现在只需把父分支名称改成发布标签名称即可。命令如下：

```
Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (master)
$ git branch RB_1.0.1 1.0

Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (master)
$ git checkout RB_1.0.1
Switched to branch 'RB_1.0.1'

Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (RB_1.0.1)
$ git log
commit 2f4d16d7370dd81d4a5da7baad4e83a774da11bc (HEAD -> RB_1.0.1, tag: 1.0)
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Thu Nov 8 15:37:29 2018 +0800

    can push

commit 041a928b39d8a8a24ddf0528320547ca961c2006
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Thu Nov 8 15:11:36 2018 +0800

    add head and title to index

    This allows for a more semantic document

commit 6e71f4eb68c0b94f65bf3ff9c4fbeafec3bfe9b1
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Thu Nov 8 14:16:21 2018 +0800

    add in hello world html
```

git log快速查看历史记录，可以看到在RB_1.0分支上只有3个提交（而不包括主分支上的最新提交）。

```
$ git log --pretty=oneline
2f4d16d7370dd81d4a5da7baad4e83a774da11bc (HEAD -> RB_1.0.1, tag: 1.0) can push
041a928b39d8a8a24ddf0528320547ca961c2006 add head and title to index
6e71f4eb68c0b94f65bf3ff9c4fbeafec3bfe9b1 add in hello world html
```

使用Git做的最后一件事儿是，为代码发布创建归档文件。没必要总是把历史记录（也就是Git版本库）一起发布，通常情况下，将标签对应的版本内容打包成一个tar包或zip包就足够了。可使用git archive命令来做归档处理。

```
Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (RB_1.0.1)
$ git archive --format=tar\
> --prefix=mysite-1.0/ 1.0\
> |gzip > mysite-1.0.tar.gz
fatal: Unknown archive format 'tar--prefix=mysite-1.0/'
```

该命令中有三个参数。第一个，--format指明要产生tar格式的输出；第二个，--prefix指明包中所有的东西都放到mysite-1.0/目录下；最后，1.0指明需要归档的标签的名称。

第三行看起来有点复杂，该命令把git archive产生的tar文件用管道输出的方法传递给命令gzip进行压缩，而压缩结果则重定向到mysite-1.0.tar.gz压缩包里。

命令git archive也可以产生zip文件。创建zip文件更简单，因为git archive的输出是已经按zip标准压缩过的。下面这条命令可以生成一个zip文件。

git archive --format=zip\

​		   --prefix=mysite-1.0/ 1.0\

​		   > mysite-1.0.zip

#### 第六节 克隆远程版本库

之前所有的讲述都是与本地版本库mysite打交道的命令，但还没涉及远程版本库。Git也可以与远程版本库打交道，以分享本地的工作成果，或者复制其他的版本库到本地。

与远程版本库打交道，首先要用git clone命令克隆远程版本库。克隆版本库会在本地创建远程版本库的完整拷贝。

git clone 有两个参数，第一个，远程版本库的位置，第二个，存放该版本库的本地目录。第二个参数是可选的，但是在本书的例子中已经有了mysite目录，所以必须要提供第二个参数指定远程版本库拷贝的目标目录。

```
git clone git://github.com/tswicegoods/mysite.git mysite-remote
```

第二个参数相当于给仓库重命名了。

第三章完。2018.18:34 