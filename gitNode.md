# 。创建版本库

## 创建目录

```
$ makdir (learngit)    // learngit 是文件名，可以自定义
$ cd learngit 	 	  // 切换到Learngit目录下面
$ pwd  				 // 查看当前文件目录下
```

## 初始化创建好的仓库

```
$ git init 			// 初始化  
Initialized empty Git repository in /Users/michael/learngit/.git/    // 路径可以自定义
					// Initialized empty Git repository 意思是初始化git储存库
					
ls -ah  // 查看隐藏文件 .git就是一个版本库
```

## 把文件添加到仓库

1. 现在仓库创建一个文件（readme.txt） // 名称自定义
2. 在文件里边添加内容   // 一开始自定义添加

```
$ git add readme.txt  		// 告诉git把文件添加到仓库 
// 执行上边的命令后不会出现任何反应
$ git commit  				// 告诉git把文件提交到仓库
$ git commit -m 'wrote a readme file'
						   // 执行上边命令后出现如下命令
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
 							// 1 file changed 表示一个文件改动
 						  // 2 insertions(+) 表示插入了两行数据
```

> git commit 和 git add扩展

```
$ git add file1.txt
$ git add file2.txt file3.txt // 可以一次提交多个文件
$ git commit -m "add 3 files" // add 3 files 只是这次提交代码的名字
```



# [时光穿梭机](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013743858312764dca7ad6d0754f76aa562e3789478044000)

> 修改文件后如果还没有添加到仓库可以用以下命令查看结果

```
$ git status
On branch master
Changes not staged for commit: // 还没有提交更改
  (use "git add <file>..." to update what will be committed) // 提交更改
  (use "git checkout -- <file>..." to discard changes in working directory) // 撤回修改

    modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```



> 查看修改内容（与上一次的对比）添加一个单词  distributed

```
$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.    			// 更改前
+Git is a distributed version control system.	 // 更改后
 Git is free software.
 // 然后再用 git add 添加
```



## [版本回退](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013744142037508cf42e51debf49668810645e02887691000)

> 查看更改的日志

```
$ git log 					// 查看提交日志
$ git log --pretty==oneline	 //	比 git log 查看的日志要清晰 commit的一串代码为版本号
```



> 回退

```
$ git reset --hard HEAD^     // 回退到上一个版本，HEAD^^ 上上个版本，依次 ，如果需要回退很多的话，可以 HEAD~100 ，回退到前100个版本   HEAD就是一个指针，可以用它来指向以前的命令
```



> 穿梭

```
$ git reset --hard 1223 (1223为版本号，虽然很长但是只写前几位就可以)
```



> 查看内容

```
$ cat readme.txt
```



> 查看命令的记录

```
$ git reflog  // 也可有命令版本号  如果要恢复到上一次的内容，也要用上边的命令
```





## [工作区和暂存库](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013745374151782eb658c5a5ca454eaa451661275886c6000)

### 工作区

> 就是你在电脑里能看到的目录，比如我的`learngit`文件夹就是一个工作区

### 版本库

> 工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。 
>
> 
>
> Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。 ![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

> 
>
> 前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的 
>
> 第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；
>
> 
>
> 第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。
>
> `master` 是git自动创建的，` git commit `就是在`master`上提交更改
>
> 
>
> 例如现在在仓库中更改一个文件，在添加一个LICENSE文本文件

```
$ git status  // 查看状态
On branch master
Changes not staged for commit:   // 没有提交更改
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory) // git checkout file  撤回此操作

    modified:   readme.txt

Untracked files:  // 还没有添加的文件
  (use "git add <file>..." to include in what will be committed)

    LICENSE

no changes added to commit (use "git add" and/or "git commit -a")
```



> 使用两次命令添加文件这个两个文件` $ git add file_name ` ，然后再用 ` $ git status `

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   LICENSE
    modified:   readme.txt
```



> 暂存区的内容图示![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907720458e56751df1c474485b697575073c40ae9000/0)



> 然后一旦用命令` $ git commit -m "file" ` 一次性提交后

```
$ git commit -m "understand how stage works"
[master e43a48b] understand how stage works
 2 files changed, 2 insertions(+)
 create mode 100644 LICENSE
```



> 一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的： 

```
$ git status
On branch master
nothing to commit, working tree clean
```



> 现在的工作区如图![](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849077337835a877df2d26742b88dd7f56a6ace3ecf000/0)

## [管理修改](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374829472990293f16b45df14f35b94b3e8a026220c5000)

> 现在，假定你已经完全掌握了暂存区的概念。下面，我们要讨论的就是，为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。
>
> 你会问，什么是修改？比如你新增了一行，这就是一个修改，删除了一行，也是一个修改，更改了某些字符，也是一个修改，删了一些又加了一些，也是一个修改，甚至创建一个新文件，也算一个修改。
>
> 为什么说Git管理的是修改，而不是文件呢？
>
> 第一步，对readme.txt做一个修改，比如加一行内容： 

```
$ cat readme.txt  // 查看内容
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
```



> 然后添加并查看状态

```
$ git add readme.txt
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   readme.txt
#
```



> 然后，在修改readme.txt

```
$ cat readme.txt 
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
```



> 提交

```
$ git commit -m "git tracks changes"
[master 519219b] git tracks changes
 1 file changed, 1 insertion(+)
```



> 提交后在查看状态

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

> 再然后你会发现第二次更改没有提交`git diff HEAD -- readme.txt` 查看		 
>
> 
>
> 工作区和版本库中文件内容的区别

```
$ git diff HEAD -- readme.txt
diff --git a/readme.txt b/readme.txt
index 76d770f..a9c5755 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,4 +1,4 @@
 Git is a distributed version control system.
 Git is free software distributed under the GPL.
 Git has a mutable index called stage.
-Git tracks changes.
+Git tracks changes of files.
```

> 可见，第二次修改确实没有被提交。
>
> 
>
> 那怎么提交第二次修改呢？你可以继续`git add`再`git commit`，也可以别着急提交第一次修改，先`git add`第二次修改，再`git commit`，就相当于把两次修改合并后一块提交了
>
> 
>
> 第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit` 
>
> # [撤销修改](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374831943254ee90db11b13d4ba9a73b9047f4fb968d000)
>
> ###还没有添加到暂存区之前
>
> 如果你的文件内容改错了并且及时发现，这是你用` $ git status `查看状态，它会提示你 `  git checkout -- file `这个命令就是撤销的命令

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)  // 

    modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

