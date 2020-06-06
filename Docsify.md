### docsify安装使用

首先安装好nodejs

#### 1. 全局安装docsify

```
npm i docsify-cli -g
```

#### 2. 初始化项目

```bash
docsify init ./docs

# 初始化成功后，可以看到 ./docs 目录下创建的几个文件
index.html 入口文件
README.md 会做为主页内容渲染
.nojekyll 用于阻止 GitHub Pages 会忽略掉下划线开头的文件

```

#### 3. 本地预览网站

```
docsify serve
```

#### 4. 常用配置项

**Github Corner**

index.html中配置

```js
<script>
        window.$docsify = {
            name: '拾悟',
            repo: 'https://github.com/shiwu/kubernetes.git',
            coverpage: true
        }
    </script>
```

### 配置导航栏及嵌套

```
<script>
  window.$docsify = {
    loadNavbar: true
  }
</script>
```

接着在项目根目录创建`_navbar.md`文件，内容格式如下：

```
* [DevOps](devops/ops.md)
  * [Docker](devops/docker.md)
  * [Kubernetes](devops/kubernetes.md)
  * [CI/CD](cicd.md)
* [自动化](auto/ops.md)
  * [Ansible](ansible.md)
* [云服务](cloud/ops.md)
* [安全](safe/ops.md)
```

任务技能：自动化、DevOps、云服务、Docker、kubernetes、安全......

openresty/1.13.6.2			