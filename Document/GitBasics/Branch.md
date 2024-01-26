## 分支
分支将像我们树上的一个树枝一样 它可能一开始的时候是同一根树枝 但是长着长着就开始分道扬镳了 这就是分支 我们的代码也是这样 可能一开始写基础功能的时候使用的是单个分支
但是某一天我们希望基于这些基础的功能 把我们的项目做成两个不同方向的项目 比如一个方向做Web网站 另一个方向做游戏服务端

因此 我们可以在一个主干上分出N个分支 分别对多个分支的代码进行维护

### 创建分支
我们可以通过以下命令来查看当前仓库中存在的分支:

```shell
                        git branch
```

我们发现 默认情况下是有一个`master`分支的 并且我们使用的也是master分支 一般情况下master分支都是正式版本的更新
而其它分支一般是开发中才频繁更新的 我们接着来基于当前分支创建一个新的分支

```shell
                        git branch test
                        # 对应的删除分支是
                        git branch -d yyds
```

现在我们修改一下文件 提交 再查看一下提交日志:

```shell
                        git commit -a -m 'branch master commit'
```

通过添加`-a`来自动将未放入暂存区的已修改文件放入暂存区并执行提交操作 查看日志 我们发现现在我们的提交只生效于master分支 而新创建的分支并没有发生修改

我们将分支切换到另一个分支:

```shell
                        git checkout test
```

我们会发现 文件变成了此分支创建的时的状态 也就是说 在不同分支下我们的文件内容是相互隔离的。

我们现在再来提交一次变更 会发现它只生效在yyds分支上 我们可以看看当前的分支状态:

```shell
                        git log --all --graph
```

### 合并分支
我们也可以将两个分支更新的内容最终合并到同一个分支上 我们先切换回主分支:

```shell
                        git checkout master
```

接着使用分支合并命令:

```shell
                        git merge test
```

会得到如下提示:

```shell
                        Auto-merging hello.txt
                        CONFLICT (content): Merge conflict in hello.txt
                        Automatic merge failed; fix conflicts and then commit the result.
```

在合并过程中产生了冲突 因为两个分支都对hello.txt文件进行了修改 那么现在要合并在一起 到底保留谁的hello文件呢?

我们可以查看一下是哪里发生了冲突:

```shell
                        git diff
```

因此 现在我们将master分支的版本回退到修改hello.txt之前或是直接修改为最新版本的内容 这样就不会有冲突了 接着再执行一次合并操作 现在两个分支成功合并为同一个分支

### 变基分支
除了直接合并分支以外 我们还可以进行变基操作 它跟合并不同 合并是分支回到主干的过程 而变基是直接修改分支开始的位置
比如我们希望将yyds变基到master上 那么yyds会将分支起点移动到master最后一次提交位置:

```shell
                        git rebase master
```

变基后 yyds分支相当于同步了此前master分支的全部提交

#### 优选
我们还可以选择其将他分支上的提交作用于当前分支上 这种操作称为`cherrypick`:

```shell
                        git cherry-pick <commit id>:单独合并一个提交
```

这里我们在master分支上创建一个新的文件 提交此次更新 接着通过cherry-pick的方式将此次更新作用于test分支上: