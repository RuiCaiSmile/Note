# git及github使用
## windows系统
### 初始操作
- `git config --global user.name "your name"`设置用户姓名
- `git config --global user.email "your_email@youremail.com"`设置用户邮箱
- `ssh-keygen -t rsa -C "your_email@youremail.com"`使用SSH协议链接github
- `git init`创建
- `git add`跟踪文件，将文件传入暂存区
- `git commit`提交文件，配合使用 `-m`提供提交信息，`-a`跳过`git add`步骤
- `git clone`克隆项目
- `git status`查看文件状态
- `git diff`显示还没有暂存起来的改动,`git diff --cached` 查看已经暂存起来的变化
- `git rm`删除文件
- `git add -A` 会把未通过 git rm 删除的文件全部stage
- `git push origin master`推送HEAD至远端仓库


### 文件回退恢复
- 同版本，尚未`add`
    使用`git checkout`查看文件列表， `git checkout 文件名称`即可恢复对应的文件到版本库的状态
- 同版本，已经`add`
    使用`git reset 文件名称`将文件从暂存区恢复到工作区，再执行`git checkout`
- 不同版本
