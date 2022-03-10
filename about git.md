# 初始化一个Git仓库
```bash
git init
```
# 添加文件到Git仓库，分两步：
 1. 使用命令 ` git add <file>` 可以添加多个文件；
 2. 使用命令 `git commit -m <message>`，完成。
# 查询工作区的状态 
```bash
git status
```
# 如果git status告诉你有文件被修改过，可以查看修改内容
```bash
get diff
```
# 显示文件日志
较为完全的显示
```bash
git log
```
简单的显示
```bash
git log --pretty=oneline
```
# 返回文件的某个版本
```bash
git reset --hard HEAD^
```
## 关于我输入`git reset --hard HEAD^`但是却跳出来more?的问题
- 原因，cmd控制台中换行符默认是\^，而不是\
- 所以相当于问你是否需要继续输入
- 解决方法有如下几种：
1. 加引号：`git reset --hard "HEAD^"`
2. 加一个\^：`git reset --hard HEAD^^`
3. 换成\~：`git reset --hard HEAD~` 或者 `git reset --hard HEAD~1`
~ 后面的数字表示回退几次提交，默认是一次，如果是回退回前100个版本也就不必要输出100个^

# window cmd 下查看文件内容
```bash
type readme.txt
```
linux 下直接`cat readme.txt`即可
# 查看某个版本的id
查看历史命令，以确定要回到哪个历史版本
```bash
git reflog
```

# git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区
```bash
git reset
```
# 注意区别暂存区和工作区（本条无内容）
# 可以丢弃工作区的修改
```bash
git checkout -- file
```
# 仅添加到暂存区的时候，注意判断一下状态
```bash
git status
```
# 将暂存区中的内容删除
```bash
git reset HEAD <file>
```
# 提交路径理解
工作区 ->暂存区->commit
# 删除文件
1.手动删除
2.`rm <filename>`
3.`del <filename>`

执行完上述操作之后还需要`git rm`和`git commit`一下,注意无论是del还是rm这一步都是`git rm`
```bash
git rm <filename>
```
```bash
git commmit -m "修改注释"
```
# 创建sshkey并添加到github或者是gitlab
```bash
 ssh-keygen -t rsa -C "youremail@example.com"
```
然后疯狂回车。
会显示创建的.ssh在哪个目录下，打开后复制id_rsa中的内容到github或者是gitlab当中

# 建立与远程库的链接（例如github或者是gitlab）
```bash
git remote add <name> git@github.com:xxx/name.git
```
```bash
git remote add <name> git@gitlab.com:xxx/name.git
```

## 推送本地文件
下一步，就可以把本地库的所有内容推送到远程库上：
```bash
git push -u <name> master
```
关联一个远程库时必须给远程库指定一个名字，即\<name>处需要指定名字，origin是默认习惯命名；
- 关联后，使用命令git push -u origin master第一次推送master分支的所有内容；

关联后,可以直接推送最新的修改
```bash
git push <name> master
```
## 相关问题解决
输入
```bash
$ git push -u origin master
```
出现
```bash
git@gitlab.com: Permission denied (publickey,keyboard-interactive).
fatal: Could not read from remote repository.
```
报错。
我的错误原因是，前面ssh的链接错误，导致无法连接
解决办法：
![在这里插入图片描述](https://img-blog.csdnimg.cn/9c8c2309d43e4383964e2b2ddfa4f301.png)
打开你的项目查看SSH,把之前输错的删除，即`get remote rm <name>`
```bash
git remote add origin <shang shu ssh>
```
如果还是不行，那就打开当前目录下的.git文件夹，然后用记事本打开config，并将里面的url修改为http格式的，例如
![在这里插入图片描述](https://img-blog.csdnimg.cn/20347b383205458c854478ba4f6f3591.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_20,color_FFFFFF,t_70,g_se,x_16)
# 删除远程库
根据名字删除，比如删除origin（然后再查看一下远程库的情况）
```bash
git remote rm origin
git remote -v
```
# 创建和合并分支
第一步建立分支
第二步切到该分支
```bash
git branch dev
git checkout dev
```
可以化简成为一个步骤
```bash
git checkout -b dev
```
`git checkout`与前面切换撤销修改的命令是同一个，即一个命令具有两个功能
切换到另一个分支也可以用更为合理的 `switch`
- 创建并切换到一个新的分支
```bash
 git switch -c dev
```
- 直接切换到一个已有的分支
```bash
git switch master
```

# 查看当前分支状态
```bash
git branch
```
# 将文件修改上传
```bash
$ git add <filename>
$ git commit -m "注释"
```
# 合并分支
```bash
git merge <分支名字>
```
该命令的作用的是合并指定分支到当前分支
# 删除分支
```bash
git branch -d <name>
git branch
```

