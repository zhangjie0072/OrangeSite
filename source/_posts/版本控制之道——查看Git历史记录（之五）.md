---
title : 版本控制之道——查看Git历史记录（之五）
---

#### 第一节 查看Git日志

**git log**命令可以查看日志。如果日志超出单屏大小，Git则显示部分输出结果。你可以上下滚动浏览日志。屏幕底部出现的冒号"："标识还有更多信息等待输出。

*此时，键入j向下浏览，键入k向上浏览，键入q退出。和vi编辑器命令一样。*

提交名称、提交人、提交日期、提交留言

```
Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/OrangeSite (blog-src)
$ git log
commit d55e51fd1fe1a310d2cb61866e0322882b0a846f (HEAD -> blog-src, origin/blog-src)
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Tue Nov 13 15:59:32 2018 +0800

    hexo g

commit c2a6392edfe4431d6f61a0e3e6411a87a4f90656
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Tue Nov 13 15:54:16 2018 +0800

    add new blog

commit 62c958c12405a7d4981ced107c28ed489ac7bb3e
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Mon Nov 12 14:54:58 2018 +0800

    add new post
```

使用git log -p可以查看各版本库之间的代码差异

```

commit 5ab37bb6eba8aa1c4f9691fc2829a9d774748ad6 (contacts)
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Tue Nov 13 15:09:32 2018 +0800

    add info 5

diff --git a/about.html b/about.html
index 8f50339..8e5b283 100644
--- a/about.html
+++ b/about.html
@@ -1,2 +1,3 @@
 I'm about.html
 info 2
+info 5

commit 71309091d790f57998e44281c68b74087ec502c3
Author: zhangjie0072 <zhangjie0072@126.com>
Date:   Tue Nov 13 15:08:49 2018 +0800

    add info 3

diff --git a/about.html b/about.html
index 8f50339..a01806a 100644
--- a/about.html
+++ b/about.html
@@ -1,2 +1,4 @@
 I'm about.html
 info 2
+info 3
+info 4
```

git log命令后输入参数-1， -2是只显示一条提交的日志或只显示两条提交的日志。也可以给git log命令传递一个指定的版本，并以此作为查看日志的起始点（查看从那时起到现在的全部提交）。名称只需要至少输入4位，Git会设法匹配，如果匹配不上或者匹配到了多个结果，Git就会报错了。

#### 第二节 指定查找范围

1. 查看最近5小时内的提交 **git log --since="5 hours"**

   以下分别为查看一个小时和2个小时的。

   ```
   Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (new02)
   $ git log --since="1 hours"
   commit 9f6b0246c3c2fd1930c78bd821a8875873548c53 (HEAD -> new02)
   Author: zhangjie0072 <zhangjie0072@126.com>
   Date:   Tue Nov 13 15:50:06 2018 +0800
   
       add new01.txt
   
   Orange@DESKTOP-8VK4FUU MINGW64 /d/MyGit/mysite (new02)
   $ git log --since="2 hours"
   commit 9f6b0246c3c2fd1930c78bd821a8875873548c53 (HEAD -> new02)
   Author: zhangjie0072 <zhangjie0072@126.com>
   Date:   Tue Nov 13 15:50:06 2018 +0800
   
       add new01.txt
   
   commit 89f9869907d3f986e601e0339e7427510580cfd9 (about)
   Merge: 7130909 5ab37bb
   Author: zhangjie0072 <zhangjie0072@126.com>
   Date:   Tue Nov 13 15:29:25 2018 +0800
   
       after merge
   
   commit 5ab37bb6eba8aa1c4f9691fc2829a9d774748ad6 (contacts)
   Author: zhangjie0072 <zhangjie0072@126.com>
   Date:   Tue Nov 13 15:09:32 2018 +0800
   
       add info 5
   ```
