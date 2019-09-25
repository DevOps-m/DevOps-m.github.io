---
title: 基于Hexo+GitHub构建个人Blog
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-25 11:45:12
password:
summary:
tags:
- Linux
categories:
- Linux
---

### 快速部署：

#### 目录结构说明：
-  "_config.yml"主要用于修改个人信息。

### 部署过程：

> 参考文献：https://godweiyang.com/2018/04/13/hexo-blog/

#### 1. 安装Git：
- 下载地址：https://git-scm.com/download/win
- 安装环境检测：在Windows运行窗口输入"git --version"，如果有版本号信息输出，则表示安装成功。

#### 2. 安装Node.js：
- 下载地址：https://nodejs.org/zh-cn/
- 安装环境检测：在Windows运行窗口输入"node -v"和"npm -v"，如果有版本号信息输出，则表示安装成功。

#### 3. 配置国内镜像源（这里使用淘宝源）：
```
npm config set registry https://registry.npm.taobao.org
```

> PS：鼠标右击，选择"Git Bash Here"，在Git Bash运行上述命令即可。

#### 4. 注册GitHub账号：
- 注册GitHub账号用于存放Blog站点，以及发布代码。[GitHub地址](https://github.com)
- 创建项目，输入以自己GitHub名字命名的项目，后面一定要加".github.io" 后缀，"README"初始化也一定要勾上，否则拉取仓库会有问题。

> PS：项目名字一定要和你GitHub名字完全一样，比如：你的GitHub名字叫"hello" ，那么仓库的名字一定要是"hello.github.io"。

- 此时项目已建成，接着点"Settings"，向下翻找到"GitHub Pages"，点击"Choose a theme"选择一个主题，然后等待一会，再回到"GitHub Pages"界面然后默认主题页面就会出来。

#### 5. 设置GitHub Key：
- 用于免密拉取项目代码。

- 创建SSH Key，将"id_rsa.pub"里的内容放入GitHub SSH KEY界面：
```
ssh-keygen -t rsa -P "" -f ~/.ssh/id_rsa -C "1005437573@qq.com"
```

- 设置Git Config个人信息：
```
git config --global user.name "李志强"
git config --global user.email "1005437573@qq.com"
```

> PS：鼠标右击，选择"Git Bash Here"，在Git Bash运行上述命令即可。

#### 6. Hexo玩法：
- 默认Hexo玩法：

```
// 在Windows系统上在合适的地方存放自己的博客文件，比如：D:\myblog。

// 切换到D:\myblog目录下，进入Git Bash窗口进行操作，操作如下：
npm i hexo-cli -g   // 安装hexo-cli；
hexo -v             // 验证hexo-cli的安装；
hexo init           // 初始化博客目录结构；
npm install         // 安装npm依赖库；
hexo generate       // 生成站点静态资源，可用"hexo g"简写；
hexo server         // 本地启动服务，默认4000端口，可用"hexo s"简写；
```

> PS：剩余内容就是更换主题，更适合自己，但是官方上的主题，Bug不少，用起来不太舒服，官方主题地址：https://hexo.io/themes/

- 拉现成代码进行修改快速部署，操作如下：

```
// 拉取代码：
git clone git@github.com:DevOps-m/hexo-matery-modified.git myblog

// 参考这默认设置，进行修改_config.yml文件内容，重点关注一下内容配置（将文章发布到GitHub设置），以为我的为例所示：
hexo根目录下的_config.yml地址：https://github.com/DevOps-m/hexo-matery-modified/blob/master/_config.yml

deploy:
- type: git                                                     // 使用发布类型；
  repository: git@github.com:DevOps-m/DevOps-m.github.io.git    // 部署代码仓库；
  branch: master                                                // 部署分支；

// 博客界面上的设置，还需要修改主题的_config.yml文件内容，可参考默认的_config.yml设置，根据自己需要进行按需修改：
主题_config.yml地址：https://github.com/DevOps-m/hexo-matery-modified/blob/master/themes/matery/_config.yml
```

#### 7. Hexo写文章，发布文章，以及发布博客相关修改操作：
```
// 安装hexo扩展包：
npm i hexo-deployer-git

// 新建文章：
hexo new post "文章标题"    // myblog\source\_posts目录下会多出一个文文件夹和一个以.md结尾的文件，一个用来存放图片数据，一个就是新建的文章文件；

// 编写文章：
只需要修改.md结尾的文件内容即可。

// 发布文章及本地预览：
hexo g      // 生成站点静态页面；
hexo s      // 本地启动服务预览；
hexo d      // 根据在_config.yml配置的GitHub仓库地址进行发布到对应的仓库；
```

