##Git 忽略文件名大小写
git congfig --get core.ignorecase 查看本地仓库git忽略大小写是打开还是关闭。

>$ git config --get core.ignorecase
>
>true

true 则会忽略文件名的大小写
可以将之设置为false

>$ git config core.ignorecase false

更改为false 之后你更改文件名之后 git status 

```
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   app/src/main/java/com/scottfu/brightbook/YxtTActivity.java

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        app/src/main/java/com/scottfu/brightbook/YXTTActivity.java
```
现在提交 仓库就会多出一个文件

        new file:   app/src/main/java/com/scottfu/brightbook/YXTTActivity.java
        modified:   app/src/main/java/com/scottfu/brightbook/YxtTActivity.java

##推荐操作
* 将忽略大小写设为默认值 $ git config core.ignorecase true 以免之后合并分支等操作 因为大小写引起的冲突

* $ git mv a A 将a更名为A

```
$ git mv app/src/main/java/com/scottfu/brightbook/YxtTActivity.java app/src/main/java/com/scottfu/brightbook/YXTTActivity.java
```

然后提交就可以了

```
运行 $ git mv 就相当于运行如下指令
$ mv README.md README
$ git rm README.md
$ git add README
```


[参考博文](http://www.procedurego.com/article/97947.html)