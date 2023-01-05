# Git

## 1.提交和查看

##### 1.1  git  init

在当前目录中创建Git管理仓库      .git

##### 1.2   git add 文件

将文件添加到暂存区（该仓库必须与   .git   文件必须在同一目录下。）

##### 1.3     git   commit   -m    “操作名”

将暂存区文件提交，并给提交命名。

##### 1.4   git   status

掌握工作区的状态。

##### 1.5    git   diff

查看修改内容，一般用在git status 显示有修改内容后使用。

##### 

### 2.版本回退

##### 2.1  git  reset  - -hard  commit_id

commit_id  是版本序号，输前几位即可。

Head指向当前版本，使用该命令可回到某次更改前的版本。

##### 2.2  git  log

查看提交历史，以便确定返回哪个版本。

##### 2.3   git   reflog

查看历史命令，以便确定回到哪个版本。

### 3.修改撤销

##### 3.1  git   restore

对工作区文件进行了修改，恢复其在版本区的内容。

#####  3.2  git  restored  - -staged   文件  

撤销在暂存区中的内容

##### 3.3 git   reset   HEAD

丢弃暂存区的修改，将文件返回工作区。

注：若已提交，最好返回之前的版本。

### 4.删除文件

##### 4.1git rm 文件

在工作区删除某个文件后，若确认删除且要在版本区删除 ，使用该指令并提交修改。

##### 4.2  git  restore  文件

在工作区删除某个文件后，若为误删，使用该指令恢复其在版本区的内容。

 ### 5.远程仓库Gitree

##### 5.1  git  remote  add origin  地址(  https   或者  git)

origin是给远程仓库指定的名字,此指令是将远程仓库和本地仓库连接起来。

##### 5.2 git  remote  em  origin

取消连接

##### 5.3 git  remote  -v

查看连接情况

##### 5.4  git  push  (-u)  origin   master

将本地仓库数据穿过去

-u 参数的作用是：第一次传输时使用，不仅可传输数据，还可把本地master分支与远程 master 分支串联起来。

#####5.5  git  clone  地址(https  或者  ssh)

将远程仓库中的文件复制到本地库中，二者必须已连接，ssh比https快捷。

### 6.分支操作

##### 6.1  git  branch

查看当前分支及指针位置。

##### 6.2 git branch   name

创建分支

##### 6.3  git  checkout/switch  name

切换分支

##### 6.4   git  switch  -c  name  或者  git  checkout  -b  name

创建加切换分支

##### 6.5 git merge name

合并某分支到当前分支

注1：当Git无法自动合并分支时，（master和其他分支修改内容不一致），必须先解决冲突，（将合并失败的文件手动编辑为我们需要的文件），其后再提交，自动合成。

##### 6.6 git merge  - -no-ff  -m  “名称”   name

注2：合并分支时，若采用极速模式，合并后删除分支会丢失掉分支信息，看不出来做过合并。而加上  - -no -ff 可采用普通模式，合并后有历史记录。注意要加操作名称。

##### 6.7 git  branch -d  name

删除分支

##### 6.8 git   log  - - graph

查看历史分支图

### 7.工作场操作

修改bug时，常常通过创建新的分支进行修复，而手头工作未完成时，可将此工作场保存，以后在取出来继续操作。

##### 7.1 git stash

保存当前工作场，可去进行其他操作。

##### 7.2 git  stash  list

查看保存的工作场

##### 7.3 git  stash  apply  内容

恢复某工作场

##### 7.4 git stash drop 内容

删除某保存的工作场‘

##### 7.5 git  stash  pop

恢复并删除某工作场

##### 7.6 git  cherry-pick  提交的操作序号

将之前的操作在当前分支下再进行一次。

 ### 8.注意事项

##### 8.1 git push origin 分支名

推送本地其他分支，若只推送master，则远程库里看不到其他分支。

##### 8.2 git  pull

推送分支时若推送失败，可能是别人推送的分支和你的冲突了，此时需要用该指令将远程库中的分支抓取下来。

##### 8.3  git  branch   - -set-upstream  分支名  远程库名/分支名

git  pull 过程中发生困难，应建立本地分支和远程分支的关联。

##### 8.4  git   checkout   -b  分支名   远程库名/分支名

切换到分支上同时与远程库中的分支关联起来。

##### 8.5  rebase  操作

将本地未push的分叉提交历史整理为直线。

### 9.标签

##### 9.1  git  tag  标签名   （指令名）

  用于新建标签，默认为HEAD，也可以指定一个指令。

#####9.2  git  tag  -a  标签名  -m “信息”

指定标签信息。

##### 9.3  git  tag

查看所有标签。

##### 9.4  git  show  标签名

显示标签信息。

##### 9.5 git  push  origin  标签名

推送一个标签

#####9.6 git  push  origin  - -tags

推送本地所有标签

注：标签均只存在于本地，只有推送才能进入远端。

##### 9.7  git  tag  -d  标签名

删除一个本地标签

##### 9.8  git  push  origin  :refs/tags/标签名

删除一个远程标签

### 10.忽略

##### 10.1   .gitignore

要忽略某些工作区文件时，需要编写  .gitignore文件并将其放在版本库中。

##### 10.2  git  add -f  文件名

添加某个忽略文件，也可以选择直接在  .gitignore 文件下编写  ！文件名

### 11.缩写

##### 11.1 git  config   - - global  alias.修改后的名字   修改前的名字

简写命令

##### 11.2 cat  .git/config  

查看针对每个仓库的命令简写。

##### 11.3 cat  .gitconfig

查看针对该用户的命令简写。
