# Git 笔记

`用于总结，不然看完啥也不做等于白嫖`


## 版本管理的演变

VCS 出现前
- 用目录拷贝区别不同的版本
- 公共文件容易被覆盖
- 成员沟通成本很高，代码集成效率低下

集中式 VCS
- 有集中2的版本管理服务器
- 具备文件版本管理和分支管理能力
- 集成效率有明显的提高
- 客户端必须时刻和服务器相连


分布是 VCS
- 服务端和客户端都有完整的版本库
- 脱离服务端，客户端照样可以管理版本
- 查看历史和版本比较等操作，都不需要访问服务器，比集中式 VCS 更能提高版本管理效率


Git 的特点
- 最优的存储能力
- 非凡的性能
- 开源的
- 很容易做备份
- 支持离线操作
- 很容易定制工作流程


## 安装 Git

链接： https://git-scm.com/book/zh/v2

```bash
git --version
```

## Git 小配置

配置 User.name 和 user.email

```bash
git config --global user.name "user.name"
git config --global user.email "user.email@email.com"
```

config 的三个作用

缺省等同于 local
```
git config --local      // 只有某个仓库有效
git config --global     // 对当前用户所有仓库有效
git config --system     // 对系统所有登录的用户有效
```

显示 config 的配置, 加 --list
```
git config --list --local      // 只有某个仓库有效
git config --list --global     // 对当前用户所有仓库有效
git config --list --system     // 对系统所有登录的用户有效
```

清除，--unset
```
git config --unset --local user.name
git config --unset --global user.name
git config --unset --system user.name
```

优先级： local > global > system


### 实践

请动⼿⽐⼀⽐，local 和 global 的优先级。
1. 在 Git 命令⾏⽅式下，⽤ init 创建⼀个 Git 仓库。
```
git init your_first_git_repo_name
```
2. cd 到 repo 中。
```
cd your_first_git_repo_name
```
3. 配置 global 和 local 两个级别的 user.name 和 user.email。
```
git config --local user.name 'your_local_name'
git config --local user.email 'your_local_email@.'
git config --global user.name 'your_global_name'
git config --global user.name 'your_global_eamil@.'
```
4. 创建空的 commit
```
git commit --allow-empty -m ‘Initial’
```
5. ⽤ log 看 commit 信息，Author 的 name 和 email 是什么？
```
git log
```


## 建 Git 仓库


两种⽅方式：
1. ⽤ Git 之前已经有项⽬目代码
```
cd 项⽬目代码所在的⽂文件夹
git init
```

2. 用 Git 之前还没有项⽬目代码
```
cd 某个⽂文件夹
git init your_project # 会在当前路路径下创建和项⽬目名称同名的⽂文件夹
cd your_project
```


## 给文件重命名的简便方法

```
mv readme readme.md
git add  readme.md
git rm readme

git status
git reset --hard

git mv readme readme.md
```


## 通过git log 查看版本演变历史

```
git log
git log --oneline
git log -n4 --oneline

git branch -v

git checkout -b temp 455jjk
git commit -am 'commit msg'

git log --all
git log --all --graph
git log --all --oneline
git log --all --oneline -n4
git log --all --oneline -n4 --graph
```

## gitk：通过图形界面工具来查看版本历史

```
gitk  # 会自动打开图形化界面
```


## 探密.git目录

```
cd .git
cat HEAD
git branch -av

cat config
vi .git/config
git config --local --list

cat master
git branch -av

```

```
ls -al

```


## commit、tree和blob三个对象之间的关系


![commit、tree和blob](./images/img1.jpg)

```
git cat-file -p xxx-id      # 可以一直往下去查询 
```


## 小练习：数一数tree的个数


![数⼀一数 tree 的个数](./images/img3.png)


```
find .get/objects -type -f
git cat-file -t xxx-id
git cat-file -p xxx-id
git commit -m 'msg'
find .get/objects -type -f
git cat-file -t xxx-id
git cat-file -p xxx-id
```


## 分离头指针情况下的注意事项

```
git checkout commit-id
修改某个文件
git commit -am 'msg'
git log
git checkout master
git branch <new-branch-name> 'commit-msg-id'    # 按照提示创建一个分支

```


## 进一步理解HEAD和branch

```
git branch -av

git checkout -b new-branch-name branchName-or-commitId

git log -n1

gitk --all

cat .get/HEAD

cat .get/refs/heads/branch-name     # 这里输出是一个 commit-id

get cat-fiel -t commit-id       # 这里输出的类型是 commit



git diff branch-name-1 branch-name-2

git diff HEAD HEAD~1
git diff HEAD HEAD^1
git diff HEAD HEAD^1^1 
git diff HEAD HEAD~2
```


## 怎么删除不需要的分支？

```
git branch -d branch-name

git branch -D branch-name       # 确认无误之后再执行
```


## 怎么修改最新commit的message

```
git commit -amend
```


## 怎么修改老旧commit的message

```
git rebase -i 目标commit-id的上一个id
```

还有后续步骤，按照提示即可


## 怎样把连续的多个commit整理成1个
```
git rebase -i commit-m

将要合并的 commit-commands 改为 squash(s)

填写合并说明

git log --graph

```


## 怎样把间隔的几个commit整理成1个

```
git rebase -i commit-m

移动要合并commit list 在一起

将要合并的 commit-commands 改为 squash(s)

填写合并说明

git log --graph

```


## 怎么比较暂存区和HEAD所含文件的差异

```
git diff --cached
```


## 怎么比较工作区和暂存区所含文件的差异

```
git diff
git diff -- <file> <file2>
```

## 如何让暂存区恢复成和HEAD的一样？

```
git reset HEAD
git reset HEAD fileName
git diff --cached       # 对比一下
```


## 如何让工作区的文件恢复为和暂存区一样？

```
git checkout -- <file>
```


## 怎样取消暂存区部分文件的更改？

```
git resst HEAD -- <file>
```

## 消除最近的几次提交

```
git reset --hard commit-id
```


## 看看不同提交的指定文件的差异

```
git diff branch-name branch-name -- <file>
git diff commit-id commit-id -- <file>
```


## 正确删除文件的方法

```
rm fileName

git rm fileName
```





常⽤用的传输协议

![常⽤用的传输协议](./images/img4.png)


哑协议与智能协议
- 直观区别：哑协议传输进度不可见；智能协议传输可见。
- 传输速度：智能协议⽐哑协议传输速度快。


备份特点

![备份特点](./images/img5.png)

