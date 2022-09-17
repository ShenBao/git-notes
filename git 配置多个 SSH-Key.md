# Git配置多个SSH-Key

## 背景

当有多个git账号时，比如：

- 一个gitee，用于公司内部的工作开发；
- 一个github，用于自己进行一些开发活动；

## 解决方法

1. 生成一个公司用的SSH-Key

```bash
ssh-keygen -t rsa -C 'xxxxx@company.com' -f ~/.ssh/gitee_id_rsa
```

2. 生成一个github用的SSH-Key

```bash
ssh-keygen -t rsa -C 'xxxxx@gmail.com' -f ~/.ssh/github_id_rsa
```

3. 在 ~/.ssh 目录下新建一个config文件，添加如下内容（其中Host和HostName填写git服务器的域名，IdentityFile指定私钥的路径）

```bash
# gitee
Host gitee.com
HostName gitee.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/gitee_id_rsa
# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/github_id_rsa
```

4. 用ssh命令分别测试

```bash
ssh -T git@gitee.com
ssh -T git@github.com
```

## 配置两个不同的用户名和邮箱

可以在每个项目的 Git 配置中设置本地的 user.name 和 user.email。这样，每个项目都会使用与之关联的用户名和邮箱地址来提交代码。

### 全局配置

如果想要为所有的本地仓库设置一个默认的用户名和邮箱，可以使用以下命令：

```bash
git config --global user.name "Your Name For All Repos"
git config --global user.email "you@example.com"
```

### 本地配置

对于特定的仓库，你可以进入该仓库的目录，并使用以下命令来覆盖全局配置，为这个特定的仓库设置用户名和邮箱：

```bash
cd /path/to/your/repo
git config user.name "Your Name For This Repo"
git config user.email "you@thisrepo.example.com"
```

### 自动化配置

如果不想每次都手动设置，可以通过创建或编辑 ~/.gitconfig 文件来根据不同的远程仓库自动设置用户名和邮箱。在 .gitconfig 文件中添加 [includeIf "gitdir:..."] 指令，指向特定仓库的配置文件。例如：

```bash
[includeIf "gitdir:~/projects/work/"]
    path = ~/projects/work/.gitconfig
[includeIf "gitdir:~/projects/personal/"]
    path = ~/projects/personal/.gitconfig
```

然后，在 ~/projects/work/.gitconfig 和 ~/projects/personal/.gitconfig 中分别定义适用于工作和个人项目的用户名和邮箱：

~/projects/work/.gitconfig
```bash
[user]
    name = Your Work Name
    email = your.work.email@example.com
```

~/projects/personal/.gitconfig
```bash
[user]
    name = Your Personal Name
    email = your.personal.email@example.com
```

这样做之后，当你在一个位于 ~/projects/work/ 下的仓库中时，Git 会自动应用工作相关的用户名和邮箱；而在 ~/projects/personal/ 下的仓库中，则会应用个人相关的用户名和邮箱。

