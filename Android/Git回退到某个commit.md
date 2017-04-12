##Git 回退到某个commit版本

----

[参考](http://www.cnblogs.com/sundemeng/archive/2012/03/23/git-reset.html)

[教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013744142037508cf42e51debf49668810645e02887691000)

* git log  查看每次commit的SHA1值或者git log --pretty=oneline 单行显示 
* git reset --hard commit_id(具体的某个commit版本)，HEAD（最近一个提交）HEAD^（上一次）




回退所有内容到上一个版本 
 
git reset HEAD^ 
 
回退a.py这个文件的版本到上一个版本 
 
git reset HEAD^ a.py
 
向前回退到第3个版本 
 
git reset –soft HEAD~3 
 
将本地的状态回退到和远程的一样 
 
git reset –hard origin/master 
 
回退到某个版本 
 
git reset 057d 
 
回退到上一次提交的状态，按照某一次的commit完全反向的进行一次commit 
 
git revert HEAD 
 