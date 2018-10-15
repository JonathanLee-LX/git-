# git的基本操作和配置

## git的基本操作

### 工作原理

.git文件夹中包含了众多功能不一的文件夹和文件，这些文件夹和文件是描述Git仓库所必不可少的信息，不可以随意更改或删除；尤其需要注意的是，.git文件夹随着项目的演进，可能会变得越来越大，因为任何文件的任何一个变动，都需要Git在objects文件夹下将其重新存储为一个新的对象文件，因此如果一个文件非常大，那么你提交几次改动就会造成.git文件夹容量成倍增长。因此，.git文件夹更像是一本书，每一个版本的每一个变动都存储在这本书中，而且这本书还有一个目录，指明了不同的版本的变动内容存储在这本书的哪一页上，

* **版本控制**：版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。

1.  本地版本控制

        在硬盘上保存补丁集（补丁是指文件修订前后的变化）；通过应用所有的补丁，可以重新计算出各个版本的文件内容。

2.  集中化的版本控制

        如何让在不同系统上的开发者协同工作？

        有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。

        优点：每个人都可以在一定程度上看到项目中的其他人正在做些什么。 而管理员也可以轻松掌控每个开发者的权限，并且管理一个 CVCS 要远比在各个客户端上维护本地数据库来得轻松容易。

        缺点：中央服务器的单点故障

3.  分布式版本控制系统

        如何解决本地版本控制系统保存在单一位置

        任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。 因为每一次的克隆操作，实际上都是一次对代码仓库的完整备份。



### 常用命令

帮助 ：`git help <verb> / git <verb> --help / man git-<verb>`

 推送文件四部曲 ：

    1. git add

    2. git status

    3. git commit / git commit -m ''
    
    4. git push

跳过使用暂存区域的方式 : `git commit -a -m 'added new benchmarks'`

从已跟踪文件清单中移除文件 ： `git rm`

想让文件保留在磁盘，但是并不想让 Git 继续跟踪 ：`git rm --cached README`

重命名 : `git mv file_from file_to`

查看提交历史 ： `git log`
            
    常用的选项:
    -p ：按补丁格式显示每个更新之间的差异
    --stat ：每次提交文件的简略的统计信息
    --pretty ：指定使用不同于默认格式的方式展示提交历史
        子选项 ：  
            oneline  将每个提交放在一行显示 例 git log --pretty=oneline
            short ：
            full :
            fuller :
            format ：可以定制要显示的记录格式
            graph ：展示分支、合并历史 例 git log --pretty=format:"%h %s" --graph

>  --graph需要与oneline 或 format 选项 结合使用时

按照时间做限制 ： `--since 和 --until `

例 ：`git log --since=2.weeks` 列出所有最近两周内的提交

显示指定作者的提交 ： `git log --author`

搜索提交说明中的关键字 ： `git log --grep`

> 注意： 如果要得到同时满足这两个选项搜索条件的提交，就必须用 --all-match 选项。否则，满足任意一个条件的提交都会被匹配出来

列出那些添加或移除了某些字符串的提交 : `git log -Sfunction_name`

只关心某些文件或者目录的历史提交 : `git log --path`

撤销操作 ： `git commit --amend`

取消暂存文件 ：` git reset HEAD <file>`

撤销对文件的修改 ：`git checkout -- <file>`

查看已经配置的远程仓库服务器 ：`git remote`
    
    子选项：
        -v ：显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL    

添加远程仓库 ： `git remote add <shortname> <url>` 

远程仓库中获得数据 ：`git fetch [remote-name]`

自动的抓取然后合并远程分支到当前分支 ：`git pull`

克隆一个仓库 ：`git clone <url>`

推送到远程仓库 ：`git push [remote-name] [branch-name]`

例 将 master 分支推送到 origin 服务器 ：`git push origin master`

查看某一个远程仓库 ：`git remote show [remote-name]`
     
     查看更多信息 ：`git remote show`

远程仓库重命名 ：`git remote rename`

例 将 pb 重命名为 paul ：`git remote rename pb paul`

远程仓库移除 ：`git remote rm [remote-name]`

列出标签 ：`git tag`

创建标签：

     轻量标签：它只是一个特定提交的引用。如果你只是想用一个临时的标签，或者因为某些原因不想要保存那些信息，也用轻量化标签
     附注标签（建议使用）：附注标签是存储在 Git 数据库中的一个完整对象。它们是可以被校验的；其中包含打标签者的名字、电子邮件地址、日期时间；还有一个标签信息；并且可以使用 GNU Privacy Guard （GPG）签名与验证。

创建轻量标签 ：`git tag [tag-name]`

创建附注标签：`git tag -a`

> 一般与`-m''`搭配使用 例 git tag -a v3 -m 'BIMBox version 3'

对过去的提交打标签 ：`git tag -a [tag-name] [sha1]`

## git的配置

### git的全局配置和仓库级别的配置

### 代码提交前需要进行的配置

### hook的配置
