# Git-Node

## 新建仓库

```cmd
mkdir learn-git
git init
\rm -rf .git #强制删除git仓库
git init my-repo #指定子文件夹目录名称并生成git目录
```



## 工作区域和状态

三个区域

* 工作区域：git所在目录
* 暂存区（索引区）：用于存放即将提交的临时存储区
* 本地仓库：git存储代码和版本信息的主要位置

![image-20250521140321339](./pic/image-20250521140321339.png)

四种文件

* 未跟踪
* 未修改
* 已修改
* 已暂存

![image-20250521141319055](./pic/image-20250521141319055.png)

## 添加和提交文件

``` cmd
echo "这是第一个文件">file.txt #linux下直接在命令行创建和修改文件
cat file1.txt #查看文件内容
git status #查看状态
```

截图中的file1.txt还是未被跟踪的状态

![image-20250521141733272](./pic/image-20250521141733272.png)

```cmd
git add file1.txt
git status
```

现在文件已经添加到了暂存区

![image-20250521141845875](./pic/image-20250521141845875.png)
```cmd
git rm --cached file1.txt
git status
```

现在文件又回到未跟踪的状态

![image-20250521142143740](./pic/image-20250521142143740.png)

```cmd
echo "test">file.sh
git add *.txt ##将所有以.txt为后缀的文件提交到暂存区里
git status
```

当所有文件都存到仓库里边后，git status会显示如下。

![image-20250521143412814](./pic/image-20250521143412814.png)

```cmd
git log
git log --oneline
```

可以使用git log命令查看提交记录。

![image-20250521143717700](./pic/image-20250521143717700.png)

## git reset

用于回退版本

```cmd
git reset --soft
git reset --hard
git reset --mixed
```

* soft：工作区和暂存区都不会清空
* hard：工作区和暂存区都会被清空
* mixed：工作区不会清空，但暂存区会被清空

![image-20250521145000870](./pic/image-20250521145000870.png)

先依次提交三个文件，然后再复制三份文件夹

```cmd
cp -rf repo repo-xxx	
```

![image-20250521145222246](./pic/image-20250521145222246.png)

```cmd
git log --oneline
git reset --soft 6c69b65 #加上回退的版本号
git lof --oneline
```

![image-20250521145431714](./pic/image-20250521145431714.png)

```cmd
git ls-files #查看暂存区的内容
```

回退到第二次提交的状态，此时暂存区和工作区的文件都存在，并且第三个文件未被提交

![image-20250521150014570](./pic/image-20250521150014570.png)

![image-20250521145857642](./pic/image-20250521145857642.png)

![image-20250521145905394](./pic/image-20250521145905394.png)

如下图所示，hard参数回退到第二个版本导致file3再工作区和暂存区都被删了

![image-20250521150159633](./pic/image-20250521150159633.png)

如下图所示，不加参数只写HEAD^是mixed回退，工作区保留，暂存区删除

![image-20250521150544227](./pic/image-20250521150544227.png)

**soft和mixed主要用于当有多个版本需要合并成一次提交**

若不小心用hard删除了本地的文件，可以使用reset回溯操作

```cmd
git reflog ##找到操作记录
git reset --hard ##回退到对应的操作之前的状态
```

![image-20250521151038402](./pic/image-20250521151038402.png)

## git diff

* 查看工作区，暂存区和仓库区之间的差异
* 查看不同版本之间的差异
* 查看不同分支之间的差异

git diff后面什么都不加的话默认比较的是工作区和暂存区之间的差异内容

红字表示删除内容，绿字表示添加内容

![image-20250521154257157](./pic/image-20250521154257157.png)

```CMD
git diff HEAD #查看暂存区和仓库之间的差异
git diff --chached #比较暂存区和仓库之间的差异
```

![image-20250521154404524](./pic/image-20250521154404524.png)

还可以通过ID比较不同版本之间的内容

```cmd
 git diff 1012b76 eb6293a
 git diff 1012b76 HEAD #HEAD表示最新提交的版本
```

![image-20250521155906911](./pic/image-20250521155906911.png)

可以使用如下方法快速比较新版本和上一个版本的不同

```cmd
git diff HEAD~ HEAD #head~表示最新版本的上一个版本
git diff HEAD^ HEAD #同上
git diff HEAD~2 HEAD #表示比较之前两个版本
git diff HEAD~3 HEAD file3.txt #也可以只显示某一个文件的差异内容
```

![image-20250521160124063](./pic/image-20250521160124063.png)

![image-20250521160456103](./pic/image-20250521160456103.png)

## git rm

```cmd
rm file1.txt #在工作区删除文件
git ls-files #查看暂存区中的内容
```

删除完file1.txt之后提示已删除，然后再add

![image-20250521160629562](./pic/image-20250521160629562.png)

