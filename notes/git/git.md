## 命令常规

```bash
# 初始化
git init 
# 历史纪录
git reflog
# 查看当前状态
git status
# 提交历史
git log
```

## 工作区&暂存

```bash
# 把文件修改添加到暂存区
git add [filename]
# 把暂存区的所有内容提交到当前分支
git commit -m 'some description'
# 差异对比
git diff HEAD -- [filename]
```

## 版本回退

```bash
# 查看提交日志
git log --pretty=oneline
# 回退到上一个版本
git reset --hard HEAD^
# 回退到指定版本
git reset --hard [版本id]
```

## 撤销文件修改

```bash
# 让文件回到最近一次git commit或git add时的状态
git checkout -- [filename]
# 文件删除
git rm [filename]
```

ps: 删除以后可以通过 "撤销文件修改" 恢复

## 远程仓库

```bash
# 生成ssh key
ssh-keygen -t rsa -C "your_email"
# 一路回车，接下来在用户主目录里找到.ssh目录，里面找到id_rsa.pub并复制里面的内容
# 登陆GitHub，打开“Account settings”，“SSH Keys”里面添加刚才复制的key
# 创建一个远程仓库 xxx
# 本地添加账户和邮箱验证
git config --global user.name "your_github_name"
git config --global user.email "your_email"
# 指定远程仓库地址
git remote add origin git@github.com:用户名/仓库名.git
# 推送
git push origin master
```

## 分支

### 常规操作

```bash
# 查看分支
git branch
# 创建分支
git branch <name>
# 切换分支
git checkout <name>
# 或者
git switch <name>
# 创建+切换分支：
git checkout -b <name>
# 或者
git switch -c <name>
# 合并某分支到当前分支
git merge <name>
# 强制禁用Fast forward模式，生成一个新的commit，以便从分支历史上可以看出分支信息
git merge --no-ff -m "merge with no-ff" dev
# 删除分支
git branch -d <name>
```

### bug分支

```bash
# 创建stash，储存当前工作
git stash
# 切换回master
git checkout master
# 在master创建一个issue分支
git checkout -b issue-101
# 修复bug
git add xxx.file 
# 提交
git commit -m "fix bug"
# 得到 [issue-101 4c805e2] fix bug 101
# 切换回master
git switch master
# 合并、删除issue-101分支
git merge --no-ff -m "merged bug fix 101" issue-101
# 切换回dev分支
git switch dev
# 恢复工作区
git stash pop
# 同步bug修复代码到dev分支
git cherry-pick 4c805e2
```

### Feature分支

```bash
# 创建并切换feature分支
git switch -c feature-vulcan
git add vulcan.file
git commit -m "add feature vulcan"
git switch dev
# 情况1：合并vulcan分支到dev
git merge --no-ff -m "merged feature vulcan" feature-vulcan
# 情况2：丢弃并删除vulcan的情况
git branch -D feature-vulcan
```
## 其他

### 更新fork仓库
```bash
# 将其他人的仓库fork到自己的repository下
# 将fork后的仓库clone到本地
# 添加源仓库地址
git remote add upstream xxxx
# 切换分支
git checkout master
# 将原仓库的更新拉到本地
git fetch upstream
# 合并
git merge upstream/master
# 推送
git push origin master
```
