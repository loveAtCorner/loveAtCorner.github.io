## 博客搭建

采用了新的基于go语言的博客框架`Hugo`来搭建我的个人博客

下面是一些参考文档：

* [Hugo官网](https://gohugo.io/)
* [Hugo中文文档](https://www.gohugo.org/)
* [使用 Hugo 和 GitHub Pages 搭建静态博客](https://waynerv.com/posts/setup-blog-with-hugo-and-github-pages/)

### 环境配置

#### Git安装

这个工具是在后序部署博客到`github pages`上必须的，mac端和windows端都是ok的

* [Git官网下载地址](https://git-scm.com/downloads)

下载安装好之后，可以查看一下`Git`是否已经安装好了：

* windows端：在桌面点击右键，选择`Git bash here`，如果出现了，其实就已经安装好了。如果不放心还是可以输入：

  ```shell
  git version
  ```

  如果出现了版本号信息，则表示已经安装好了。

* mac端：打开终端`terminal`同样输入上述的命令：

  ```shell
  git version
  ```

  如果出现了版本号信息，则表示已经安装好了。

#### Hugo安装

Hugo安装总共有两种方式：

1.文件下载方式（Linux，mac，windows）：

[Hugo最新releases版本下载官网](https://github.com/gohugoio/hugo/releases)

2.命令行下载的方式：

​	Hugo普通版本：

* windows端：最好采用文件下载方式，因为后续还要找到`hugo.exe`的地址，并配置环境变量。

* mac端：使用brew安装：

  ```shell
  brew install hugo
  ```

#### Hugo配置

这是windows端特有的步骤(与java的jdk配置相似)：

1.找到安装好的`hugo.exe`

2.点击复制文件路径

3.找到到windows系统设置的`高级系统设置`，点击`环境变量`,在用户变量的`path`中添加你复制好的`hugo路径`,应用了之后就配好了

4.右键桌面，点击`Git bash here`，输入

```shell
hugo version
```

若能显示版本信息，就说明安装成功了。

#### Hugo扩展版本：

**这个是否选择安装与你安装的主题有关，如果你选择的主题需要扩展版本，那么就需要安装，如果不需要则忽略这个模块的内容！**

* windows端：

  1.在桌面右键`Git bash here`输入：

  ```shell
  iwr -useb get.scoop.sh | iex
  ```

  如果出错了，就输入：

  ```shell
  Set-ExecutionPolicy RemoteSigned -scope CurrentUser
  ```

  2.使用scoop安装扩展版本，输入:

  ```shell
  scoop install hugo-extended
  ```

* mac端：我没有试过，应该命令是差不多的

### 新建站点

这里用我的站点名称`myblog`作为例子：

选择要生成站点的路径，`Git bash here`，输入：

```shell
hugo new site myblog
cd myblog
ls -l   //显示站点myblog的文件目录
```

这时会出现很多文件夹和一个配置文件。

### 下载主题

如果没有主题的样例来进行修改，是很难自己进行博客的写入的！所以我们一般的办法就是下载一个主题根据它的示例站点`exampleSite`来修改配置文件以及写入博客内容。

* [Hugo官方主题](https://themes.gohugo.io/)

  1.注意官方网站本身就是用Hugo框架来搭建的，如果喜欢，可以将第一个主题`Academic`下载并进行修改。

  2.点击主题后，即可进入下载页面，点击`Demo`可以查看相应主题的示例网站，点击`Download`可以进入相应主题的github仓库，记得给它个star哦！

  我们点击Download，点击`code`，复制https链接,这里我使用的是stack主题

  3.在站点根目录右键`Git bash here`，输入：

  ```shell
  cd themes
  git clone https://github.com/zhaohuabing/hugo-theme-cleanwhite.git
  ```

### 本地启动服务端口（https）

*这个功能主要用于修改配置参数时，查看预览效果的功能，但并不会生成html文件，是非常重要的。*

1.退出themes目录：

```shell
cd ..
```

2.启动：

```shell
hugo serve -t hugo-theme-cleanwhite
```

会提示`Web Server is available at //localhost:1313/`，复制`//localhost:1313/`在网页端预览。

### 写博客文件

Hugo官方采用Markdown格式进行博客的撰写。

如果你要新建一个博客内容，可以按照下面的方式进行。

* 创建一个 `about` 页面：

```shell
$ hugo new about.md
```

`about.md` 自动生成到了 `content/about.md` ，打开 `about.md` 文件看下：

```
+++
date = "2015-10-25T08:36:54-07:00"
draft = true
title = "about"

+++

正文内容
```

内容是 `Markdown` 格式的，`+++` 之间的内容是 [TOML](https://github.com/toml-lang/toml) 格式的，根据你的喜好，你可以换成 [YAML](http://www.yaml.org/) 格式（使用 `---` 标记）或者 [JSON](http://www.json.org/) 格式。

* 创建第一篇文章，放到 `post` 目录，方便之后生成聚合页面。

```bash
$ hugo new post/first.md
```

打开编辑 `post/first.md` ：

```
---
date: "2015-10-25T08:36:54-07:00"
title: "first"
 
---

### Hello Hugo

 1. aaa
 1. bbb
 1. ccc
```

### 新建Github仓库，作为部署网站的仓库

注意事项：**新建的仓库名必须与你的github用户名相同**，以我的github名字loveAtCorner为例。

我的仓库名为：`loveAtCorner.github.io`，点击创建，注意先不要生成`README.md`文件，否则你在第一次推送文件前还需要先拉取文件。

如果你没有`VPN`，不能稳定访问github，可以使用`Gitee`，这个仓库名就没有限制了

### 生成博客站点（由.md文件生成.html文件）

1.在根目录下输入如下命令：

```shell
hugo --theme=hugo-theme-cleanwhite --baseURL="https://loveAtCorner.github.io/" --buildDrafts
```

你可以根据你的主题名和github仓库名不同来进行更改。

2.此时，根目录下会多出一个`public`的文件夹，里面会有几个配置文件和一些html文件，它们分别是由config配置文件和markdown博客文件生成的。我们将其作为部署网站的根目录。

### 在Github Pages或者Gitee Pages部署网站

* 使用Github配置：

  步骤：

  * 进入public文件夹

    ```shell
    cd public
    ```

  * 创建git本地仓库，并上传github

    ```shell
    git init		//git初始化
    git add .
    git commit -m "我的第一次博客提交"   //将文件提交至本题仓库
    git remote add origin https://github.com/loveAtCorner/loveAtCorner.github.io.git //将本题仓库与远端的github仓库进行关联
    git push -u origin master //将文件上传到远端的github仓库的master分支
    ```

  * 输入你的github用户名和github密码

    当然你也可以提前配置`SSH`钥匙

    ```shell
    git config --global user.name "用户名"
    git config --global user.email "邮箱地址"
    ```

    然后生成SSH公钥，并配置到github上，**具体步骤可自行百度**，如果嫌麻烦可以每次输入用户名和密码。

  * 最后你就可以使用`loveAtCorner.github.io`的域名进行访问你的静态博客了。

在github上的步骤就是如此了。

* 使用Gitee配置：

  前面的步骤和github一样，需要多一步手动生成`Gitee Pages`：

  **进入远端仓库，点击`服务`，点击`Gitee Pages`，勾选`强制使用HTTPS`，点击生成，就可以访问了。

### 更改博客内容后上传Github（和第一次上传有所不同）

1.在本地博客的根目录进入public：

```shell
cd public 
```

2.更改文件之后上传

这里的更改是指通过更改cotents文件夹的.md文件重新生成.html文件。

```shell
git add .
git commit -m "你要备注的内容"
git push -u origin master //如果你只有master分支也可以直接使用 git push
```


