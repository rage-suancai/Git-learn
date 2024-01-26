## 基本命令介绍

### 创建本地仓库
我们可以将任意一个文件夹作为一个本地仓库 输入:

```shell
                        git init
```

输入后 会自动生成一个`.git`目录 注意这个目录是一个隐藏目录 而当前目录就是我们的工作目录

创建成功后 我们可以查看一下当前的一个状态 输入:

```shell
                        git status
```

如果已经成功配置为Git本地仓库 那么输入后可以看到:

```shell
                        On branch master

                        No commits yet
```

这表示我们还没有向仓库中提交任何内容 也就是一个空的状态

### 添加和提交
接着我们来看看 如何使用git来管理我们文档的版本 我们创建一个文本文档 随便写入一点内容 接着输入:

```shell
                        git status
```

我们会得到如下提示:

```shell
                        Untracked files:
                          (use "git add <file>..." to include in what will be committed)
                        	hello.txt
                        
                        nothing added to commit but untracked files present (use "git add" to track
```

其中`Untracked files`是未追踪文件的意思 也就是说 如果一个文件处于未追踪状态 那么git不会记录它的变化
始终将其当做一个新创建的文件 这里我们将其添加到暂存区 那么它会自动变为被追踪状态:

```shell
                        git add hello.txt # 也可以 add . 一次性添加目录下所有的
```

再次查看当前状态:

```shell
                        Changes to be committed:
                            (use "git rm --cached <file>..." to unstage)
	                            new file:   hello.txt
```

现在文件名称的颜色变成了绿色 并且是处于`Changes to be committed`下面 因此 我们的hello.txt现在已经被添加到暂存区了

接着我们来尝试将其提交到Git本地仓库中 注意需要输入提交的描述以便后续查看 比如你这次提交修改了或是新增了哪些内容:


```shell
                        git commit -m "Hello World"
```

接着我们可以查看我们的提交记录:

```shell
                        git log
                        git log --graph
```

我们还可以查看最近一次变更的详细内容:

```shell
                        git show [也可以加上commit ID查看指定的提交记录]
```

再次查看当前状态 已经是清空状态了:

```shell
                        On branch master
                        nothing to commit, working tree clean
```

接着我们可以尝试修改一下我们的文本文档 由于当前文件已经是被追踪状态 那么git会去跟踪它的变化 如果说文件发生了修改 那么我们再次查看状态会得到下面的结果:

```shell
                        Changes not staged for commit:
                            (use "git add <file>..." to update what will be committed)
                            (use "git restore <file>..." to discard changes in working directory)
                            	modified:   hello.txt
```

也就是说现在此文件是处于已修改状态 我们如果修改好了 就可以提交我们的新版本到本地仓库中:

```shell
                        git add .
                        git commit -m 'Modify Text'
```

接着我们来查询一下提交记录 可以看到一共有两次提交记录

我们可以创建一个`.gitignore`文件来确定一个文件忽略列表 如果忽略列表中的文件存在且不是被追踪状态 那么git不会对其进行任何检查:

```yaml
                        # 这样就会匹配所有以txt结尾的文件
                        *.txt
                        # 虽然上面排除了所有txt结尾的文件 但是这个不排除
                        !666.txt
                        # 也可以直接指定一个文件夹 文件夹下的所有文件将全部忽略
                        test/
                        # 目录中所有以txt结尾的文件 但不包括子目录
                        xxx/*.txt
                        # 目录中所有以txt结尾的文件 包括子目录
                        xxx/**/*.txt
```

创建后 我们来看看是否还会检测到我们忽略的文件

### 回滚
当我们想要回退到过去的版本时 就可以执行回滚操作 执行后 可以将工作空间的内容恢复到指定提交的状态:

```shell
                        git reset --hard commitID
```

执行后 会直接重置为那个时候的状态 再次查看提交日志 我们发现之后的日志全部消失了

那么要是现在我们又想回去呢? 我们可以通过查看所有分支的所有操作记录:

```shell
                        git reflog
```

这样就能找到之前的commitID 再次重置即可