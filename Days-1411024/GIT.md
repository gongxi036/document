[TOC]



![](https://res.cloudinary.com/drvm6gm5e/image/upload/v1731554194/Git_cbqmw3.webp) 

# 一、git使用准备

- 一般企业中使用代码管理工具Git开发时都是通过拉分支进行功能细致开发，所以掌握git的分支操作是必要的
- 使用git下载指定分支命令为： `git clone -b 分支名仓库地址 `
- **初始开发git操作流程**
  - 本地创建公钥 `ssh-keygen -t rsa -C "youremail@example.com" `,邮箱改成自己的邮箱，然后一直回车。
  - 公钥的地址：`c/user/Administrator/.shh/.id_rsa.pub `;
  - 克隆最新主分支代码 `git clone 地址`
  - 克隆注定分支代码 `git clone -b branch 地址`,一般克隆的代码都是`master`分支
- 注册用户
  - 注册使用者邮箱 `git config --global user.email "you@example.com" `
  - 注册用户名 `git config --global user.name "Your Name" `
  - 修改提交的用户名    `git config --global user.name '名字' `、`git config user.name '名字' `
  - 查看`config`的配置信息 `git config --list`





# 二、必备知识点

- 概念

  ![流程图](https://res.cloudinary.com/drvm6gm5e/image/upload/v1731554196/gitflow_lkwdij.webp)

  1. **Remote:** 远程主仓库；
  2. **Repository：** 本地仓库；
  3. **index：** Git追踪树，暂存区；
  4.  **workspace：** 本地工作区（即你编辑器的代码）

- 一般的操作流程：《工作区》-> `git status`查看状态 ->  `git add .` 将所有修改的文件加入暂存区 -> `git commit -m '提交描述'`  将代码提交到本地仓库 -> `git push` 将本地仓库代码更新到远程仓库

# 三、git remote

- 为远程仓库指定别名，以便于管理远程主机，默认只有一个是为 `origin` （主机名、别名）

1. 查看主机名： `git remote` 
2. 查看主机名即网址： `git remote -v`
   - 默认克隆远程仓库到本地时，远程主机为 `origin` ,如需指定别名可用 `git clone -o <别名> <远程git地址>` 
3. 添加远程主机： `git remote add origin <远程git地址>` 
4. 删除远程主机： `git remote rm orign`
5. 修改远程知己的别名： `git remote rename <原主机名> <新主机名>` 
6. 查看本地分支与远程分支情况

   ```bash
   git remote show origin
   ```
7. 本地同步远程已删除的分支（同步）

   ```bash
   git remote prune origin
   ```

   

# 四、本地的分支操作

1. 创建分支： `git checkout -b <branch>` 

2. 查看本地分支： `git branch` 

3. 对分支重命名：`git branch -m <old> <new>`

4. 删除本地分支： `git branch --delete <branch>`

   注意： the branch  XXX is not fully merged    //  XXX分支与当前分支的内容不一致  没有进行合并 

   `git branch -D <branch>` 

5. 当你乱改了工作区的摸个文件，想直接丢弃工作区的修改时

   `git checkout --file` 

6. 分支合并： `git merge <branch>`  将改分支合并到当前分支

7. 修改`commit` 的注释内容（未进行push操作）

   `git commit --amend`     [参考地址](<https://blog.csdn.net/yulsh/article/details/54613189>)  

8. 一般克隆的代码都是拉取的`master`分支，拉取分支操作

   1. `git clone -b branch 地址` ,拉取指定分支的代码
   2. 拉取`master`分支之后，想拉取远程某一分支的代码到本地
      - `git checkout -b 本地分支名x origin/本地分支名x`,使用该方式会在本地新建分支x，并自动切换到该本地分支x,本地分支会和远程分支建立映射关系
      - `git fetch origin 远程分支名x:本地分支名x`,使用该方式会在本地新建分支x，但是不会自动切换到该本地分支x，需要手动checkout,本地分支不会和远程分支建立映射关系

# 五、远程分支操作

1. 创建远程分支： `git push origin <本地分支>:<远程分支>` 

2. 删除远程分支
   1. `git push origin  :<远程分支>` ，就是`push` 一个空的本地分支到远程分支上
   2. `git push origin --delete <远程分支>` 

3. 基于远程分支创建本地分支

   `git checkout -b <本地分支> origin/<远程分支>`

4. 远程分支与本地分支建立联系
   1. `git push origin <远程分支>`； 只限与本次提交
   2. `git branch --set-upstream-to=origin/<远程分支> <本地分支>`；与远程分支永久建立联系

5. 如果远程账号修改了密码，则需要重新输入密码，再提交

   `git config --system --unset credential.helper` 

# 六、基本操作流程

1. 本地处理

   1. 先建立本地仓库： `git init` ；

      注意： 需要有一个 `.gitignore` 文件，把不需要的文件进行忽略。

   2.  把文件添加到暂存区： `git add .` 

   3. `git commit -m '提交描述'`  把修改的文件提交到本地仓库

   >  [commit 规范](<http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html>)
   >
   >  - feat：添加新功能（feature）
   >  - fix：修复 Bug
   >  - docs：仅仅修改文档（documentation）
   >  - style： 仅仅修改代码格式（不影响代码运行的变动）
   >  - refactor：重构（即不是新增功能，也不是修改bug的代码变动）
   >  - test：增加测试
   >  - chore：构建过程或辅助工具的变动

2. 与远程仓库建立链接 （参考 `git remote` 的内容）

3. 把代码上传到远程仓库 ： `git push` 

   注意： 

   - 如果是第一次提交 则需要强行提交

     `git push -u origin master`

   - 如果远程仓库与本地有冲突（远程仓库不是空的），则需要覆盖

     `git push -u origin master -f` 


# 七、版本回退

1. `git checkout -- <文件名>` / `git checkout .` :  本地修改了一堆文件(并没有使用git add到暂存区)，想放弃修改
2. `git reset HEAD <文件名>`/`git reset HEAD .`：本地修改了一推文件，已经使用git add到暂存区，想放弃修改
3. `git reset HEAD <文件名>` ： 把暂存区的修改撤销，重新放回工作区
4. `git reset --hard commit_id` ： 完成撤销,同时将代码恢复到前一commit_id对应的版本
5. `git reset commit_id` ： 完成Commit命令的撤销，但是不对代码修改进行撤销，可以直接通过git commit 重新提交对本地代码的修改。



# 八、后续问题

- 错误： `refusing to merge unrelated histories`

  解决方案： `git pull origin master --allow-unrelated-histories ` 



# 九、储藏

**使用场景**：现在你想切换分支，但是还不想提交现有代码。

```
// 存储修改的内容
$ git stash
// 存储新增和修改的内容
$ git stash push -u / git stash --include-untracked
```

1. 查看现有的所有储藏，此命令显然暗示了git stash可以多次保存工作进度，并用在恢复时候选择。

```git
$ git stash list
```

2. 重新应用已经实施的储藏

```git
$ git stash pop
```

3. 删除某个stash

```git
$ git stash drop stash@{x} 
```

4. 清空当前所有的stash

```git
$ git stash clear 
```



# 补充

## 标签

1. 创建标签
   - 创建轻量标签： `git tag v0.2.0` 
   - 创建附注标签： `git tag -a v0.2.0 -m '备注'` 
   - 给特定的`commit`版本打标签：`git tag v1.0 commitId`

2. 查看附注标签
   - 列出当前仓库的所有标签： `git tag` 
   - 列出符合模式的标签： `git tag -l 'v0.1.*'`
   - 查看标签版本信息： `git show v0.1.0`

   



## 删除标签

- 误打或需要修改标签时，需要先删除标签，再打新标签

  `git tag -d v0.1.0` 

- 删除远程标签

  `git push origin -d v1.0` 

  `git push origin :ref/tags/v1.0`

## 补打标签

- 给指定的`commit` 打标签

  `git tag v0.1.0 49e0cd22f6bd9510fe65084e023d9c4316b446a6 `

## 发布标签

- 将 `v0.1.0` 标签提交到`git ` 服务器

  `git push origin v0.1.0`

- 将本地所有标签一次性提交到`git` 服务器

  `git push origin --tag` 

##  `Commit` 规范

一个标准的 Git 提交信息通常应包含以下几个部分：

1. **类型（Type）**：说明提交的类型，例如修复（fix）、特性（feat）、文档（docs）、样式（style）、重构（refactor）、测试（test）等。
2. **范围（Scope）**（可选）：说明提交影响的范围，通常是提交所涉及的模块或文件。
3. **主题（Subject）**：简要说明提交的目的或变更。
4. **正文（Body）**（可选）：详细描述提交的内容，必要时可以分多行，解释变更的动机和具体内容。
5. **页脚（Footer）**（可选）：用于添加关联的任务或问题的追踪编号，或注明破坏性变更（Breaking Changes）。

#### 示例

下面是一个标准的 Git 提交信息的示例，遵循 [Conventional Commits](https://www.conventionalcommits.org/) 规范：

```bash
feat(auth): add JWT authentication

// 正文
Add JWT authentication to the user login flow. This change includes
generating a token upon successful login and validating the token
for protected routes.

// 页脚
BREAKING CHANGE: The login endpoint now returns a JWT token which
is required for subsequent requests.
```

#### 编写规范

1. **主题**：不超过50个字符，尽量简洁明了。
2. **正文**：详细描述变更，必要时可以分段。
3. **页脚**：注明重大变更、关联的任务编号或其他需要特别注意的信息



# 参考文献

[廖雪峰教程](<http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000> )  

[简书教程](<http://www.jianshu.com/p/072587b47515> ) 

