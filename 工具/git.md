# Git

## 修改提交信息
1. 已知 使用`git commit --amend --author 'qiudeng <2143975743@qq.com>'` 这个命令可以修改上一条commit的记录
    - 也可以用--amend修改要commit的文件、commit message等，后面再说
2. 使用`git rebase`把其他commit临时提升到commit的最顶层
    1. 比如你要修改的commit在最近的3个commit之内，执行 `git rebase -i HEAD~3`
    2. git会用默认编辑器打开一段格式为`pick <commit hash> <message>`的内容
    3. 把要修改的message前面的pick改成edit(我不确定改几个，只试过改一个) 然后保存，此时用git log查看就会发现这个commit被提到了最顶层，此时我们处于rebase的暂停状态
    4. 用上面的`git commit --amend`命令编辑commit信息
    5. 输入`git rebase --continue`结束暂停状态 commit信息就修改完毕了,rebase也结束了。