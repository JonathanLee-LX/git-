# Git的分支管理最佳实践

## 单主干
单主干的分支实践（Trunk-based development，TBD）在 SVN 中比较流行。Google 和 Facebook 都使用这种方式。trunk 是 SVN 中主干分支的名称，对应到 Git 中则是 master 分支。TBD 的特点是所有团队成员都在单个主干分支上进行开发。当需要发布时，先考虑使用标签（tag），即 tag 某个 commit 来作为发布的版本。如果仅靠 tag 不能满足要求，则从主干分支创建发布分支。bug 修复在主干分支中进行，再 cherry-pick 到发布分支。图 1 是 TBD 中分支流程的示意图。
图 1. TBD 中的分支流程的示意图
![图 1. TBD 中的分支流程的示意图](https://www.ibm.com/developerworks/cn/java/j-lo-git-mange/img001.png)
由于所有开发人员都在同一个分支上工作，团队需要合理的分工和充分的沟通来保证不同开发人员的代码尽可能少的发生冲突。持续集成和自动化测试是必要的，用来及时发现主干分支中的 bug。因为主干分支是所有开发人员公用的，一个开发人员引入的 bug 可能对其他很多人造成影响。不过好处是由于分支所带来的额外开销非常小。开发人员不需要频繁在不同的分支之间切换。

## Github flow
GitHub flow 是 GitHub 所使用的一种简单的流程。该流程只使用两类分支，并依托于 GitHub 的 pull request 功能。在 GitHub flow 中，master 分支中包含稳定的代码。该分支已经或即将被部署到生产环境。master 分支的作用是提供一个稳定可靠的代码基础。任何开发人员都不允许把未测试或未审查的代码直接提交到 master 分支。

对代码的任何修改，包括 bug 修复、hotfix、新功能开发等都在单独的分支中进行。不管是一行代码的小改动，还是需要几个星期开发的新功能，都采用同样的方式来管理。当需要进行修改时，从 master 分支创建一个新的分支。新分支的名称应该简单清晰地描述该分支的作用。所有相关的代码修改都在新分支中进行。开发人员可以自由地提交代码和 push 到远程仓库。

当新分支中的代码全部完成之后，通过 GitHub 提交一个新的 pull request。团队中的其他人员会对代码进行审查，提出相关的修改意见。由持续集成服务器（如 Jenkins）对新分支进行自动化测试。当代码通过自动化测试和代码审查之后，该分支的代码被合并到 master 分支。再从 master 分支部署到生产环境。图 2 是 GitHub flow 分支流程的示意图。
图 2. Github flow 中的分支流程的示意图
![图 2. Github flow 中的分支流程的示意图](https://www.ibm.com/developerworks/cn/java/j-lo-git-mange/img002.png)
GitHub flow 的好处在于非常简单实用。开发人员需要注意的事项非常少，很容易形成习惯。当需要进行任何修改时，总是从 master 分支创建新分支。完成之后通过 pull request 和相关的代码审查来合并回 master 分支。GitHub flow 要求项目有完善的自动化测试、持续集成和部署等相关的基础设施。每个新分支都需要测试和部署，如果这些不能自动化进行，会增加开发人员的工作量，导致无法有效地实施该流程。这种分支实践也要求团队有代码审查的相应流程。

## git-flow
git-flow 应该是目前流传最广的 Git 分支管理实践。git-flow 围绕的核心概念是版本发布（release）。因此 git-flow 适用于有较长版本发布周期的项目。虽然目前推崇的做法是持续集成和随时发布。有的项目甚至可以一天发布很多次。随时发布对于 SaaS 服务类的项目来说是很适合的。不过仍然有很大数量的项目的发布周期是几个星期甚至几个月。较长的发布周期可能是由于非技术相关的因素造成的，比如人员限制、管理层决策和市场营销策略等。

git-flow 流程中包含 5 类分支，分别是 master、develop、新功能分支（feature）、发布分支（release）和 hotfix。这些分支的作用和生命周期各不相同。master 分支中包含的是可以部署到生产环境中的代码，这一点和 GitHub flow 是相同的。develop 分支中包含的是下个版本需要发布的内容。从某种意义上来说，develop 是一个进行代码集成的分支。当 develop 分支集成了足够的新功能和 bug 修复代码之后，通过一个发布流程来完成新版本的发布。发布完成之后，develop 分支的代码会被合并到 master 分支中。

其余三类分支的描述如表 1所示。这三类分支只在需要时从 develop 或 master 分支创建。在完成之后合并到 develop 或 master 分支。合并完成之后该分支被删除。这几类分支的名称应该遵循一定的命名规范，以方便开发人员识别。
表 1. git-flow 分支类型

分支类型 | 命名规范 | 创建自 | 合并到 | 说明 |
--------|----------|-------|-------|-------|------|
feature | feature/*  | develop | develop | 新功能
release | release/*  | develp | develop和master | 一次版本的发布
hotfix | hotfix/*  | master | develop和master | 生产环境中发现的紧急 bug 的修复
对于开发过程中的不同任务，需要在对应的分支上进行工作并正确地进行合并。每个任务开始前需要按照指定的步骤完成分支的创建。例如当需要开发一个新的功能时，基本的流程如下：
- 从 develop 分支创建一个新的 feature 分支，如 feature/my-awesome-feature。
- 在该 feature 分支上进行开发，提交代码，push 到远端仓库。
- 当代码完成之后，合并到 develop 分支并删除当前 feature 分支。

在进行版本发布和 hotfix 时也有类似的流程。当需要发布新版本时，采用的是如下的流程：

- 从 develop 分支创建一个新的 release 分支，如 release/1.4。
- 把 release 分支部署到持续集成服务器上进行测试。测试包括自动化集成测试和手动的用户接受测试。
- 对于测试中发现的问题，直接在 release 分支上提交修改。完成修改之后再次部署和测试。
- 当 release 分支中的代码通过测试之后，把 release 分支合并到 develop 和 master 分支，并在 master 分支上添加相应的 tag。

因为 git-flow 相关的流程比较繁琐和难以记忆，在实践中一般使用辅助脚本来完成相关的工作。比如同样的开发新功能的任务，可以使用 git flow feature start my-awesome-feature 来完成新分支的创建，使用 git flow feature finish my-awesome-feature 来结束 feature 分支。辅助脚本会完成正确的分支创建、切换和合并等工作。

## Maven JGit-Flow
对于使用 Apache Maven 的项目来说，Atlassian 的 JGit-Flow 是一个更好的 git-flow 实现。JGit-Flow 是一个基于 JGit 的纯 Java 实现的 git-flow，并不需要安装额外的脚本，只需要作为 Maven 的插件添加到 Maven 项目中即可。JGit-Flow 同时可以替代 Maven release 插件来进行发布管理。JGit-Flow 会负责正确的设置不同分支中的 Maven 项目的 POM 文件中的版本，这对于 Maven 项目的构建和发布是很重要的。

在 Maven 项目的 pom.xml 文件中添加代码清单1中的插件声明就可以使用 JGit-Flow。<configuration>中包含的是 JGit-Flow 不同任务的配置。

清单 1. JGit-Flow 的 Maven 设置

``` xml
<build>
   <plugins>
       <plugin>
           <groupId>external.atlassian.jgitflow</groupId>
           <artifactId>jgitflow-maven-plugin</artifactId>
           <version>1.0-m5.1</version>
           <configuration>
              <flowInitContext>
                <versionTagPrefix>release-</versionTagPrefix>
              </flowInitContext>
               <releaseBranchVersionSuffix>RC</releaseBranchVersionSuffix>
               <noDeploy>true</noDeploy>
               <allowSnapshots>true</allowSnapshots>
               <allowUntracked>true</allowUntracked>
           </configuration>
       </plugin>
   </plugins>
</build>
```
JGit-Flow 提供了很多配置选项，可以在 POM 文件中声明。这些配置项可以对不同的任务生效。常用的配置项如表2 所示。

表 2. JGit-Flow 的配置项

## 选择合适的实践
每个开发团队都应该根据团队自身和项目的特点来选择最适合的分支实践。首先是项目的版本发布周期。如果发布周期较长，则 git-flow 是最好的选择。git-flow 可以很好地解决新功能开发、版本发布、生产系统维护等问题；如果发布周期较短，则 TBD 和 GitHub flow 都是不错的选择。GitHub flow 的特色在于集成了 pull request 和代码审查。如果项目已经使用 GitHub，则 GitHub flow 是最佳的选择。GitHub flow 和 TBD 对持续集成和自动化测试等基础设施有比较高的要求。如果相关的基础设施不完善，则不建议使用。
## 小结
Git 作为目前最流行的源代码管理工具，已经被很多开发人员所熟悉和使用。在基于 Git 的团队开发中，Git 分支的作用非常重要，可以让团队的不同成员同时在多个相对独立的特性上工作。本文对目前流行的 3 种 Git 分支管理实践做了介绍，并着重介绍了 git-flow 以及与之相关的 Maven JGit-Flow 插件

## Git的Commit Message的格式
每次提交，Commit message 都包括三个部分：Header，Body 和 Footer。
```
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```
其中，Header 是必需的，Body 和 Footer 可以省略。

不管是哪一个部分，任何一行都不得超过72个字符（或100个字符）。这是为了避免自动换行影响美观。
### Header
Header部分只有一行，包括三个字段：type（必需）、scope（可选）和subject（必需）
### type
`type`用于说明commit的类别，只允许使用下面七个标识
```
- feat：新功能（feature）
- fix：修补bug
- docs：文档（documentation）
- style： 格式（不影响代码运行的变动）
- refactor：重构（即不是新增功能，也不是修改bug的代码变动）
- test：增加测试
- chore：构建过程或辅助工具的变动
```
如果`type`为`feat`和`fix`，则该 commit 将肯定出现在 Change log 之中。其他情况（docs、chore、style、refactor、test）由你决定，要不要放入 Change log，建议是不要。
### scope
`scope`用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。
### subject
`subject`是 commit 目的的简短描述，不超过50个字符。
```
- 以动词开头，使用第一人称现在时，比如change，而不是changed或changes
- 第一个字母小写
- 结尾不加句号（.）
```
### body
Body 部分是对本次 commit 的详细描述，可以分成多行。下面是一个范例。

```
More detailed explanatory text, if necessary.  Wrap it to
about 72 characters or so.

Further paragraphs come after blank lines.

- Bullet points are okay, too
- Use a hanging indent
```
有两个注意点。

1. 使用第一人称现在时，比如使用change而不是changed或changes。
2. 应该说明代码变动的动机，以及与以前行为的对比

### Footer
Footer 部分只用于两种情况。

### 不兼容的变动
如果当前代码与上一个版本不兼容，则 Footer 部分以`BREAKING CHANGE`开头，后面是对变动的描述、以及变动理由和迁移方法。
```
BREAKING CHANGE: isolate scope bindings definition has changed.

    To migrate the code follow the example below:

    Before:

    scope: {
      myAttr: 'attribute',
    }

    After:

    scope: {
      myAttr: '@',
    }

    The removed `inject` wasn't generaly useful for directives so there should be no code using it.
```
### 关闭Issue
如果当前 commit 针对某个issue，那么可以在 Footer 部分关闭这个 issue 。
```
Closes #234
```
也可以一次关闭多个 issue 。
```
Closes #123, #245, #992
```
### Revert
还有一种特殊情况，如果当前 commit 用于撤销以前的 commit，则必须以revert:开头，后面跟着被撤销 Commit 的 Header。
```
revert: feat(pencil): add 'graphiteWidth' option
This reverts commit 667ecc1654a317a13331b17617d973392f415f02.
```
Body部分的格式是固定的，必须写成This reverts commit &lt;hash>.，其中的hash是被撤销 commit 的 SHA 标识符。

如果当前 commit 与被撤销的 commit，在同一个发布（release）里面，那么它们都不会出现在 Change log 里面。如果两者在不同的发布，那么当前 commit，会出现在 Change log 的Reverts小标题下面

## Commitizen
Commitizen是一个撰写合格 Commit message 的工具。

安装命令如下。

`$ npm install -g commitizen`
然后，在项目目录里，运行下面的命令，使其支持 Angular 的 Commit message 格式。

`$ commitizen init cz-conventional-changelog --save --save-exact`
以后，凡是用到git commit命令，一律改为使用git cz。这时，就会出现选项，用来生成符合格式的 Commit message。

![](http://www.ruanyifeng.com/blogimg/asset/2016/bg2016010605.png)

## 生成 Change log
如果你的所有 Commit 都符合 Angular 格式，那么发布新版本时， Change log 就可以用脚本自动生成（例1，例2，例3）。

生成的文档包括以下三个部分。






