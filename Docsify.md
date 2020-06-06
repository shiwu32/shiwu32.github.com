### docsify安装使用

#### 0. Docsify 是什么

一个好用的文档网站生成工具。

docsify 是一个动态生成文档网站的工具。不同于 GitBook、Hexo 的地方是它不会生成将 `.md` 转成 `.html` 文件，所有转换工作都是在运行时进行。

这将非常实用，如果只是需要快速的搭建一个小型的文档网站，或者不想因为生成的一堆 `.html` 文件“污染” commit 记录，只需要创建一个 `index.html` 就可以开始写文档而且直接[部署在 GitHub Pages 或国内Gitee。



**特性**

- 无需构建，写完文档直接发布
- 容易使用并且轻量 (~18kB gzipped)
- 智能的全文搜索
- 提供多套主题
- 丰富的 API
- 支持 Emoji
- 兼容 IE10+
- 支持 SSR



首先下载安装好nodejs环境

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

**4.1 Github Corner**

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

**4.2 配置导航栏及嵌套**

```js
<script>
  window.$docsify = {
    loadNavbar: true
  }
</script>
```

接着在项目根目录创建`_navbar.md`文件，内容格式如下：

```markdown
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