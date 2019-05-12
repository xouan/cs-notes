
#### 本节讲解了几个基本命令：
1. git config：配置相关信息
2. git clone：复制仓库
3. git init：初始化仓库
4. git add：添加文件到暂存中
5. git diff：比较内容
6. git status：获取当前项目状况
7. git commit：提交
8. git branch：分支相关
9. git checkout：切换分支
10. git merge：合并分支
11. git reset：恢复版本
12. git log：查看日志
# 起步

# git基础
## 获取仓库
2. git clone <仓库地址>：复制远程仓库
```
git clone https://github.com/swiftma/program-logic.git   # 克隆远程仓库
git init
```
3. git init：初始化仓库
```
git init # 初始化当前目录为git仓库
``` 
## 改变文件状态
4. git add：添加文件到暂存中
```
git add readme.md  # 添加指定文件到暂存，可选择多个文件
git add -A   # 添加所有未暂存文件到暂存
git add .    # 添加所有未暂存文件到暂存
git add -u   # 添加所有未暂存文件到暂存
```
5. git commit：提交
```
git commit -m "worte a readme file"  # 提交暂存区文件到仓库，一次提交多个
git commit -am "wrote a readme file"  # 提交暂存区和已修改的文件
```

## 检查历史信息和状态
6. git status：获取当前项目状况
```
git status
```
7. git log：查看日志
```
$ git log
commit df4edeb1a50c93abb96c6da6d3f077159404c7b9 (HEAD -> master)
Author: youquanxu <2515922813@qq.com>
Date:   Sat Nov 17 20:55:42 2018 +0800

    增加readme文件

commit 7e908c66c02170c2039942883fb0dc08d1b73607
Author: youquanxu <2515922813@qq.com>
Date:   Sat Nov 17 14:01:05 2018 +0800

    提交改变

commit b77da8e53723705492dcd6eedc899003ccde3f6b
Author: youquanxu <2515922813@qq.com>
Date:   Sat Nov 17 13:25:32 2018 +0800
```
## 比较提交
```
$ git diff    // 查找当前工作目录未暂存内容和本地暂存之间的差异
$ git diff --cached   // 显示当前暂存和上次提交之间的差异
$ git diff HEAD   // 显示未暂存文件与上次提交之间的差异
$ git diff master..experimental  // 比较两个分支之间的差异
$ git diff master ...experimental  // 比较master与experimental的共有父分支与experimental之间的差异
```
## 分布式
```
$ git pull /d/myrepo master  // 从远程分支拉取修改合并到主分支
```
## 撤销和版本回退
## 分支
## 远程
```
$ git remote  // 显示自己的远程仓库简写
$ git remote -v  // 显示所有的远程仓库简写及其地址
$ git remote add origin https://github.com/youquanxu/CS-notes.git  // 添加一个新的远程仓库，远程仓库存在
```
```
$ git push origin master  // 推送分支master到远程origin分支,需输入用户名和密码
```
```
$ git fetch origin  //从远程拉取数据，不会自动合并到分支，需要手动合并，执行git merge <分支名> <远程分支名>
```
# git中级
# git高级

