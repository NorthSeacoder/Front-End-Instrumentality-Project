## 代码存在的三个区域
1. Working Area
2. Staging Area
3. Repository
### Working Area
- 该区域中的文件是未被 git 处理的文件
- 也被叫做 untracked files

#### ls-files -s 命令
![store](./images/git-areas-and-stashing/ls-files.png)
- 展示暂存区中所有的文件
#### 暂存区中文件的添加与删除
```
//添加
git add <file>
//删除
git rm <file>
//重命名
git mv <file>
```
#### git add -p 命令
- 可交互的命令
- 一般用于做了大量修改时拆分提交操作

### Staging Area(暂存区)
- 即将成为下一次提交的文件
- 暂存区让 git 知道下一次提交与当前提交的变化
### Repository
- 保存所有提交的区域

### git中代码所在位置的切换
![move](./images/git-areas-and-stashing/move.png)

### git stash 
- 保存未提交的变动
#### 基础使用
```
//保存
git stash

//查看
git stash list

//查看内容
git stash show stash@{0}

//应用最新的 stash(保留)
git stash apply

//应用特定的 stash
git stash apply stash@{0}

//保存文件(默认不保存新增文件)
git stash --include-untracked

//保存所有文件(包括 gitignore)
git stash --all

//命名
git stash save "name"

//从 stash 建立分支
git stash branch <branchname> <stashid>

//应用并删除最新 stash,有冲突则不删除
git stash pop

//删除 stash
git stash drop <stashid(不填则删除最新 stash)>

//删除所有 stash
git stash clear
```