![image-20250521161055277](./pic/image-20250521161055277.png)

```cmd
git rm file2.txt
```

直接用git rm 同时删除工作区和暂存区中的文件

![image-20250521161135139](./pic/image-20250521161135139.png)

## .gitignore

在这个文件中列出需要忽略的文件，这些文件就不会提交到仓库中

![image-20250521161416920](./pic/image-20250521161416920.png)

先创建两个log文件

![image-20250521161548204](./pic/image-20250521161548204.png)

```cmd
echo access.log > .gitignore
cat .gitignore
```

![image-20250521161643474](./pic/image-20250521161643474.png)

![image-20250521161717676](./pic/image-20250521161717676.png)

在将access.log添加到gitignore之后，git已经忽略这个文件

![image-20250521161738013](./pic/image-20250521161738013.png)

提交到仓库后没有access.log

![image-20250521161902338](./pic/image-20250521161902338.png)

可以手动修改.gitignore中的内容，添加*.log，这样可以忽略所有类型的log文件

再次添加log文件后直接忽略

![image-20250521162124812](./pic/image-20250521162124812.png)

**注意此时other.log已经在仓库中，若在本地区修改之后还是可以正常提交至仓库。需要将other.log从版本库中删掉**

![image-20250521162219348](./pic/image-20250521162219348.png)

```cmd
git rm --cached other.log #只删除仓库中的log而不删除工作区的log，不加cached直接删除工作区的文件
```

![image-20250521162527711](./pic/image-20250521162527711.png)

下图是先添加一个文件夹

![image-20250521162702794](./pic/image-20250521162702794.png)

![image-20250521162750068](./pic/image-20250521162750068.png)

在.gitignore中添加temp文件夹（注意以斜线结尾），这样就忽略了

![image-20250521162835491](./pic/image-20250521162835491.png)

![image-20250521162923200](./pic/image-20250521162923200.png)

![image-20250521163000712](./pic/image-20250521163000712.png)

## 关联本地仓库和远程仓库

```cmd
git remote add origin git@github.com:f66gh/TEST.git #第一个origin为仓库别称，第二个为线上仓库地址
git branch -M main #指定分支名称为main
git push -u origin master:main # -u表示上传，第一个master是本地的分支，第二个main是线上仓库的分支
```

![image-20250521194426950](./pic/image-20250521194426950.png)

可以使用git remote add 命令同时关联github和gitlab两个仓库

![image-20250521194730226](./pic/image-20250521194730226.png)

## VSCode

![image-20250521195818282](./pic/image-20250521195818282.png)

## 分支

```cmd
git branch #查看当前所有分支
git branch dev #新建dev分支	
git checkout dev #切换到dev分支
git switch master #切换到master分支
```

master分支工作区复制到dev上，dev工作区有新的文件

![image-20250522150520177](./pic/image-20250522150520177.png)

```cmd
git merge dev #将dev分支合并到当前分支（master）
git branch -d dev #删除dev分支，且dev是已经被合并的分支
git branch -D dev #强制删除dev分支
git checkout -b dev 61aaa56 #从dev2这个状态新建一个分支（和恢复指定状态的分支一个意思）
```

![image-20250522151150934](./pic/image-20250522151150934.png)

## 冲突

```cmd
git commit -a -m "feat:1" #对已存在的文件可以同时进行提交至暂存区和仓库
git commit -am "feat: 1" #上述命令行可简写为这个
```

![image-20250522152804175](./pic/image-20250522152804175.png)

可以用status查看当前状态，也可以用diff查看两个不同分支的不一致情况

![image-20250522152845905](./pic/image-20250522152845905.png)

应当手动修改冲突文件并再次提交

```cmd
git merge --abort #当合并出现冲突时可以终止合并
```

## rebase

如图所示，执行rebase之后所有分支都会合并成一个，从分支的最晚公共结点嫁接到rebase后面的分支

![image-20250522153459133](./pic/image-20250522153459133.png)

```cmd
alias graph="git log --oneline --graph --decorate --all" #alias是别名指令
```

![image-20250522154236673](./pic/image-20250522154236673.png)

```cmd
git reset --hard #将仓库版本回退到指定节点
```

![image-20250522154534041](./pic/image-20250522154534041.png)

```cmd
cp -rf branch-demo rebase1 # 复制文件
cp -rf branch-demo rebase2
git rebase master #将当前分支变基到master
```

![image-20250522154828386](./pic/image-20250522154828386.png)

![image-20250522155059020](./pic/image-20250522155059020.png)

![image-20250522155219399](./pic/image-20250522155219399.png)

![image-20250522155247968](./pic/image-20250522155247968.png)

 ## 工作流模型

![image-20250522155748187](./pic/image-20250522155748187.png)

