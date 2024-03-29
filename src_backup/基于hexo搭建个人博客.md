基于 hexo 搭建个人博客

[toc]

# 环境准备

在 windows 中用 git bash 执行命令。如果 git bash 中无法用 npm 命令，重启就好了

## 安装 Node.js

访问[官网](http://nodejs.cn/download/)下载即可，安装时默认安装即可。

## 安装 CNPM 插件

通过 npm 安装插件有时网速会很慢，cnpm 使用淘宝镜像，当 cnpm 上没有你需要的插件时，它会自动去 npm 服务器上寻找同时在保存至 cnpm 服务器上。

安装淘宝镜像

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

使用方法

```
cnpm install [name]
```

## 安装Hexo

执行如下命令安装Hexo：

```bash
cnpm install hexo-cli -g
```

## 安装自动部署插件

```bash
cnpm install hexo-deployer-git --save
```

## 本地查看效果

```bash
hexo g
hexo s
```

登录 http://localhost:4000/ 查看效果：

## Hexo 初始化配置

新建一个 hexo 文件夹，进入此文件夹，右键鼠标，点击 Git Bash Here，输入以下命令：

```
hexo init
```

## 安装 git

访问[官网](https://git-scm.com/downloads)下载即可，安装时默认安装即可

## 配置 git 个人信息

git 通过用户名和邮箱区分用户，原则上不要求与 github 一致，但建议一致。

替换用户名和邮箱后执行如下命令：

```bash
git config --global user.name y*0
git config --global user.email z*@*.com
```

## 创建 github 仓库

创建一个新的仓库，注意仓库的名字要为：github用户名+.github.io，eg: `Richard-coder.github.io`

## 配置 SSH 密钥

通过 SSH 密钥完成本地仓库与 github 仓库的同步

替换邮箱后，执行如下命令，出现提示则一直回车即可

```bash
ssh-keygen -t rsa -C z*@*.com
```

## 在 github 账户中添加 ssh 公钥

在 C:\Users\用户名\.ssh下找到 id_rsa.pub，用编辑器打开，复制

登录 github，Setting--> SSH and GPG keys --> New ssh keys，Title可以随便写，粘贴公钥

## 将博客部署到github

复制 github 仓库的 ssh 地址，eg: `git@github.com:Richard-coder/Richard-coder.github.io.git`

打开`hexo` 文件夹下的`_config.yml`文件，按照下图进行修改。

![image-20200118005331786](img/%E5%9F%BA%E4%BA%8Ehexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2.assets/image-20200118005331786.png)

在`hexo` 文件夹下执行如下命令：

```bash
hexo g
hexo d
```

访问`https://你的用户名.github.io`, 比如我的是`https://richard-coder.github.io/`就可以通过此链接访问博客了。

## 写文章

创建一个新的文章有两种方式，一种是执行如下命令，会在`hexo\source\_posts`生成 文章标题.md 文件

```bash
hexo n "文章标题"
```

另一种方式是直接在`hexo\source\_posts`目录下右键鼠标新建文本文档，改后缀为 .md 即可。

## 切换主题

[点击此处](https://hexo.io/themes/)进入 Hexo 官网的主题专栏，选择喜欢的主题，点击进去即可找到主题的 github 地址。再打开 Hexo 文件夹下的 themes 目录，右键 Git Bash Here，输入以下命令：

```bash
git clone 此处填写你刚才复制的主题地址
```

比如克隆主题 Aero-Dual ，命令为：

```bash
git clone https://github.com/levblanc/hexo-theme-aero-dual
```

等待下载完成后即可在 themes 目录下生成 hexo-theme-aero-dual 文件夹，然后打开 Hexo 文件夹下的配置文件 _config.yml ，找到关键字 theme，修改参数为：theme：hexo-theme-aero-dual （其他主题修改成相应名称即可），再次注意冒号后面有一个空格！

![img](img/%E5%9F%BA%E4%BA%8Ehexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2.assets/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMTkvMDMvMjUvNWM5OGY4N2MwYTdlOS5wbmc.jpg)

更新主题后可能需要执行如下命令清楚缓存，否则服务器可能更新不了主题

```bash
hexo clean
```

hexo 中有两个主要的配置文件，文件名都是`_config.yml`，均用于站点配置，一份位于站点根目录下，eg: `hexo\_config.yml`，主要用于 hexo 整站的设置，另一个位于主题目录下，eg: `hexo\themes\hexo-theme-aero-dual\_config.yml`, 这个配置文件由主题作者提供，用于配置主题相关选项，按需修改即可。

# 使用 GitHub 分支保存 Hexo 环境和博文

我们的 `username.github.io` 的 master 分支, 保存了我们用 `Hexo` 生成的静态 `Html`，但如何保存 `Hexo` 环境和博客的 `markdown` 源码呢？

解决办法是，在 `github` 上新建一个 `GitHub Page : gatieme.github.io`. 在这个仓库下, 另外新增加一个 `hexo` 分支, 并且设置该分支为主分支。这种方法需要重新准备环境，创建 hexo 文件夹和 github 仓库。

这种方法的原理：github page 要求使用 master 发布静态网页，并且 github 仓库的默认分支是 master 分支，git clone 下载代码时，拉取的时默认分支的代码。因此，可以创建一个 hexo 分支保存 hexo 环境及文章的 .md 文件，并更改 hexo 分支为默认分支，这样 push 和 pull 的就是 hexo 分支了，并且提交源码时不必担心将静态网页带上去，因为有`.gitignore`将其忽略了。而网站的静态网页则通过 deploy 插件完成到 github 的部署。

```bash
git branch hexo
```

## 操作步骤

1. 创建仓库，仓库名为`username.github.io`，并勾选用 readme 初始化。

   ![image-20200118185207917](img/%E5%9F%BA%E4%BA%8Ehexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2.assets/image-20200118185207917.png)

2. clone 仓库到本地，并将仓库文件夹重命名为 hexo 

```bash
git clone git@github.com:Richard-coder/Richard-coder.github.io.git
```

3. 创建 hexo 分支，并将新分支推送到 github。

```bash
git branch hexo #创建新分支
git checkout hexo #切换到新分支
git push -u origin hexo #将新分支推送到github
```

4. 将 hexo 分支设置为默认分支

github 仓库页面，点选 branch-> change defult branch。

![image-20200118192307276](img/%E5%9F%BA%E4%BA%8Ehexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2.assets/image-20200118192307276.png)

![image-20200118192341553](img/%E5%9F%BA%E4%BA%8Ehexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2.assets/image-20200118192341553.png)

![image-20200118192606431](img/%E5%9F%BA%E4%BA%8Ehexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2.assets/image-20200118192606431.png)

5. 初始化 hexo 环境

先把 hexo 文件夹下的内容拷贝出来（.git 文件夹和 readme.md ），否则 hexo 无法初始化，执行如下命令：

```bash
hexo init
```

hexo 初始化完成了，将原文件夹下的内容拷贝回来。

6. 配置 `Hexo` 的 `deploy`.

配置 hexo 文件夹下的 `_config.yml` 中的 `deploy` 参数, 分支应为 `master`.

```bash
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: git@github.com:Richard-coder/Richard-coder.github.io.git
  branch: master
```

7. 生成静态网页

```bash
hexo g
hexo d 
```

如果hexo d后 ERROR Deployer not found: git ，执行下面命令即可解决：

```
cnpm install --save hexo-deployer-git
```

8. 保存本地 hexo 环境，并推送到 github

```bash
git add .
git commit -m "..."
git push origin hexo
```

## 日常博文修改

在本地对博客进行修改添加新博文、修改样式等等后. 通过下面的流程进行管理：

1. 依次执行git add .、git commit -m “…”、git push origin hexo指令将改动推送到GitHub (此时当前分支应为hexo)

2. 然后才执行 hexo generate -d 发布网站到 master 分支上.
   

## 换电脑写博客

当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤 :

- 安装git
```
sudo apt-get install git
```

- 设置git全局邮箱和用户名
```
git config --global user.name "yourgithubname"
git config --global user.email "yourgithubemail"
```

- 设置ssh key
```
ssh-keygen -t rsa -C "youremail"
#生成后填到github和coding上（有coding平台的话）
#验证是否成功
ssh -T git@github.com
ssh -T git@git.coding.net #(有coding平台的话)
```

- 安装nodejs
```
sudo apt-get install nodejs
sudo apt-get install npm
```


- 安装hexo
```
sudo npm install hexo-cli -g
```

无需执行`hexo init` 进行初始化

在任意文件夹下, 将 Richard-coder.github.io 仓库 clone 到本地(默认分支为hexo)

```
git clone git@github.com:Richard-coder/Richard-coder.github.io.git
```

然后进入克隆到的文件夹, 依次执行下列指令:

```bash
cd xxx.github.io
cnpm install
cnpm install hexo-deployer-git --save
```

生成，部署：
```
hexo g
hexo d
```

然后就可以开始写你的新博客了
```
hexo new newpage
```
### Tips:

不要忘了，每次写完最好都把源文件上传一下
```
不要忘了，每次写完最好都把源文件上传一下
```

如果是在已经编辑过的电脑上，已经有clone文件夹了，那么，每次只要和远端同步一下就行了

```
git pull
```


注意:

>hexo init 会初始化 Hexo 环境, 该操作会清空.git 文件夹, 导致版本控制信息会丢失.
>
>因此除了首次搭建环境之外, 无需运行此命令.
>
>如果万不得已出现 Hexo 环境损坏, 需要重新初始化, 可以先拷贝出 .git 文件夹, 然后搭建环境并初始化之后, 将 .git 信息重新拷贝回来.

## 图片不能显示问题

参考:
> [hexo博客如何插入图片](https://zhuanlan.zhihu.com/p/265077468)
> [资源文件夹](https://hexo.io/zh-cn/docs/asset-folders.html)

进入到博客文件夹
```
cd Richard-coder.github.io
```

将 `config.yml` 文件中的 `post_asset_folder` 选项设为 `true` 来打开。

```
#_config.yml
post_asset_folder: true
```

上述设置的作用:
> 当资源文件管理功能打开后，Hexo将会在你每一次通过 `hexo new [layout] <title> `命令创建新文章时自动创建一个文件夹。这个资源文件夹将会有与这个文章文件一样的名字。将所有与你的文章有关的资源放在这个关联文件夹中之后，你可以通过相对路径来引用它们，这样你就得到了一个更简单而且方便得多的工作流。



安装相对路径引用的标签插件
```
cnpm install hexo-renderer-marked
```

之后在`config.yaml`中手动添加配置如下：
```
marked:
  prependRoot: true
  postAsset: true
```

上述插件及设置作用
> 不安装插件, markdown中引用图片方式为 `{% asset_img example.jpg This is an example image %}` , 安装插件后, 可以通过使用相对路径的常规 markdown 语法` ![](example.jpg)`来引用图片


为了便于管理可本地查看, 创建`src_backup`文件夹, 所有博客源代码放在此文件夹下, 文档编写完成后, 执行如下命令, `file_1`是文章名字, 可替换为任意字符串

```
hexo new file_1
```

上述命令会在`source/_posts` 创建`file_1.md`文件和`file_1`文件夹, 将原始文档的所有图片拷贝到`file_1`文件夹下, 将原始文档内容拷贝到`file_1.md`文件中, 并修改`file_1.md`文件中图片的引用方式, 一般用替换就好, 比如将
```
![image-20200118192606431](img/file_name.assets/image-20200118192606431.png)
```
替换为
```
![image-20200118192606431](image-20200118192606431.png)
```
即只应用文件名即可

这样
```
hexo g
hexo d
```
便可以正常显示图片了

## Butterfly 主题

参考

> [Butterfly 安装文档(一) 快速开始](https://butterfly.js.org/posts/21cfbf15/)

### 下载并应用主题

进入到博客文件夹
```
cd Richard-coder.github.io
```

butterfly主题需要hexo版本大于5.3, 升级hexo
```
npm update #不要有cnpm
npm audit fix
npm audit fix --force
hexo vesion #查看版本 
```

下载主题
```
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

删除`themes\butterfly\.git`文件夹, 因为博客文件夹下已有`.git`, 多个git嵌套会导致推送失败

安装插件
```
cnpm install hexo-renderer-pug hexo-renderer-stylus --save
```

应用主题, 修改 Hexo 根目录下的 _config.yml，把主题改为butterfly
```
theme: butterfly
```

为了减少升级主题后带来的不便，请使用以下方法（建议，可以不做）。

在 `hexo` 的根目录创建一个文件 `_config.butterfly.yml`，并把主题目录的 `_config.yml` 内容复製到 `_config.butterfly.yml` 去。( 注意: 复製的是主题的 `_config.yml` ,而不是 `hexo` 的 `_config.yml`)

> 注意： 不要把主题目录的 `_config.yml` 删掉

> 注意： 以后只需要在 `_config.butterfly.yml`进行配置就行。
> 如果使用了 `_config.butterfly.yml`， 配置主题的 `_config.yml` 将不会有效果。

`Hexo`会自动合併主题中的`_config.yml`和 `_config.butterfly.yml`里的配置，如果存在同名配置，会使用`_config.butterfly.yml`的配置，其优先度较高。

### 配置主题



## 部署参考

> [hexo超完整的搭建教程，让你拥有一个专属个人博客](https://zhuanlan.zhihu.com/p/44213627)
> 
> [使用hexo，如果换了电脑怎么更新博客？ - 直上云霄的回答 - 知乎](https://www.zhihu.com/question/21193762/answer/489124966)
> 
