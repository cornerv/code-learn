## 说明
在一台服务器上建立一个源代码库，库里可以存放许多不同项目的源程序，由源代码库管理员统一管理这些源程序。每个用户在使用源代码库之前，首先要把源代码库里的项目文件下载到本地(Checkout)，然后用户可以在本地任意修改，最后用svn命令进行提交(Commit)，由svn源代码库统一管理修改；
## linux命令行
gitbash安装后可以window本地使用svn
### 1.创建SVN仓库
如果没有SVN仓库，可以新建一个自己的仓库；
~~~shell
svnadmin create SVN_learn
~~~
此时文件夹里会生成以下目录结构：
![](Pasted%20image%2020230418182326.png)

![](Pasted%20image%2020230418183117.png)
### 2.启动仓库
~~~shell
#重新开个bash界面，这个界面用来启动SVN，会卡在enter界面
svnserve -d -r SVN_learn
~~~
![](Pasted%20image%2020230418183009.png)
### 3.将仓库同步到本地文件夹
~~~shell
#重点：checkout(检出)、commit(提交)、update(更新)
#在G盘下建立user1、user2两个目录，模拟两个协同工作的用户的workspace。
#检出checkout(co)：第一次和SVN服务器交互时，需要使用checkout将仓库检出到本地。
#说明：checkout一次，就建立了与SVN仓库的连接。
#checkout=co
mkdir user1 user2
svn checkout file:///G:/SVN_learn ./user1
svn checkout file:///G:/SVN_learn ./user2
#-r 指定版本号，服务器上的SVN需要输入密码
svn co svn://10.8.238.100/flow/trunk/plain210616 -r r470 ./
~~~
![](Pasted%20image%2020230418184722.png)
### 4.提交commit文件
在user1目录下新建Demo1.java文件，将该文件提交到SVN仓库。下图演示了三种典型的错误提交。
![](Pasted%20image%2020230418184813.png)
~~~shell
#1.新文件提交之前需要add
svn add Demo1.java
#2.commit提交的时候需要写注释
svn commit -m "init Demo1.java" Demo1.java
~~~
![](Pasted%20image%2020230418185329.png)

### 5.更新Update文件
切换到user2的工作空间(user2目录下)，user2第一次使用SVN仓库，需要检出。user2修改Demo1.java后提交。切换到user1目录，更新(update)。可以理解为从仓库下载到本地；
~~~shell
#update=up
svn update
svn up
~~~
![](Pasted%20image%2020230418190029.png)
### 6.删除与恢复delete和revert(慎用)
如果delete后，提交到服务器(commit)，则服务器上的数据也被删除了(慎用)；
delete未提交之前，可以用revert恢复文件；
~~~shell
svn delete Demo1.java #删除后当前路径下该文件也会消失
svn revert Demo.java
~~~
![](Pasted%20image%2020230418190350.png)

### 7.冲突解决
假设user1和user2目录下的Demo1.java都更新到了最新版本。user1修改Demo1.java，提交至仓库。若user2也修改Demo1.java并提交，此时user2的TortoiseSVN会报提交版本过时的错误，并提醒user2需要更新，更新时就会发生冲突。如下图：
![](Pasted%20image%2020230418193540.png)

解决方法：不确定
~~~shell
1. svn update
-   .mine是我的修改，尚未update前的 Demo1.java。
-   .r4 是别人提交前的版本，尚未导致冲突的版本。
-   .r5 是别人提交后的版本，导致冲突的版本。
-   Demo1.java 包含了我和现有版本的冲突内容
2. 直接修改Demo1.java,
3. svn resolve --accept working Demo1.java=
4. 重新提交commit
~~~


![](Pasted%20image%2020230418193722.png)
## Windows下：TortoiseSVN使用
### 1.创建目录：
创建目录SVN_test,进入该目录下，右击 — TortoiseSVN — Create repository here，并创建默认的SVN目录结构，如下图所示：
![](Pasted%20image%2020230418191930.png)

