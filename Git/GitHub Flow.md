# 理解GitHub Flow
GitHub flow是Git Flow的简化版，是一个轻量级的基于分支的工作流，通过创建分支，commit提交，pull request，合并分支等等来进行协同开发。其最大优点就是简单，对于需要持续发布的产品，可以说是最合适的流程。

## 比较
* Git Flow      ⇨ 版本发布
* GitHub Flow   ⇨ 持续发布
* Gitlab Flow   ⇨ 持续发布和版本发布（可根据实际需要进行选择）
* 自定义


# 协同开发流程
1. 创建组织
2. 创建项目主仓库
3. Clone主仓库（内部项目）或Fork主仓库后Clone fork仓库（开源项目）
4. 添加上游地址
---
5. 同步最新代码
6. 创建功能分支feat/2
7. 提交代码
8. 合并最新代码（解决合并冲突）
9. 推送代码
10. 提交Pull request
11. 讨论审核代码
12. 测试环境测试（可选步骤）
13. 合并和部署
14. 删除功能分支feat/2

## 相关Git指令
### 新建代码库
```bash
# 初始化Git代码库
git init

# 新建一个代码库，初始化为Git代码库
git init <project-name>

# 克隆一个仓库项目
git clone <url>
```

### 增加/删除文件
```bash
# 添加指定文件到暂存区
git add <file1> <file2> ...

# 添加指定目录到暂存区
git add <dir>

# 添加全部文件到暂存区
git add .

# 删除暂存区文件
git rm --cached <file>

# 强制删除暂存区和工作区文件
git rm -f <file>
```

### 代码提交
```bash
# git commit -m [message]

# 提交暂缓区的指定文件到仓库区
git commit [file1] [file2] ... -m [message]

# 改写上一次提交
git commit -amend -m [message]
```
### 分支
```bash
# 查看分支
## 查看所有本地分支
git branch

## 查看所有远程分支
git branch -r

## 查看所有本地和远程分支
git branch -a

# 创建分支
## 基于当前分支，创建但不切换
git branch [new branch]

## 基于当前分支，创建并切换
git checkout -b [new branch]

## 基于远程分支，创建追踪分支
git branch --track [branch] [remote-branch]

# 切换分支
## 切换到指定分支
git checkout [branch]
git switch [branch]

## 切换到上一个分支
git checkout -

# 合并分支
## 合并指定分支到当前分支（快速）
git merge [branch]

## 合并指定分支到当前分支（非快速）
git merge --no-ff [branch]

## 合并压缩
git merge --squash [branch]

```


http://scottchacon.com/2011/08/31/github-flow.html