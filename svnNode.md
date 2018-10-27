# SVN

### svn常用命令

#### 管理员命令

` svnadmin help` 查看子命令

`svnadmin help create ` 查看子命令用法

```s&#39;v
$ svnadmin create ./project/test

(project是顶级仓库，test是根仓库)
svn仓库分为两级：顶级仓库和根仓库
该命令用于创建版本库（创建的是根仓库）
注意：在创建根仓库时，顶层仓库目录必须时存在的，其不会自动创建，根仓库目录test是否存在都可以，如果没有则自动创建，如果test有则自动默认成仓库

-test
	--conf  // 配置
		---authz  // 权限
		---hooks-env.tmpl // 钩子（了解即可） 
		---password  // 连接密码
		---svnserver.conf // 连接仓库
	--db   // 存放日志（修改、提交）
		---
	--hooks   // 钩子
	--locks
	--format
```



#### 服务端命令

` $ svnserve -d -help` 

查看此命令的用法

 `$ svnserve -d` 

用于开启DOS系统下的svn服务，为SVN服务运行独立的端口，让客户端访问仓库

**svn默认端口号：3690 ； 可以用netstat -a 命令查看当前网络的连接状态**



`$ svnserve -d --listen-port 8888` 

修改svn开启的端口号

访问这个仓库要

svn://localhost:3690/e:/svn/......

svn://localhost:3690/仓库地址



`$ svnserve -d -r svn的顶层仓库路径`  

一旦指定，那么客户端在使用svn时直接给出根仓库名即可

访问这个仓库要

svn://localhost/test     就不用在去写全部路径

**开机启动svn服务器**

`$ sc create SVNServer binpath=svnserve.exe的路径  --service -r 顶层仓库的路径start=auto depend=Tcpip `  设置开机自启动,就不用再去用`$ svnserve -d` 命令每次去启动了

`$ net stop SVNSrever`  停止服务

`$ sc delete SVNServer` 删除服务，最好先停止再去删除



#### 客户端命令

**在工作中会直接给你svn://.......   直接去到你想把这个仓库连接的文件夹下checkout**

`$ svn checkout` 

 检出：创建客户端指定目录与服务端指定根仓库间的连接关系

一个客户端一般情况下，一般只检出一次



##### 基于顶层仓库的checkout

`$ svnserve -d` 先去开启svn服务器然后再去访问

`$ svn checket svn://localhost/要检出的文件夹 要检出到文件夹的路径`

例：

```
$ svnserve -d -r 顶层文件夹路径
$ svn checkout svn://localhost/顶层文件夹下的仓库的名称 要检出到文件夹的路径    
// 因为在顶层文件夹开启的服务所以只写顶层文件夹下的名称就好

$ svn checkout svn://localhost/顶层文件夹下的仓库的名称
// 如果不去指定检出到哪个目录就好直接copy顶层仓库中所有的子仓库

```



##### 基于根仓库的checkout

```
// 直接去找到根仓库连接
$ svnserve -d -r ./
// 直接到要检出到的仓库执行此命令
$ svn checkout svn://localhost 

```

#### 服务端修改客户端权限

**需要修改的文件夹在你创建的仓库中的 conf/svnserve.conf中**

```
# anon-access = read  没有被认证的，打开这个只能读
# anon-access = write 没有被认证的，打开这个只能写
```



#### svn add

当一个文件/目录，被放到你克隆下来的项目中后，svn不会知道他的存在，即svn并不会对其进行管理，若要时svn对其进行管理，必须将其通知add命令，添加到svn管理中，需要注意，被add的文件/目录，必须将克隆项目中

```
在克隆下来的仓库中添加readme.txt文件
$ svn add readme.txt
再次修改这个readme.txt
$ svn add readme.txt 
svn: warning: W150002: 'E:\svn\group\aacof\readme.txt' is already under version control
svn: E200009: Could not add all targets because some targets are already versioned
svn: E200009: Illegal target for the requested operation
提示你已被add，所以这个与git不同，值需要add一次然后可以随便修改，修改文件名除外


在克隆下来的仓库中添加一个文件aaa，在文件中添加一个文本，它会连文件中的文本一起提交
$ svn add aaa 

```

#### svn commit

**commit 命令用于将客户端克隆下来的仓库中所有的文件和目录提交到服务端**

```
$ svn commit readme.txt -m aa
直接提交到仓库中
$ svn commit -m 日志名称
提交全部
```



#### svn update

**update将当前客户端中的克隆仓库更新到与svn服务端相同**

```
$ svn update readme.txt(指定文件名) 
只更新指定的文件
$ svn update
更新所有文件
```



#### svn delete

```
$ svn delete 文件名称
删除指定的文件，只是删除客户端自己的，无法删除客户端的，和手动删除一个效果，但是手动删除无法同步到服务端
```























