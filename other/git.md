常用操作

```
git init
git add .
git commit -m "first commit"
git remote add origin [url]
git push origin master
```



修改git用户密码

```
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