![](Pasted%20image%2020230418190810.png)
![](Pasted%20image%2020230418190937.png)
## 检出：checkout
在G盘下建立user3、user4两个目录，模拟两个协同工作的用户的workspace。
进入user3目录下，右击 — SVN Checkout，在URL of repository中输入：
file:///G:/SVN_test。【此时仓库还没有启动SVN服务，所以使用file://】

![](Pasted%20image%2020230418191154.png)![](Pasted%20image%2020230418191217.png)
![](Pasted%20image%2020230418191332.png)


![](Pasted%20image%2020230418191915.png)
### 3.提交：commit

在user3/trunk目录下新建Demo1.java，在该文件上右击 — TortoiseSVN — add，则将Demo1.java纳入版本控制。然后右击 — SVN Commit，提交至代码仓库。
![](Pasted%20image%2020230418192013.png)
![](Pasted%20image%2020230418192043.png)

### 4.更新：update

对user4进行上面的检出操作。并修改user4目录下的Demo1.java(如增加一个字段)，并commit。

回到user3/trunk，右击 — SVN Update。
![](Pasted%20image%2020230418192236.png)

### 5.冲突解决
假设user3和user4目录下的Demo1.java都更新到了最新版本。user3修改Demo1.java，提交至仓库。若user4也修改Demo1.java并提交，此时user4的TortoiseSVN会报提交版本过时的错误，并提醒user4需要更新，更新时就会发生冲突。如下图：选择更新

![](Pasted%20image%2020230418192524.png)

![](Pasted%20image%2020230418192724.png)
![](Pasted%20image%2020230418192741.png)

![](Pasted%20image%2020230418192825.png)


对于每个更新冲突的文件，Subversion会在冲突文件所在目录下放置了三个文件：
Demo1.java.mine：发生冲突时的本地版本。
Demo1.java.r3：最后更新之后的本地版本。
Demo1.java.r4：仓库中的最新版本。

解决办法：
在Demo1.java上右击 — TortoiseSVN — 编辑冲突，这时你需要确定哪些代码是需要的，做一些必要的修改然后保存。小技巧：编辑冲突时，可使用直接复制需要的代码到Merged窗口即可。
编辑完成后保存，直接选择标记为已解决，即标记为冲突已解决。退出编辑冲突窗口，发现冲突发生时生成的三个文件被自动删除了，且Demo1.java变成了未提交状态。

![](Pasted%20image%2020230418193027.png)

![](Pasted%20image%2020230418193125.png)
![](Pasted%20image%2020230418193247.png)

### 6.其他操作
#### 1)删除：delete
删除文件或目录，不能直接用Windows的删除命令来操作，那样只是没有显示出来，实际并没有删除，在更新后，删除的文件又会被更新出来的。要想从库中删除，必须选中你要删除的内容，TortoiseSVN — delete，这样才会将这个文件标记成要删除的。确认需要删除后，使用前面所讲的提交命令，就会真正的在库中删除了。
如果直接在仓库删除就无法恢复了。

#### 2)重命名：rename
重命名也不能直接用Windows的重命名命令来操作，必须选中你要重命名的文件，TortoiseSVN — rename。修改后提交就可以更新到仓库。
改名的处理方式相当于新增了一个以新名称命名的文件，原名称命名的文件进行了删除。

#### 3)还原：revert
在未提交之前，你对前面做的操作反悔了，可以使用revert来恢复。

#### 4)检查更新：Check for modifications
 此功能可以显示你所做的修改有哪些还没有提交的。还可以看到版本库里的改动，即别人提交了哪些文件的改动，你还没更新到本地。

#### 5)导出：export
使用SVN的工作空间每个目录下面都有一个.svn隐藏目录，利用SVN的export命令可轻松地导出不含.svn目录的工作空间。
