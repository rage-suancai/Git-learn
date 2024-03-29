## 使用IDEA版本控制
虽然前面我们基本讲解了git的命令行使用方法 但是没有一个图形化界面 始终会感觉到很抽象 所以这里我们使用IDEA来演示
IDEA内部集成了git模块 它可以让我们的git版本管理图形化显示 当然除了IDEA也有一些独立的软件比如: SourceTree(挺好用)

打开IDEA后 找到版本控模块 我们直接点击创建本地仓库 它会自动将当前项目的根目录作为我们的本地仓库 而我们编写的所有代码和项目目录下其他的文件都可以进行版本控制

我们发现所有项目中正在编写的类文件全部变红了 也就是处于未追踪状态 接着我们进行第一次初始化提交 提交之后我们可以在下方看到所有的本地仓库提交记录

接着我们来整合一下Web环境 创建新的类之后 IDEA会提示我们是否将文件添加到Git 也就是是否放入暂存区并开启追踪 我们可以直接对比两次代码的相同和不同之处

接着我们来演示一下分支创建和分支管理

### 远程仓库
远程仓库实际上就是位于服务器上的仓库，它能在远端保存我们的版本历史，并且可以实现多人同时合作编写项目
每个人都能够同步他人的版本 能够看到他人的版本提交，相当于将我们的代码放在服务器上进行托管。

远程仓库有公有和私有的 公有的远程仓库有GitHub, 码云, Coding等 他们都是对外开放的 我们注册账号之后就可以使用远程仓库进行版本控制
其中最大的就是GitHub 但是它服务器在国外 我们国内连接可能会有一点卡 私有的一般是GitLab这种自主搭建的远程仓库私服 在公司中比较常用 它只对公司内部开放 不对外开放

这里我们以GitHub做讲解 官网: https://github.com 首先完成用户注册

#### 远程账户认证和推送
接着我们就可以创建一个自定义的远程仓库了

创建仓库后 我们可以通过推送来将本地仓库中的内容推送到远程仓库

```shell
                        git remote add 名称 远程仓库地址
                        git push 远程仓库名称 本地分支名称[:远端分支名称]
```

注意`push`后面两个参数 一个是远端名称 还有一个就是本地分支名称 但是如果本地分支名称和远端分支名称一致 那么不用指定远端分支名称 但是如果我们希望推送的分支在远端没有同名的
那么需要额外指定 推送前需要登陆账户 GitHub现在不允许使用用户名密码验证 只允许使用个人AccessToken来验证身份 所以我们需要先去生成一个Token才可以

推送后 我们发现远程仓库中的内容已经与我们本地仓库中的内容保持一致了 注意 远程仓库也可以有很多个分支

但是这样比较麻烦 我们每次都需要去输入用户名和密码 有没有一劳永逸的方法呢? 当然 我们也可以使用SSH来实现一次性校验 我们可以在本地生成一个rsa公钥:

```shell
                        ssh-keygen -t rsa
                        cat ~/.ssh/github.pub
```

接着我们需要在GitHub上上传我们的公钥 当我们再次去访问GitHub时 会自动验证 就无需进行登录了 之后在Linux部分我们会详细讲解SSH的原理

接着我们修改一下工作区的内容 提交到本地仓库后 再推送到远程仓库 提交的过程中我们注意观察提交记录

```shell
                        git commit -a -m 'Modify files'
                        git log --all --oneline --graph
                        git push origin master 
                        git log --all --oneline --graph
```

我们可以将远端和本地的分支进行绑定 绑定后就不需要指定分支名称了

```shell
                        git push --set-upstream origin master:master
                        git push origin
```

在一个本地仓库对应一个远程仓库的情况下 远程仓库基本上就是纯粹的代码托管了(云盘那种感觉 就纯粹是存你代码的)

#### 克隆项目
如果我们已经存在一个远程仓库的情况下 我们需要在远程仓库的代码上继续编写代码 这个时候怎么办呢?

我们可以使用克隆操作来将远端仓库的内容全部复制到本地:

```shell
                        git clone 远程仓库地址
```

这样本地就能够直接与远程保持同步

#### 抓取, 拉取和冲突解决
我们接着来看 如果这个时候 出现多个本地仓库对应一个远程仓库的情况下 比如一个团队里面 N个人都在使用同一个远程仓库
但是他们各自只负责编写和推送自己业务部分的代码 也就是我们常说的协同工作 那么这个时候 我们就需要协调

比如程序员A完成了他的模块 那么他就可以提交代码并推送到远程仓库 这时程序员B也要开始写代码了 由于远程仓库有其他程序员的提交记录
因此程序员B的本地仓库和远程仓库不一致 这时就需要有先进行pull操作 获取远程仓库中最新的提交

```shell
                        git fetch 远程仓库 # 抓取: 只获取但不合并远端分支 后面需要我们手动合并才能提交
                        git pull 远程仓库 # 拉取: 获取+合并
```

在程序员B拉取了最新的版本后 再编写自己的代码然后提交就可以实现多人合作编写项目了 并且在拉取过程中就能将别人提交的内容同步到本地 开发效率大大提升

如果工作中存在不协调的地方 比如现在我们本地有两个仓库 一个仓库去修改hello.txt并直接提交 另一个仓库也修改hello.txt并直接提交 会得到如下错误:

                        To https://github.com/xx/xxx.git
                        ! [rejected]        master -> master (fetch first)
                        error: failed to push some refs to 'https://github.com/xx/xxx.git'
                        hint: Updates were rejected because the remote contains work that you do
                        hint: not have locally. This is usually caused by another repository pushing
                        hint: to the same ref. You may want to first integrate the remote changes
                        hint: (e.g., 'git pull ...') before pushing again.
                        hint: See the 'Note about fast-forwards' in 'git push --help' for details.

一旦一个本地仓库推送了代码 那么另一个本地仓库的推送会被拒绝 原因是当前文件已经被其它的推送给修改了 我们这边相当于是另一个版本 和之前两个分支合并一样 产生了冲突 因此我们只能去解决冲突问题

如果远程仓库中的提交和本地仓库中的提交没有去编写同一个文件 那么就可以直接拉取:

```shell
                        git pull 远程仓库
```

拉取后会自动进行合并 合并完成之后我们再提交即可

但是如果两次提交都修改了同一个文件 那么就会遇到和多分支合并一样的情况 在合并时会产生冲突 这时就需要我们自己去解决冲突了

我们可以在IDEA中演示一下 实际开发场景下可能会遇到的问题

至此 Git版本控制就讲解到这里