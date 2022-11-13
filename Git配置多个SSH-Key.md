# Git配置多个SSH-Key

## 背景

当有多个git账号时，比如：

a. 一个gitee，用于公司内部的工作开发；
b. 一个github，用于自己进行一些开发活动；

## 解决方法

1. 生成一个公司用的SSH-Key

```
ssh-keygen -t rsa -C 'xxxxx@company.com' -f ~/.ssh/gitee_id_rsa
```

2. 生成一个github用的SSH-Key

```
ssh-keygen -t rsa -C 'xxxxx@gmail.com' -f ~/.ssh/github_id_rsa
```

3. 在 ~/.ssh 目录下新建一个config文件，添加如下内容（其中Host和HostName填写git服务器的域名，IdentityFile指定私钥的路径）

```
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

```
ssh -T git@gitee.com
ssh -T git@github.com
```


