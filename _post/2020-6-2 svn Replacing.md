### 前因简介
---
今天早上有同事吐槽说之前在打包机器上使用自己的svn co下来了一些文件，导致自己每次改密码的时候都要去那台机器上重新输入一次密码。
这是一种很不安全的方式，之前研究过，在linux下svn的密码是明文存储的，存储在~/.subversion/auth/svn.simple下，在这台机器上的所有svn密码都会看到，
正好之前处理过类似的工作，所以作为一个服务端程序我义无反顾的决定使用公共账号重新co一份下来，把同事的隐私密码保留起来。
正是因为这样的原因，导致今天踩了一个svn的坑。

#### 操作流程

操作流程其实很简单，按照之前规定的程序co下来固定的目录之后，然后执行了导表部分，自动提交之后，看起来没什么问题的样子。
就在这时，问题出现了，在别的目录下svn up之后发现有几个重要目录出现了svn replacing状态，查看replacing的地方，发现之前所有的log都消失了。


#### svn replacing

查询之后发现，svn replacing是指在同一svn目录下，先执行了svn delete后又进行svn add，这样svn就会将文件或目录置为replacing状态，之前所有的提交记录都消失了。

#### 解决方案

使用
```
svn cp http://xxx@rev http://xxx2
svn mv http://xxx http://xxx3
svn mv http://xxx2 http://xxx
svn rm http://xxx3
```
或是
```
svn copy 目标文件/文件夹 -r 正确版本 临时文件名
svn delete  目标文件/文件夹
svn copy 临时文件名 目标文件/文件夹
svn ci *
```
这样就把之前版本的恢复过来，同时也保留了之前的log

#### 过程中的小插曲

在机器上操作时，可能会出现tree conflict，这时候只是svn revert 可能会有问题，需要svn revert --depth=infinity 即可

#### 收尾工作

处理好这一部分之后需要研究一下为啥会出现这个问题，重新做了一次操作，发现这次啥情况都没有了- -|||
以后做这类操作的时候还是要注意下不要提交，先在本地操作好之后再上传~

