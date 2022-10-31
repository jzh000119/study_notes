https://www.liaoxuefeng.com/wiki/896043488029600

# GIT 廖雪峰学习笔记

---

VScode 的 git bash 终端按 Ctrl + s 会卡住，按 Ctrl + z 恢复。

## Git 简介 

1. 配置名称和邮箱：

```bash
git config --global user.name ""
git config --global user.email ""
```

> global 表示计算机上所有的 git 仓库都会使用这个配置

2. 空文件夹创建版本库：

```bash
git init
```

> 目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

3. 文件放入 git 仓库：

```bash
git add file.txt
git commit -m " "
```

> git add 提交所有修改到暂存区（stage）
>
> git commit 一次性把暂存区的所有修改提交到分支

![image-20221026170847094](https://jzh119.oss-cn-beijing.aliyuncs.com/img/image-20221026170847094.png)

## 时光机穿梭

### 版本回退

1. 查看提交的 git 快照：

```bash
git log
```

<img src="https://jzh119.oss-cn-beijing.aliyuncs.com/img/image-20221026164248715.png" alt="image-20221026164248715" style="zoom:67%;" />

<img src="https://jzh119.oss-cn-beijing.aliyuncs.com/img/image-20221026164406692.png" alt="image-20221026164406692" style="zoom:67%;" />

> pretty 漂亮的

2. HEAD 表示当前版本，上一个版本 HAED^, 上两个版本 HEAD^^。

+ 回退到上一个版本：

```bash
$ git reset --hard HEAD^
HEAD is now at 756cb1c commit
```

+ 回退到 `add data` ,根据版本号进行回退 （版本号写几位就好）

```bash
$ git reset --hard 2e43d
HEAD is now at 2e43daa add data
```

> 版本回退，只是相当于改变 HEAD 指针指向

+  记录了分支变化的过程：`git reflog` 用于确定要回到未来的哪个版本 

```bash
$ git reflog
756cb1c (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
2e43daa HEAD@{1}: reset: moving to 2e43d
756cb1c (HEAD -> master) HEAD@{2}: reset: moving to HEAD^
2e43daa HEAD@{3}: reset: moving to 2e43d
756cb1c (HEAD -> master) HEAD@{4}: reset: moving to HEAD^
2e43daa HEAD@{5}: commit: add data
756cb1c (HEAD -> master) HEAD@{6}: commit: commit
8250d9f HEAD@{7}: commit (initial): commit
```

### 工作区和暂存区

1. 查看工作区和版本库里面最新版本的区别：

```bash
$ git diff HEAD -- t.txt
diff --git a/t.txt b/t.txt
index 4ea4e08..b1c48f6 100644
--- a/t.txt
+++ b/t.txt
@@ -1,3 +1,3 @@
 1. hello
 2. data
-3. git tracks changes.
\ No newline at end of file
+3. git tracks changes of file.
\ No newline at end of file
```

> 每次修改 都需要 git add 、 git commit 两步 
>
> 确保 commit 前 add 所有文件



### 撤销更改

1. 撤销更改：(这些命令在输入 `git status` 时有提示)

<img src="https://jzh119.oss-cn-beijing.aliyuncs.com/img/image-20221026172044946.png" alt="image-20221026172044946" style="zoom:67%;" />

:one: 未 `git add`,撤销 工作区更改

```bash
# 让文件回到最近一次 git commit 或 git add 的状态
git restore t.txt
```

:two: 已经 `git add` ，但是未 `git commit` ,现在就是 暂存区干净，工作区 有修改,可以再 使用 1 撤销 工作区的修改

```bash
git restore --staged t.txt
```

:three: 如果`git commit` 提交到了版本库可以使用版本回退，在推送到远程分支之前。



2. 小节

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git restore t.txt`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git restore --staged t.txt`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库



### 删除文件

本地提交文件至版本库后，本地将文件删除。工作区和版本库状态不同，解决方法：

:one:确实要从版本库删除：  `git rm ` 然后 `git commit`

```bash
16280@jzh MINGW64 /e/test/learn_git (master)
$ git rm del.txt
rm 'del.txt'

16280@jzh MINGW64 /e/test/learn_git (master)
$ git commit -m "remove del.txt"
[master a1fe137] remove del.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 del.txt
```

:two:删错了，从版本库恢复：（ 根据文件名恢复 ）

```bash
git checkout -- del.txt
```

> 用版本库版本 替换 工作区版本



## 远程仓库

### 添加远程仓库

1. 本地仓库关联远程仓库

```bash
git remote add origin https://github.com/jzh000119/learngit.git
```

2. 查看远程库信息：

```bash
$ git remote -v
origin  https://github.com/jzh000119/learngit.git (fetch)
origin  https://github.com/jzh000119/learngit.git (push)
```

2. 取消远程仓库关联：

```bash
git remote remove origin
```

4. 推送内容到远程仓库

```bash
16280@jzh MINGW64 /e/test/learn_git (master)
$ git push -u origin master
```

> -u 参数：Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。 （**第一次推送分支的时候用**）

下次提交的时候，就可以直接提交：

```bash
git push origin master
```

5. 没有网络也可以进行 `git add / git commit` 在有网络的时候，`git push`即可。

6. `git://`使用 ssh；使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令。 



## 分支管理

### 创建与合并分支

```bash
git checkout -b dev
```

1. 列出所有分支：

```bash
git branch 
```

2. 在 dev 分支修改后，合并到 master 分支, master 执行如下指令

```bash
git merge dev
```

> git merge 用于合并指定分支到当前分支

3. 合并分支后，删除 dev 分支

```bash
git branch -d dev
```

4. 切换分支：

```bash
git switch dev
git checkout dev
```

5. 创建 + 切换分支：

```bash
git branch -b dev
git switch -c dev
```

### 解决冲突

1. 分支合并出现冲突：

```bash
git merge feature1
```

<img src="https://jzh119.oss-cn-beijing.aliyuncs.com/img/image-20221027100309774.png" alt="image-20221027100309774" style="zoom:67%;" />

+ 代表不同分支

2. 对于不同的位置，进行手动合并后，再进行提交即可。

```bash
16280@jzh MINGW64 /e/test/learn_git (master|MERGING)
$ git add .

16280@jzh MINGW64 /e/test/learn_git (master|MERGING)
$ git commit -m "conflict fixed"
[master fc6fefd] conflict fixed

16280@jzh MINGW64 /e/test/learn_git (master)
$
```

3. 查看分支的合并情况，显示为分支合并图

```bash
git log --graph --pretty=oneline --abbrev-commit
```

<img src="https://jzh119.oss-cn-beijing.aliyuncs.com/img/image-20221027101005285.png" alt="image-20221027101005285" style="zoom:67%;" />

4. 合并分支后，删除 feature1 分支

```bash
16280@jzh MINGW64 /e/test/learn_git (master)
$ git branch -d feature1 
Deleted branch feature1 (was 2863796).
```

### 分支管理策略

1. 合并分支时，Git 会用 `Fast forward` 模式，但是这种模式下，删除分支后，就会丢掉分支信息。
2. 强制禁用 `Fast forward`，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

```bash
git merge --no-ff -m "merge with no-ff" dev
```

<img src="https://jzh119.oss-cn-beijing.aliyuncs.com/img/image-20221027104603743.png" alt="image-20221027104603743" style="zoom:67%;" />

3. 合并分支时，加上 `--no-ff`参数就可以使用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而 `fast forward`合并就看不出来曾经做过合并。 （ 注意 commit 信息）

<img src="https://jzh119.oss-cn-beijing.aliyuncs.com/img/image-20221027105257260.png" alt="image-20221027105257260" style="zoom:67%;" />

> 红色箭头为添加了 `--no-ff`参数
>
> 绿色箭头为 `fast sorward`合并。

### BUG分支

1. 修复 bug 时，可以创建新的分支进行修复，然后合并，最后删除。
2. 当手头的工作没有完成时，先把工作现场 `git stash`一下，然后修复 bug ，修复后，再 `git stash pop`，回到工作现场。
3. 在 master 分支上修复的 bug，想要合并到 dev 分支，可以用 `git cherry-pick <commit>` 命令，把 bug 提交的修改 “复制到”当前分支，避免重复劳动。



### Feature分支

1. 如果新创建的分支，如果删除，将丢掉更改。强行删除，需要使用 `-D` 参数



### 多人协作

1. 推送到远程 dev 分支： `git push origin dev`

![image-20221030152025845](https://jzh119.oss-cn-beijing.aliyuncs.com/img/image-20221030152025845.png)

2. 拉取代码之前，要先建立本地分支与远程分支的链接：(关联分支)

```bash
16280@jzh MINGW64 /e/test/learn_git (dev)
$ git branch --set-upstream-to=origin/dev dev
branch 'dev' set up to track 'origin/dev'.
```



3. 拉取代码： `git pull`
4. 多人协作工作模式：

+ 首先，可以试图用 `git push origin <branch-name>` 推送自己的修改。
+ 如果推送失败，则因为远程分支比你本地的分支更新，需要先 `git pull` 试图合并。
+ 如果合并有冲突，则解决冲突，并在本地提交。
+ 没有冲突或者解决冲突后，再用 `git push origin <branch-name>`，推送就能成功！

### Rebase

主要是把 git 提交的历史变成一条干净的直线

## 标签管理

标签就是一个让人容易记住的有意义的名字，它跟某个 commit 绑在一起。

#### 创建标签

1. 新建标签：（最新的 commit 打上标签）

```bash
16280@jzh MINGW64 /e/test/learn_git (dev)
$ git tag v1.0

16280@jzh MINGW64 /e/test/learn_git (dev)
$ git tag
v1.0
```

2. 对历史打标签：

找到对应的 commit 号，然后打标签。

```bash
16280@jzh MINGW64 /e/test/learn_git (dev)
$ git tag v0.9 fc6fef

16280@jzh MINGW64 /e/test/learn_git (dev)
$ git tag
v0.9
v1.0
```

3. 指定标签信息：

```bash
git tag -a <tagname> -m "..." 
```

4. 查看所有标签：

```bash
git tag
```

5. 查看标签信息：

```bash
git show v0.1
```

#### 操作标签

1. 删除标签：`git tag -d v0.1`

2. 推送标签到远程：

```bash
git push origin v1.0 
git push origin --tags # 推送所有本地分支
```

3. 删除标签：

+ 如果推送到远程，就要先删除本地标签: 

```bash
git tag -d v0.9
```

+ 删除远程标签：

```bash
git push origin :refs/tags/v0.9
```



## Github

1. github 上，可以任意 Fork 开源仓库。
2. 自己拥有 Fork 后仓库的读写权限。
3. 可以推送 pull request 给官方的仓库来贡献代码。

## 自定义 Git

### 忽略特殊文件：

+ 忽略操作系统自动生成的文件，比如缩略图等；

+ 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的`.class`文件；

+ 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

> git 的根目录创建一个 .gitignore 文件，把要忽略的文件名填进去，git 就会自动忽略这些文件

+ 不排除文件：

```bash
!文件名
```

### 配置别名

1. 配置命令别名：

```bash
16280@jzh MINGW64 /e/test/learn_git (dev)
$ git config --global alias.br branch
```

> `--global`参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下 都有用。

2. 简写：

```bash
co = checkout
ci = commit
br = branch
st = status
```

3. 查看全局的配置信息：

![image-20221030171923986](https://jzh119.oss-cn-beijing.aliyuncs.com/img/image-20221030171923986.png)

```bash
16280@jzh MINGW64 /e/test/learn_git (dev)
$ cat ~/.gitconfig
[user]
        email = 675883210@qq.com
        name = jzh
[credential "https://git.hpcer.dev"]
        provider = generic
[color]
        ui = true
[alias]
        st = status
        co = checkout
        ci = commit
        br = branch
```

> 文件在 C:\Users\jzh 目录下
