## Git

### 创建版本库

版本库（仓库）即是一个Git可以管理的目录，对其中每一个文件的操作都可以被追踪，以便于在将来还原。

```shell
#创建仓库分为两步
##创建一个文件夹
mkdir fold
##进入文件夹，初始化变成Git可管理的仓库
git init
```

**把文件添加到版本库**

**注意：所有版本控制系统都只能跟踪文本文件的改动，二进制文件的修改是无法知道的。**

```shell
#将文件放到Git仓库里只需两步
##一、把文件添加到仓库
git add readme.txt
##二、把文件提交到仓库
git commit -m "add the readme.txt"
```

### 版本控制

当修改readme.txt文件后，可以使用`git status`查看仓库当前状态：

```shell
git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
	modified:   readme.txt
no changes added to commit (use "git add" and/or "git commit -a")
```

使用`git diff`查看修改内容：

```shell
git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
...
```

下一步使用`git add`和`git commit`提交修改。

#### 版本回退

Git作为版本控制工具，为文件修改的各个版本提供了快照，这个快照被称为`commit`，我们可以从`commit`中恢复之前的文件版本。

`git log`则为我们提供了之前各个版本的相关信息以便选择回退的版本，使用--pretty=oneline可以精简输出。

```shell
git log
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
...
commit e475afc93c209a690c39c13a46716e8fa000c366
...
```

Git中当前版本可以用`HEAD`标识，上一个版本为`HEAD^`，前n个版本为`HEAD~n`。回退到之前的版本就可以用`git reset`，从过去回到现在也使用`git reset`

```shell
#回到过去
git reset --hard HEAD^
HEAD is now at ...
#回到未来
git reset --hard [版本号]
HEAD is now at ...
```

到达过去版本时，`git log`已无法查看更新的版本信息，因此Git中还提供了一个用来记录每一次操作的命令`git reflog`，使用这个命令可以获取到每一次commit的版本号。

#### 工作区与暂存区

本机目录就是工作区，工作区中有一个隐藏目录`.git`这个就是Git的版本库。

Git的版本库中有被称为stage或index的暂存区，及Git自动创建的第一个分支`master`及指向`master`的一个指针`HEAD`。

![git-repo](./images/repo.png)

在版本控制的两步中：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

#### 管理修改

Git跟踪并管理的是修改，而非文件。

**每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。**

1. 修改了工作区文件的内容，想直接丢弃工作区的修改时，使用`git checkout -- file`

2. 修改了工作区文件的内容还添加到了暂存区中，使用`git reset HEAD file`，清除暂存区中的修改，再按1清除工作区的修改
3. 提交到版本库里时，使用版本回退。

误删文件可以使用`git checkout`恢复：

```shell
git checkout -- test.txt
```

**`git checkout`其实是用版本库里的版本替换工作区的版本**

### 远程仓库

#### 添加远程库

Git的优势就在于可以使用github建立远程库作为本地库的副本。

```shell
#在本地使用地址添加远程库，别名为origin
git remote add origin git@github.com:timgreat/gulimall.git
#把本地库的所有内容推送到远程库origin的main分支上
##参数-u代表第一次推送把本地的main分支与远程的master分支关联起来
git push -u origin main
#查看远程库信息
git remote -v
#删除远程库（解除本地库与远程库的绑定信息）
git remote rm origin
```

#### 从远程库克隆

从头开发一个项目，最好的方式就是先创建远程库，再从远程克隆到本地。

```shell
#从远程库克隆到本地
git clone git@github.com:timgreat/cs61b.git
```

### 分支管理

分支可以创建不同的工作流，到开发完成时可以合并各个分支。这样每条分支间不受对方影响，可以大大提升开发效率。

#### 创建分支与合并分支

`main`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

```ascii
                  HEAD
                    │
                    │
                    ▼
                   master
                    │
                    │
                    ▼
┌───┐    ┌───┐    ┌───┐
│   │───>│   │───>│   │
└───┘    └───┘    └───┘
```

每次提交，`master`分支都会向前移动一步。

当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

```ascii
                   master
                    │
                    │
                    ▼
┌───┐    ┌───┐    ┌───┐
│   │───>│   │───>│   │
└───┘    └───┘    └───┘
                    ▲
                    │
                    │
                   dev
                    ▲
                    │
                    │
                  HEAD
```

切换分支后，对工作区的修改就是针对新的分支了：

```ascii
                  master
                    │
                    │
                    ▼
┌───┐    ┌───┐    ┌───┐    ┌───┐
│   │───>│   │───>│   │───>│   │
└───┘    └───┘    └───┘    └───┘
                             ▲
                             │
                             │
                            dev
                             ▲
                             │
                             │
                           HEAD
```

当完成新分支上的工作后，就可以把新分支合并到主分支上，删掉新的分支：

```ascii
                           HEAD
                             │
                             │
                             ▼
                            master
                             │
                             │
                             ▼
┌───┐    ┌───┐    ┌───┐    ┌───┐
│   │───>│   │───>│   │───>│   │       
└───┘    └───┘    └───┘    └───┘
                             ▲
                             │
                             │
                            dev
              
                           HEAD
                             │
                             │
                             ▼
                            master
                             │
                             │
                             ▼
┌───┐    ┌───┐    ┌───┐    ┌───┐
│   │───>│   │───>│   │───>│   │
└───┘    └───┘    └───┘    └───┘
```

具体流程：

1. 创建dev分支，在切换到该分支上：

```shell
#创建分支
git branch dev
#切换到分支
git checkout dev
#以上两步合并为一条命令
git checkout -b dev
#查看当前分支
git branch
```

2. 将新分支合并到主分支上：

```shell
#合并分支
git merge dev
#删除分支
git branch -d dev
```

**tips**

切换分支常使用`git switch`

```shell
#创建并切换到新的分支
git switch -c dev
#直接切换到已有分支
git switch master
```

#### 解决冲突

当在不同分支上同时修改了一个文件时，merge会发生冲突，此时需要手动修改冲突部分，重新提交。

例如，当在两个不同分支`master`与`feature1`上修改了同一个文件并`git commit`时，分支结构：

```ascii
                            HEAD
                              │
                              ▼
                           master
                              │
                              ▼
                            ┌───┐
                         ┌─>│   │
┌───┐    ┌───┐    ┌───┐  │  └───┘
│   │───>│   │───>│   │──┤
└───┘    └───┘    └───┘  │  ┌───┐
                         └─>│   │
                            └───┘
                              ▲
                              │
                          feature1
```

此时，`git merge`将会产生冲突，需修改后再提交，最后删除`feature1`分支。

```ascii
                                     HEAD
                                       │
                                       ▼
                                    master
                                       │
                                       ▼
                            ┌───┐    ┌───┐
                         ┌─>│   │───>│   │
┌───┐    ┌───┐    ┌───┐  │  └───┘    └───┘
│   │───>│   │───>│   │──┤             ▲
└───┘    └───┘    └───┘  │  ┌───┐      │
                         └─>│   │──────┘
                            └───┘
                              ▲
                              │
                          feature1
```
