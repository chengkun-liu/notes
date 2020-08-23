# 第1章 Git

***

### 1. 版本控制的功能（作用）

* 自动合并代码
* 每一行代码的历史可追溯
* 每次提交都有一个版本号

### 2. 常见版本控制软件

* `SVN`：集中式 版本控制，所有客户端只能通过一个远程仓库沟通 服务器单点故障，容错性差
* `Git`：分布式版本控制，由本地仓库和远程仓库组成，每个本地仓库都可以作为远程仓库使
  * 速度快
  * 分布式
  * 强大的分支管理
* Git的相关概念

​	1.版本库：".git"目录，存放日志信息，版本信息和配置信息

​	2.暂存区：版本库中的index 文件夹，保存进行了临时修改的文件

​	3.工作区：包含".git"文件的目录（".git"作为一级目录），主要用于存放代码

​	4.工作区中文件的两种状态		

​		①未跟踪（untracked，未纳入版本控制）

​		②已跟踪（tracked，已纳入版本控制）

​					Unmodified 未修改状态

​					Modified 已修改状态

​					Staged 已暂存状态

### 3. Git常用命令

#### 3.1 本地仓库

* 环境配置
  * 项目级别和系统级别同时存在时，以项目级别设置的信息为准
  * 项目级别
    * `git config user.name`  设置用户名
    * `git config user.email` 设置邮箱
    * 查看位置：.git/config
  * 系统级别
    * `git config --global user.name`  设置用户名
    * `git config --global user.email` 设置邮箱
    * 查看位置：~/.gitconfig（当前用户的家目录下的隐藏文件夹）
* 获取仓库
  * `git clone + url` ：克隆远程仓库
  * `git init `： 初始化本地仓库（并没有和远程仓库建立联系）
* 查看状态
  * `git status`
  * `git status -s `
  * `git diff`
* 查看设置
  * `git config --system/local/global --list`
* 查看日志
  * `git log` 命令行模式
  * `git reflog` ：显示移动到其他版本指针需要移动步数 HEAD@{num}
  * `git log --pretty=oneline`
  * `git log --oneline`
  * `gitk` 图形界面模式
* 前进后退
  * 本质：将HEAD指针指向不同的版本
  * 基于索引操作(推荐)：`git reset --hard 局部索引或全部索引`
  * `git reset`三个参数比较
    * `--soft`：仅仅移动本地仓库的指针（很少使用）
    * `--mixed`：移动本地仓库和暂存区的指针（很少使用）
    * `--hard`：移动本地仓库、暂存区和工作区的指针（推荐使用）
* 提交暂存
  * `git add +文件名称`  提交文件到暂存区或解决冲突
  * `git add . ` 提交所有文件到暂存区
  * `git reset HEAD -- + 文件名称`  撤销 提交到暂存区操作
  * `git rm --cached +文件名称` 撤销 提交到暂存区操作
* 提交仓库（本地）
  * `git commit -m + 日志内容` 把文件从暂存区提交到本地仓库
  * `git commit` 进入VI模式书写日志信息
  * `git commit -a -m`
  * `这种操作之后，文件的状态转换为已跟踪未修改状态，是不显示出来的`
* 删除文件（必须是提交到本地的才能正常删除）
  * `git rm +文件名称`  删除工作区文件并提交到暂存区，`git commit -m "删除日志信息"`将删除动作提交到本地仓库，进行跟踪记录，同时操作删除文件才算成功
  * 手动删除文件，默认没有提交到暂存区，要先提交到暂存区，再进行提交
  * 删除文件如何恢复：移动HEAD指针到未删除的版本即可，然后在备份需要的文件
    * 删除命令已经提交到本地仓库
      * `git reset --hard 局部索引或全部索引(版本号)`
    * 删除命令还未提交到本地仓库
      * `git restore + 删除文件的名称`
* 文件比较
  * `git diff + 文件名称`：默认当前工作区文件和当前暂存区文件进行比对
  * `git diff 部分索引或全部索引 + 文件名称`：默认当前工作区文件和本地仓库对应版本文件进行比对
  * `git diff `：当前工作区和当前暂存区所有文件进行比对
  * `git diff 部分索引或全部索引`：当前工作区所有文件和本地仓库对应版本所有文件进行比对
