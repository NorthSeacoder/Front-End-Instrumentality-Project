### 分布式管理系统

#### Git存储数据的方式
- 核心是键值对
- 值是数据,键是数据的哈希值

##### 键-SHA1
- 是一种加密哈希函数
- 通过提交的部分数据,它会产生一组40位的16进制数组
- 当数据相同时,返回的值一定相同

##### 值-blob
- git 会将压缩数据连同一些元数据一同存放在blob对象中
    - 头部是blob标识符和正文的大小
    - content和header以\0分隔(C语音中的空字符串结束符)
    - 正文  
![blob](./images/What-is-Git/blob-1.png)  

#### git内部的操作
##### hash-object命令(获取content的SHA1字符串)
![git-bash-hash-object](./images/What-is-Git/git-bash-hash-object.png) 
- --stdin => 从命令行中获取标准输入,否则该命令一般用于转换文件

##### openssl命令(直接从内容获取sha1值)
![git-openssl](./images/What-is-Git/git-openssl.png)
- -n=>去掉换行符

#### git存储数据的方式(.git文件夹)

![store](./images/What-is-Git/store.png)
- -w=>write 写下入

##### .git文件夹的结构
![git-construction](./images/What-is-Git/git-construction.png)  
- 刚才的blob文件就保存在objects/8a文件夹下
- 该文件夹以blob sha1值前两个字符命名
- blob依然缺少了一些信息
    - 文件名
    - 文件夹结构

#### tree对象
- 包含指针(SHA1)
    - 指向blob
    - 指向其他tree
- 指针类型(blob/tree)
- 文件名
- 模式  
![tree](./images/What-is-Git/tree.png)  

##### tree的一些优化
- 相同的内容只存储一次
- 所有的存储对象都是压缩文件
- As files change, their contents remain mostly similar.
- git会把问价压缩为一个包文件
- 包文件会存储对象以及增量-文件不同版本的差异
- 包文件会在以下条件下生成:
    - 内容较多
    - 垃圾回收期间
    - 推向远程仓库

#### commit对象(代码快照)
构成:
- 指针:指向tree
- 作者和committer
- 日期
- message
- 父级commits(可能没有,也可能多个)
- SHA1值是以上数据的hash值  
![commit](./images/What-is-Git/commit.png) 

##### cat 命令(linux)
- 显示整个文件
```
cat filename
```
- 创建一个新文件
```
cat > filename
```
- 合并文件
```
cat file1 file2 > file
```
##### 显示 blob 文件

![cat](./images/What-is-Git/cat.png) 
- 此处是压缩对象

##### cat-file命令
![cat-file](./images/What-is-Git/cat-file-t.png) 
- -t =>显示类型  

![cat-file](./images/What-is-Git/cat-file-p.png) 
- -p 显示内容  

- tree
![cat-file](./images/What-is-Git/cat-file-tree.png) 
![cat-file](./images/What-is-Git/tree2.png) 
- commit
![cat-file](./images/What-is-Git/commit1.png) 
##### 为什么 commit 无法被修改
- 修改任何数据,commit都会生成新的SHA1
- 即使数据没有变动,时间也不会相同