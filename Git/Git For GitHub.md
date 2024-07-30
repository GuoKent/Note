### Git For GitHub

初始化项目：`git init`

查看分支：`git branch`

切换分支：`git checkout <branch>`

查看代码更新状态：`git status`

克隆仓库：`git clone https://github.com/username/repository.git`

获取最新更改：`git pull`

> A 机器上传 github 后，B 机器还没更新，要先 pull 后再继续编辑



#### 代码提交流程：add 暂存区->commit 本地仓库->push 远程仓库

将文件存入暂存区：`git add .` or `git add filename`

提交更改：`git commit -m "some message"`

>存至本地仓库

连接远程仓库：` git remote add origin https://github.com/your-username/your-repository.git`

> 首次连接使用，连接一次即可

上传远程仓库：`git push -u origin main`

> 想传到哪个分支，要先 checkout 到对应分支，然后按照 "add 暂存区->commit 本地仓库->push 远程仓库"
>
> -u 代表合并/归并，若仓库本身有不一致的东西，可以用 -f 来覆盖**(会删除仓库原有的所有内容，慎用！！)**。首次提交需要 -u，后面直接`git push origin <branch>`
>
> 跨分支合并：`git push origin branch1:branch2`



#### 其他问题

##### 1.登录令牌 token 问题

```python
% git clone https://github.com/.../....git
Cloning into '...'...
Username for 'https://github.com': 3427870036@qq.com
Password for 'https://....com@github.com': 
remote: Support for password authentication was removed on August 13, 2021.
remote: Please see https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls for information on currently recommended modes of authentication.
fatal: Authentication failed for 'https://github.com/.../....git/'
```

原因：是因 [github](https://so.csdn.net/so/search?q=github&spm=1001.2101.3001.7020) 在 2020 年 7 月，宣布要求对所有经过身份验证的 Git 操作使用基于令牌的身份验证（例如，个人访问 GitHub 应用程序安装令牌）。从 2021年8月13 日开始，将在 GitHub.com 上对 Git 操作进行身份验证时不再接受帐户密码。

解决方法：[link](https://blog.csdn.net/qq_37255976/article/details/134558484)

##### 2.上传代码 hint 报错

```python
hint: Updates were rejected because the remote contains work that you do 
hint: not have locally. This is usually caused by another repository pushing 
hint: to the same ref. You may want to first integrate the remote changes 
hint: (e.g., 'git pull ...) before pushing again. hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

解决方法：[link](https://blog.csdn.net/qq_43265673/article/details/107392047)

##### 3.网络连接问题

```python
fatal: unable to access ‘https://github.com/.../.git‘: Recv failure Connection was rese
```

解决方法：[link](https://blog.csdn.net/qq_43546721/article/details/139506583) 使用方法一，取消代理（推荐）

```python
git config --global --unset http.proxy
git config --global --unset https.proxy
```



##### 长期有效 token

```python
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
```

