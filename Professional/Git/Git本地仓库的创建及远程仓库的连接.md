# Git本地仓库的创建及远程仓库的连接

来源：https://www.liaoxuefeng.com/wiki/896043488029600

### 1、创建本地仓库

（1）创建文件夹

（2）进入文件夹运行 git bash，运行命令：**git init**

### 2、添加GitHub远程仓库

（1）登陆GitHub并创建新的仓库(repository)   --->   命名

（2）在本地仓库运行命令：**git remote add origin https://github.com/wBekvam/test.git**（根据提示）

### 3、本地库的内容推送到远程

命令：**git push -u origin master**

​			远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。

​			由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

