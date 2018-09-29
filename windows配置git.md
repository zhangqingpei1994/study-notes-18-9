## 在Windows下安装git并用github同步

### 1. 在官网上下载git 

 git下载地址：https://www.git-scm.com/download/，自己当时下载的时候下不下来，是境伟大佬帮忙翻墙下的。

### 2. 配置公钥

安装git没有什么东西可讲的，只要一直下一步就行，当安装完git之后右键就会出现 Git Bash Here 和Git Gui Here 的选项，当安装完事之后在某个路径下，最好全部是英文路径下右键->Git Bash Here ,然后输入以下命令：

​     **ssh-keygen -t rsa -C "邮箱"** 

按三次回车就生成了git的公钥，如下图所示是网上的例子：

![](https://github.com/zhangqingpei1994/picture/blob/master/study-notes-new-start/5.png)

自己的是在C:\Users\zhang\ .ssh这个路径下的id_rsa.pub，打开复制粘贴到github中，在github下面的settings中的ssh选项中，如下图所示：

![](https://github.com/zhangqingpei1994/picture/blob/master/study-notes-new-start/2.png)

点击New SSH key,然后将复制的公钥粘贴到这里，添加公钥成功。

### 3.设置自己的名字和邮箱

因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。在命令行中输入以下命令并填入相关信息：

```c++
 git config --global user.name "Your Name"

 git config --global user.email "email@example.com"
```

注意：这些命令行都是在git  bash  here执行的，配置的用户名和邮箱都是本地的

### 4.在本地创建版本库

1）找到一个适当的地方，点击右键Git Bash Here进入命令行输入以下命令创建一个文件夹：

```
$ mkdir study-notes-new-start
$ cd study-notes-new-start
```

我是在F:\github文件夹下执行的Git Bash Here，github就是一个普通的文件夹

![](https://github.com/zhangqingpei1994/picture/blob/master/study-notes-new-start/3.png)

如图所示是文件布局结构

2）通过git init命令把此目录变为可以Git管理的仓库，经过这个操作，当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的，一般情况下不要修改以防破坏Git仓库。

```
$ cd study-notes-new-start
$ git init
```

经过上面两条命令就会在当前目录下生成一个.git的文件夹

![](https://github.com/zhangqingpei1994/picture/blob/master/study-notes-new-start/1.png)

### 5.添加文件到版本库

1)  新建一个README.md，里面内容自己写

2）执行以下命令把文件添加到仓库

````
$ git add README.md
````

3）执行以下命令把文件提交到仓库，引号内为提交说明

```
$ git commit -m "test"
```

### 6.关联github

此时你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作。

- **github创建仓库**

注册并登录GitHub，按照以下三步操作即可创建一个仓库。第一步为创建一个新的仓库，第二步为仓库起一个名字，第三步确认创建。

![](https://github.com/zhangqingpei1994/picture/blob/master/study-notes-new-start/4.png)

这个图片是网上的别人博客里面的，名字最好和自己刚刚创建的本地仓库一样，不知道不一样会不会有影响。

- **关联github仓库**

  git bash 执行以下命令：

  ```
  git remote add origin git@github.com:zhangqingpei1994/study-notes-new-start.git
  ```

- 传文件到github

  按照前面的提示，我们已经安装好了本地的仓库以及远程的仓库，并且进行了简单的配置，这时候我们可以尝试提交文件了。输入push命令：

  ```
  $ git push -u origin master
  ```

### 7. 保存github的用户名和密码

这个在windows上git好像自动保存了。

参考网址：https://blog.csdn.net/sssssuuuuu666/article/details/78565381




