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

## 换行符问题

参考[这篇文章](https://cloud.tencent.com/developer/article/2349002)

MAC: `\r` = CR = Carriage-Return = 回车

LINUX: `\n` = LF = Line-Feed = 换行

WINDOWS: `\r\n` = CRLF = Carriage-Return Line-Feed = 回车换行

对于Windows环境：
打开`%USERPROFILE%\.gitconfig`，有一项`autocrlf`，当该值等于：
- true 表示在推送时转成 `\n`，在拉取时转成 `\r\n`
- false 表示在推送时和拉取时都原样保留换行符。这样的设置在所有程序员都在同一个平台开发时很有用，git 完全不处理换行符，全部改由开发者自行解决。
- input 表示在推送时转成 `\n`，在拉取时原样保留换行符。注意到，这样的设置会让仓库里所有的换行符都变成 `\n` 不再有什么时候有 `\r\n` 。
- 