```
$ git checkout -- file_name 
```

> 命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：
>
> 一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
>
> 一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
>
> 总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。
>
> `git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。 
>
> ###放到暂存区之后
>
> 现在已经用` $ git add readme.txt `把文件提交到了暂存区，但是你还没有提交，这是你发现了问题

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    modified:   readme.txt
```

> 你先查看状态 Git会告诉我们，用命令` git reset HEAD <file>  ` 可以把暂存区的修改撤掉重新放回工作区

```
$ git reset HEAD readme.txt
Unstaged changes after reset:
M    readme.txt
```



> `git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。
>
> 再用`git status`查看一下，现在暂存区是干净的，工作区有修改：

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   readme.txt
```

> 然后在丢弃工作区的修改

```
$ git checkout -- readme.txt

$ git status
On branch master
nothing to commit, working tree clean
```

### 小结

> 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。
>
> 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。
>
> 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013744142037508cf42e51debf49668810645e02887691000)一节，不过前提是没有推送到远程库。 

# [删除文件](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013758392816224cafd33c44b4451887cc941e6716805c000#0)

> 删除文件命令` rm file_name `
>
> 这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了： 

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

> 现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`： 

```
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

> 另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本： 

```
$ git checkout -- test.txt
```

###小结

> 命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。 

# [远程仓库](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374385852170d9c7adf13c30429b9660d0eb689dd43a000)

## 创建目录  

*  ` $ ssh-keygen -t rsa -C "youremail@example.com"`

  ![GitHub添加仓库步骤](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908342205cc1234dfe1b541ff88b90b44b30360da000/0)

## [添加远程仓库](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013752340242354807e192f02a44359908df8a5643103a000)

* 步骤   在Repository name填入`learngit`，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库： 

  ![](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849084639042e9b7d8d927140dba47c13e76fe5f0d6000/0)

  ![](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849084720379a3eae576b9f417da2add578c8612a2e000/0)

* 连接本地仓库，必须切换到本地的learngit仓库里面

  ` $ git remote add origin git@github.com:你的Github名称/learngit.git`

  添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。 

* 然后把仓库内容添加到仓库`git push -u origin master`   -u 是因为第一次传的时候仓库是空的，只有第一次添加时用

* 推送内容命令 `$ git push origin master` 

## [远程仓库克隆](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375233990231ac8cf32ef1b24887a5209f83e01cb94b000) 

* 步骤

![](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849085474010fec165e9c7449eea4417512c2b64bc9000/0)



* 克隆GitHub仓库到本地 `git clone git@github.com:你的GitHub名称/gitskills` 
* 然后你的本地就会有一个gitskills仓库

# [分支管理](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013743862006503a1c5bf5a783434581661a3cc2084efa000)

## [创建与合并分支](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840038939c291467cc7c747b1810aab2fb8863508000)

在[版本回退](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013744142037508cf42e51debf49668810645e02887691000)里，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即`master`分支。`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

![](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849087937492135fbf4bbd24dfcbc18349a8a59d36d000/0)

 每次提交，`master`分支都会向前移动一步，这样，随着你不断提交，`master`分支的线也越来越长： 

* 创建了新的分支 ` $ git checkout -b branch_name` （英语小知识：branch----分支）

* 以上命令相当于两个命令

  ```
  $ git branch dev
  $ git checkout dev // 切换分支
  ```

* 当创建了这个分支以后

  ![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908811773187a597e2d844eefb11f5cf5d56135ca000/0)

* 然后后面的提交都是用这个分支,比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变 

  ![](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849088235627813efe7649b4f008900e5365bb72323000/0)

* 然后把dev合并到master上 `$ git merge dev` 

  ![](https://cdn.liaoxuefeng.com/cdn/files/attachments/00138490883510324231a837e5d4aee844d3e4692ba50f5000/0)

* 合并后甚至可以删除dev分支，仓库的内容都不会改变

  ![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908867187c83ca970bf0f46efa19badad99c40235000/0)

* 查看当前的分支`$ git branch`

* 用dev的时候更改仓库里面文件，然后提交

  ```
  $ git add file_name
  $ git commit -m "这次命令名称"
  ```

* 然后切换到master ,查看文件内容

  ```
  $ git checkout master
  $ cat file_name
  ```

* 合并分支 `$ git merge`

* 删除分支`$ git branch -d dev`

## [解决冲突](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840202368c74be33fbd884e71b570f2cc3c0d1dcf000)