* 忽略列表
  * `touch .gitignore` 命令行模式创建文件，配置忽略信息
    * `*.iml` 忽略所有以`.iml`结尾的文件
    * `!lib.iml` 不忽略 此文件，通常和第一种联合使用
    * `/TODO` 忽略当前目录下的`TODO` `文件夹
    * `doc/*.txt` 忽略doc一级目录中以`.txt`结尾的文件
    * `doc/**/*.pdf` 忽略doc目录下所有以`.PDF`结尾的文件

#### 3.2 远程仓库

##### 3.2.1 远程仓库

* 查看远程仓库
  * ` git remote`
  * `git remote -v`
  * `git remote show <shortName>` 

* 添加仓库（远程）
  * `git remote add <shortName> + url` 添加远程仓库，通常和`git init`联合使用，一个本地仓库可以同时与多个远程仓库建立联系
* 删除远程仓库
  * `git remote rm + url(<shortName>)` 仅仅删除与远程仓库联系
*  远程抓取
  * `git fetch <shortName> 分支名称`  + `git merge origin/master` 抓取远程仓库数据到本地仓库的`objects`文件夹，需要手动合并到工作区
  * `git pull origin master`抓取远程仓库数据到本地仓库工作区间
  * `覆盖式抓取，存在相同的文件覆盖，存在不同的文件包conflict` ，`git pull origin master --allow-unrelated-histroies` 强行完成抓取
* 远程推送
  * `git push origin（url） master(分支)`

##### 3.2.2 分支操作

* fatal: Not a valid object name: 'master'.首次创建的仓库必须要提交东西到本地仓库之后，才可以创建分支
* 查看分支
  * `git branch` 查看本地分支
  * `git branch -r` 查看远程分支
  * `git branch -a` 查看g所有分支
* 创建分支（创建的分支是以当前的分支为基础的，很重要）
  * `git branch 分支名称`
* 切换分支
  * `git checkout 分支名称`
* 推送本地分支到远程仓库
  * `git push origin（url） 分支名称`
* 合并分支
  * 切换到合并分支上
  * `git merge +被合并分支的名称`
  * 合并分支时冲突产生原因：多个用户基于同一个版本，对同一个文件的同一个的位置做了修改。git在合并代码是会合并完成后，提交到本地仓库，再从本地仓库提交到远程仓库本地分支和远程分支产生了关系，直接 `git push menjin(url) +分支名称`，就能将本地的最新信息推送到远程仓库中存存储
* 删除分支
  * `git branch -d(-D,强制删除 )`删除本地分支，当存在本地分支和远程分支的内容存在差异时，只能强制删除 ，防止内容丢；失如果处于当前分支，则不能删除自己，只能删除其他分支
  * `git push menjin(url) -d + 分支名称` 删除远程分支
* 避免冲突
  * 不能完全避免，只能尽量减少
  * 分工明确
  * 及时更新代码（拉取），早中晚各一次
* 解决冲突
  * 打开产生产生冲突的文件，手动修改
  * `git add + 文件名称`
  * `git commit -m + 日志信息`：此时不要添加文件名称

##### 3.2.3 标签操作

* 某个分支在某个时间点的特定状态，可以通过标签回到特定的时间点

* 创建标签
  * `git tag 标签名称（v1.0见名知义）`
* 查看标签
  * `git tag` 查看所有标签
  * `git tag show 标签名称` 查看某一个标签的详细信息
* 检出标签：创建一个分支指定对应的版本号
  * `git checkout -b 分支名称 标签名称`
* 推送标签到远程仓库
  * `git push menjin(url) + 标签名称`
* 删除标签
  * `git tag -d 标签名称` 删除本地标签
  * `git push 远程仓库名 :refs/tags/标签名称` 删除远程标签

1. IDEA中操作Git

   * 配置Git操作命令
     * `setting-version control`指定`git.exe`位置 完成后进行测试
   * 创建本地仓库
     * `VCS——>important -->create Git Repository -->选工厂目录（重点 ）`
   * 提交到暂存区
     * `鼠标右键-Git-Add（工程中文件颜色变成绿色）`
   * 提交到本地仓库
     * `鼠标右键-Git-commit(还有另外提交)`
   * 推送到远程仓库
     * `鼠标有右键-Repository-push-define`
   * 从远程仓库clone代码
     * `回到idea主界面-点击check out from Version Control ->Git`
   * 从远程仓库拉取
   * 版本对比
     * `鼠标右键-Git-compare with->选择版本信息->右边是最新版本,版本存在差异的地方会自动颜色，后期代码多了很重要的 `
   * 创建分支
     * `VCS->Git->branches`可以创建分支，查看所有分支
   * 切换分支
     * `VCS->Git->branches-选择想要切换的分支，然后点击checkout,在idea的右下角可以看到当前的分支情况`
   * 合并分支
     * `鼠标右键->Git->Respository->merge branch`

### 4. Git执行流程

1. 第一次，从远程仓库clone代码到本地仓库，后续想要获取最新代码直接通过拉取
2. 从本地仓库检取（检出）代码到工作区进行操作
3. 提交代码到暂存区
4. 暂存区提交到本地仓库，本地仓库保管了各个历史版本
5. 需要和团队共享 代码时，将代码push到远程仓库

### 5. 提高github访问速度

1. 参考原文`https://www.wuaisite.com/post-78.html`

