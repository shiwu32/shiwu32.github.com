常用操作

```
git init
git add .
git commit -m "first commit"
git remote add origin [url]
git push origin master
```



修改git用户密码

```bash
查看当前用户名和邮箱
git config user.name
git config user.email
修改
git config --global user.name "zhangsan(新的用户名)"
git config --global user.email "123456@qq.com(新的邮箱)"
git config --global user.password "123456(新的密码)"
//这里的zhangsan和邮箱都是你修改之后的用户名和邮箱

git config --system --unset credential.helper
git config --global credential.helper store
```



git修改远程仓库地址

```bash
# 方法有三种：
1.修改命令
git remote origin set-url [url]

2.先删后加
git remote rm origin
git remote add origin [url]

3.直接修改config文件
```



Gitee pages

```bash
username.gitee.io
创建仓库时仓库名和用户名一致即可
```

Gibhub pages

```
username.github.io
# 创建仓库用username.github.com 
```

同时将本地git同步到gitee和github

```bash
git push 的语法是这样的：
$ git push <远程主机名> <本地分支名>:<远程分支名>
因此如果有多个远程需要关联，只需要

$ git remote add hostA 仓库A地址 
$ git remote add hostB 仓库B地址 
然后同步的时候

git push hostA <本地分支名>:<远程分支名>
如果你想设置hostA为默认的，也可以使用-u参数。
 
实际操作：
git remote add github https://github.com/shiwu32/shiwu32.github.com.git
git remote add gitee https://gitee.com/shiwu32/shiwu32.git
 
git push gitee master:master
git push github master:master
```



## 报错与解决方法

```bash

$ git remote add hub https://github.com/shiwu32/shiwu32.github.com.git

$ git push hub master:master
To https://github.com/shiwu32/shiwu32.github.com.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/shiwu32/shiwu32.github.com.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

问题分析：我是先在github上创建了空仓库, 当时勾选了创建readme.md文件, 但是在利用gitbash的时候没有pull到本地.之后利用gitbash进行提交本地文件到仓库, 在push的时候出现错误.

解决办法：错误信息中可以看出本地和远程匹配不完整, 在网上也查找了下, 总结为本地仓库和远程仓库有冲突所致 .
总结的几种解决办法:

1: 进行push前先将远程仓库pull到本地仓库
$ git pull origin master    #git pull --rebase origin master
$ git push -u origin master

2: 强制push本地仓库到远程 (这种情况不会进行merge, 强制push后远程文件可能会丢失 不建议使用此方法)

$ git push -u origin master -f
3: 避开解决冲突, 将本地文件暂时提交到远程新建的分支中

$ git branch [name]
# 创建完branch后, 再进行push
$ git push -u origin [name]

```

