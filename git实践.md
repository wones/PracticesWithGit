# Git实践
* 本地Git操作
* 远程Git操作--Github
* Git 分支
* .gitignore忽略文件
## 本地Git操作
### 基本配置
#### 1.初始化仓库
```git
git init
```
#### 2.配置用户名和邮件
```git
git config --global user.name "tom"
git config --global user.email tom@qq.com
#查看配置， --list 可以查看
git config --list --show-origin
git config user.name
```
#### 3.添加文件
```git
# 主要使用.和*进行匹配
git add .  #将所有目录下的文件进行跟踪
git add *.txt
```
添加文件后git会对添加文件进行追踪，追踪的参数包括修改，删除。
#### 4.查看仓库状态
```git
git status
git status -s #获得更简洁的报告
```
    A：表示被追踪的文件
    M：文件被修改，
    ？？：新添加的未跟踪的文件
#### 5.提交版本
```git
git commit -m "message"
```
如果不添加 -m参数在win上会弹出安装时选择的文本编辑器进行提交信息的编写
#### 6.获取帮助
-h参数，git子命令查询帮助
```git 
#获取add命令帮助说明
git add -h
```
#### 7.获取提交历史
```git
git log
```
log信息会按照提交时间的先后列出
主要的参数包括:

    -p/–patch -num（定义显示最近的几条据）
### 撤销操作
* 重新提交
```git
git commit --amend #该方法会替代上一次的提交信息
```
* 取消暂存的文件
```git
git reset HEAD xxx(文件)
```
* 撤销对文件的修改
```git
git checkout -- <file>
```
注意：git chekcout 是让文件回到最近一次该文件git commit或git add时的状态。
### 打标签
```git
# 查询标签
git tag -l "标签"
# 创建标签，如果不使用-a参数 进行版本信息查询时就看不到额外的标签信息
git tag -a v1.1 -m "message"
# 查看版本信息
git show v0.1
# 给历史版本打标签
git tag -a v1.2 9fcebb2(部分校验和)
# 删除标签 -d
git tag -d v1.4
如果使用远端服务器，标签需要另外push
```
### Git别名
```git
git config --global alias.br branch
git config --global alias.st status
```
## 远程Git操作--Github
多次本地git库关联到github出现错误后，建议构建先创建远端的github仓库，使用git clone 仓库名命令克隆至本地，再进行git的使用。
* 首先查看本地git仓库是否配置远端服务器
```git
git remote
```
如果是克隆的仓库，可以看到origin（git给克隆仓库服务器的默认名字）
**可以存在不止一个远端url**
* 添加远程仓库
```git
# pb 别名 
git remote add pb https://github.com/paulboone/tic.git
# 获取仓库信息
git fetch pb
必须注意 git fetch 命令只会 将数据下载到你的本地仓库——它并不会自动合并或修改你当前的工作
```
* 推送到远程仓库
```git
# 推送到origin的master分支
git push origin master
# 展示远程仓库的url和跟踪分支信息，列出了执行git push会自动推送的远程分支
git remote show origin
```
* 远程仓库的重命名与移除
```git
git remote rename pb paul
git remote remove paul
```
* 标签的推送（共享标签）与删除（远端）
默认git push 不会传送标签到远端服务器仓库上。
```git
git push origin <tagname>
#  一次性提交多个标签
git push origin --tags

# 删除标签
git push <remote>：refs/tags/<tagname>
git push origin --delete <tagname>
```
## Git分支
一般来说执行commit命令后git 中存在三个对象，一个是文件快照对象，树对象（记录文件结构和快照对象的索引，以及提交对象（包含指向前树对象的指针和所有提交信息。
### 创建分支
```git
# 创建新的分支
git branch testing
```
创建分支后需要知道如何指向这个分支，git中存在HEAD的特殊指针，通过它进行分支的指定。
```git
# 指向新建的分支
git checkout testing

# 创建分支并立即切换
git checkout -b <newbranchname>
```
### 分支的合并
因为在新建的分支上进行修改后，快照是接续主分支的，合并分支就像将把分支的指针指向修改后的快照。在官方文档中把这一操作叫做快进。
```git
# 合并新的分支到当前分支
git merge <newbranch>
```
分支合并后如需要删除临时新建的分支可以使用：
```git
# 合并新的分支到当前分支
git branch -d <branch>
```
### 分支管理
```git
# 查看分支
git branch
git branch -v # 可以查看每个分支的最后一次提交
# --merged ：查看哪些分支已经与当前分支合并
# --no-merged 查看所有包含未合并工作的分支，分支中有未合并的工作，不能使用-d删除
# -D 强制删除
```
## .gitignore忽略文件
当一个git库中有不需要上传的文件时 进行配置：

    *.[oa]

    *~
    表示忽略以.a或.o结尾的文件，和忽略名字以~结尾的文件。
例子
```git
# 先忽略所有的.a文件
*.a
# 但跟踪所有的lib.a文件
！lib.a
# 忽略 当前目录下的TOD文件 而不忽略 xxx/TOD
/TOD
# 忽略任何目录下名为build的文件
build/
# 忽略当前目录下的文件，而不忽略doc/server/a.txt
doc/*.txt
# 忽略所有
doc/**/*.txt
```