2. 修改本地host文件

   ```xml
   # 查询域名对应的ip地址 https://www.ipaddress.com/
   140.82.113.3 www.github.com
   140.82.113.3 github.com
   ```

3. `ipconfig /flushdns`

# 第2章 Github

## 1. 使用目的

* 托管项目代码

## 2. 基本概念

1. Repository：仓库存放项目代码，一个项目对应一个仓库
2. Star：收藏项目，方便下次查看
3. Fork：复制克隆项目，将别人的项目完完整整的赋值一份到自己的仓库下。该Fork的项目是独立存在的。
4. Pull Request：更新代码，如果对方绝对还行，就会合并到自己的代码中
5. Watch：关注，如果关注了某一个项目，该项目的后续更新都会收到通知
6. Issues：事务卡片，发现代码有bug，但是代码还没有成型，就可以通过Issues和对方讨论这是不是bug。
7. Github首页：
8. 仓库主页：
9. 个人主页：

## 3. 用户注册

1. sign in：登录
2. sign up：注册
3. 翻墙软件：Shadowsocks

## 4. 登录首页

1. https://github.com/

## 5. 仓库管理

### 1. 新建文件

### 2. 编辑文件

### 3. 删除文件

### 4. 上传文件

### 5. 搜索文件

1. 可以使用快捷键T

### 6. 检出项目

## 6. Issues

### 1. 作用

* 发现代码可能存在bug，但是代码没有成型，需要双方讨论
* 发现开源项目出现问题，需要讨论解决

## 7. Github实用操作

1. `关键字 in:name,readme,description`
2. `关键字 stars:>=10000 forks:>10000`
3. `关键字 stars:1000..5000 forks:5000..10000`
4. `awesome + 想学习的内容`
5. `location:shanghai language:java`

### 1. Pull Request

​	**申请合并**

1. 先 fork 别人的仓库，相当于拷贝一份，相信我，不会有人直接让你改修原仓库的
2. clone 到本地分支，做一些 bug fix
3. 发起 pull request 给原仓库，让他看到你修改的bug
4. 原仓库进行Code Review（代码审查），如果没有什么问题，仓库所有者就会 把你提交的代码merge 到项目中

1. `Assignees`就是`负责人`的意思，负责处理这个`issue`

### 3. Marketplace

### 4. Explore

如何在github上找项目练手

1. in:name spring_boot  stars:>3000 forks:>1000
2. in:readme +要搜索的内容
3. in:description +微服务   language：java   pushed：2019-03-01

# 第3章 Git

## 1. 目的

* 使用git管理GitHub，进行代码托管

## 2. 工作区域

1. 工作空间：添加，编辑，修改文件等动作
2. 暂存区：暂存已经修改或添加的文件等资源，最后统一提价到本地仓库
3. 本地仓库：最终确定的资源保存到本地仓库，成为一个新的版本，并且对外暴露