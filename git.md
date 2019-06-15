# git

## git 基础

记录每次更新
```bash
git add     //添加改变到暂存区。
git commit -m ''    //提交暂存的文件快照。
git commit -a -m ''     //合并添加改变和提交暂存两个操作。
git rm file     //删除文件
git rm \*~      //查看当前目录及其子目录中所有～结尾的文件
git mv file     //重命名文件
```

log日志文件
```
git log     //会按时间列出所有的更新，最近的更新排在最上面。
git log —since=2.weeks

git log -p -2   //会列出最近两次的更新
```

撤销操作
```
git commit —amend       //补齐上一次提交，可以修改message，也可以添加一些遗漏的文件。
git reset HEAD file     //取消暂存file文件。
git unstage file        //同上，取消暂存file文件
git checkout — file     //取消file的修改
```

打标签：//为特定时间的版本打上标签
```
git tag     //列选已有的标签
git tag -l 'v1.4.*'     //列选v1.4系列标签
git show v1.4       /*git show命令查看相应标签的版本信息，并连通现实打标签式的提交对象*/
```

//两种标签：
```
git tag -a v1.4 -m 'my version 1.4'     //带标注的标签
git tag v1.4_a  //轻量级标签
```

//签署标签：
```
git tag -s v1.5 -m 'my signed 1.5 tag'      //添加签署标签
git tag -v v1.5     /*此方式用于验证已经签署的标签，毁掉用GPG来验证签名，所以需要有签署者的公钥，存放在keyring中，才能验证。*/
```

//共享标签：
```
git push origin v1.5    /*git push 并不会把标签传送到远程服务器上，只有通过显示命令才能分享标签到远程仓库*/
git push origin —tags   //传送所有标签
```

//删除标签
```
git tag -d v1.0     //删除本地标签
git push origin :refs/tags/v1.0     //删除远程标签
```

## 远程仓库：

添加删除远程仓库
```
git remote add origin path      /*添加远程仓库，origin为远程仓库名字（此处pb代表程仓库地址，clone克隆操作会自动使用默认的master和origin名字）*/
git remote rm origin    //删除远程仓库
git remote      //显示远程仓库的名字
git remote -v       //显示远程仓库的地址
```

数据下载
```
git fetch origin     /*抓取远程仓库中本地仓库所没有的信息，但只是讲远程的数据拉到本地仓库，并没有自动合并到当前工作分支，只有当你去是最好准备，才能手动合并。*/
git pull origin
```

数据上传
```
git push origin master      //可以将本地仓库中的数据推送到远程仓库。
git remote show origin      //查看某个远程仓库的详细信息。
git remote rename someRepo anotherRepo      //修改某个远程仓库在本地的简称。
```


## git分支：

创建新分支以及切换分支：
```
/*在切换分支的时候，要注意确保暂存区或者工作目录里，那些还没有提交的修改，他会   
和你即将检出的分支产生冲突从而阻止git为你切换分支。切换分支的时候最好保持一个清
洁的工作区域。*/

git branch testing      //这会在当前commit对象上新建一个分支指针。
git branch -d testing       //删除testing分支
git checkout testing        //切换到testing分支上
git checkout -b iss53   /*同时也可以将上面两条指令合并，即在创建分支的同时生成一个新的分支。*/
```

合并分支
```
git merge someBranch
```

Fast forward：由于当前master分支所在的提交对象是要并入分支的直接上游，Git只需要把master分支指针直接右移即可。

分叉处在不在两个分支的末尾处，此时git会用分支的末端以及他们的共同祖先以及他们的共同祖先进行一次简单的三房合并计算。这个提交对象比较特殊，他有两个祖先。值得一体的是Git可以自己裁决哪个共同祖先才是最佳合并基础。

分支管理
```
git branch      //查看所有分支
git branch -v       //查看各个分支最后一个提交对象的
git branch —merger      //查看那些分支已被并入当前分支
git branch —no-merger       //查看哪些分支尚未合并的工作
```

Git创建空白新分支
```
git branch <new_branch>
git checkout <new_branch>
git rm --cached -r . 
git clean -f -d

git commit --allow-empty -m "[empty] initial commit"

git push origin <new_branch>
```
