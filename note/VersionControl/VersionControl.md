## Version Control
如有任何疑问，请阅读讲义或看git的官方文档，都有详细的讲解

### Inventing version control
#### A list of oeprations that should be supported by a version control scheme
* reverting to a past version
* comparing two different versions
* pushing full version history to another location
* pulling history back from that location
* **merging** versions that are offshoots of the same earlier version
#### Multiple developers
logs: We can look at the log for just a particular file: a view of the log restricted to those changes that involved modifying some file. We can also use the log to figure out which change contributed each line of code, or, even better, which person contributed each line.
#### Multiple branches
It sometimes makes sense for a subset of the developers to go off and work on a branch, a parallel code universe for, say, experimenting with a new feature. 
#### Distributed vs. centralized
#### Sum——features of a version ctrl sys
* reliable, multiple files, meaningful versions, revert, compare version, review history, not just for code
* merge, track responsiblities, work in parallel

### Git
#### git website
有简中翻译
git commands:https://git-scm.com/docs 
git web:https://git-scm.com/
"what is git" read this chapter carefully(如git不同于其它一些版本控制工具：是快照流而非基于差异)


#### git object graph
The history of a Git project is a directed acyclic graph (DAG). The history graph is the backbone of the full object graph stored in .git, so let’s focus on it for a minute.
Each node in the history graph is a commit a.k.a. version a.k.a. revision of the project: a complete snapshot of all the files in the project at that point in time. 
详情阅读讲义原文


有一些命令可以查看版本历史（比如做了多少次修改，哪些文件被删了，谁合并了文件），用desktop也能做到类似功能。需要结合graph来理解这些操作。



#### add
add from working directory to staging area, then commit
也就是说，更改之后staging area里面不会自动帮你改好
note: 
1. 百分之九十九的时间里，别把源程序的exe文件add上去，因为它们不可读，难以管理。在git里有一个隐藏文件.gitignore  to specify files or directories that should be ignored by version control
2. 多用 git status 查看当前add和commit的状态
3. git reverse：undo the commits


#### push and pull
After we commit, we can send new commits to a remote repo by using `git push`
And we receive new commits using `git pull`. In addition to fetching new parts of the object graph, git pull also updates the working copy by checking out the latest version. If the remote repository and the local repository have both changed, git pull will try to merge those changes together.


#### Merge
merge的工作流程大概如下：A和B同时从远程仓库克隆，各自作了更改，A首先push，B commit之后没办法push（被git拒绝，因为可能会覆盖A的工作）。这时B需要先pull一次，这里的pull会同时更改B的工作区和commit的历史。如果两个人修改的地方不重合，那么B的pull会在commit中merge两个人的修改历史，然后B就可以push了；但如果重合，那么就会report a merge conflict, 此时两个人需要协商后再push。


#### git show
查看commit历史中文件的具体信息