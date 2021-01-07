---
title: gitCommand
date: 2021-01-07 20:07:44
tags: 
- git
categories:
- git
comments: false
---

# git command

[可以观看廖雪峰大神写的git教程](https://www.liaoxuefeng.com/wiki/896043488029600)

*   `git init`

    > 初始化一个git仓库

*   `git remote add origin <git@server-name:path/repo-name.git>`

    > 关联远程库

*   `git clone`

    > 克隆

*   `git checkout -b <name>` or `git switch -c <name>`

    > 检出分支pr-branch并切换到新分支上

*   `git checkout -b <new-branch> origin/<remote-branch>`

    > 检出远程分支remote-branch到本地，命名为new-branch

*  ` git commit`

    > 本地提交

*   `git pull`

    > 远程更新拉到本地

*   `git push`

    > 本地推送到远程

*   `git merge c2`

    > 把c2 branch 内容合并到当前分支上

*   `git rebase c2`

    > 将c2分支最新的提交同步到当前分支，并变成线性提交

*   `git checkout c2^`

    > 分离HEAD，让其指向c2的父级

*   `git checkout c2~2`

    > 让其指向c2的父级的父级

*   `git branch -f c2 HEAD~2`

    > 将分支c2重新分配给当前分支的父级的父级

*   `git reset HEAD~1`

    > 将当前分支向后移到较早的提交上来恢复更改

*   `git revert HEAD`

    > 重写当前分支，把上次的提交，反转过来再次提交， 来恢复之前的更改

*   `git push -u origin master`

    > 将本地的master分支推送到origin主机，同时指定origin为默认主机，后面就可以不加任何参数使用git push了

*   `git pull`

    > 相当于git fetch, git merge

*   `git pull --rebase`

    > 相当于git fetch , git rebase ，本地就不会出现提交分叉以及万恶的merge commit信息

*   `rebase` 好处

    <pre spellcheck="false" class="md-fences md-end-block ty-contain-cm modeLoaded" lang="reStructuredText" cid="n214" mdtype="fences" style="box-sizing: border-box; overflow: visible; font-family: var(--monospace); font-size: 0.9em; display: block; break-inside: avoid; text-align: left; white-space: normal; background-image: inherit; background-position: inherit; background-size: inherit; background-repeat: inherit; background-attachment: inherit; background-origin: inherit; background-clip: inherit; background-color: rgb(248, 248, 248); position: relative !important; border: 1px solid rgb(231, 234, 237); border-radius: 3px; padding: 8px 4px 6px; margin-bottom: 15px; margin-top: 15px; width: inherit;">想要更好的提交树，使用rebase操作会更好一点。
    这样可以线性的看到每一次提交，并且没有增加提交节点。
    merge 操作遇到冲突的时候，当前merge不能继续进行下去。手动修改冲突内容后，add 修改，commit 就可以了。
    而rebase 操作的话，会中断rebase,同时会提示去解决冲突。
    解决冲突后,将修改add后执行git rebase –continue继续操作，或者git rebase –skip忽略冲突。</pre>

*   `git stash`

    > 储藏现场工作（将git仓库已经追踪记录的，但是没有commit的东西先存起来）

*   `git stash list`

    > 查看储藏区

*   `git stash apply stash@{0}`

    > 恢复储藏区， 不会删除储藏区的内容，等待commit

*   `git stash pop`

    > 恢复储藏区， 同时会删除储藏区的内容，等待commit

*   `git stash drop stash@{0}`

    > 删除储藏区的内容

*   `git cherry-pick 4c805e2`

    > 复制一个特定的提交到当前分支

*   `git tag <tagname> ? <commit ID>`

    > 用于新建一个标签，默认为`HEAD`，也可以指定一个commit id

*   `git tag -a <tagname> -m "This is a tag"`

    > 可以指定标签信息

*   `git tag`

    > 查看所有标签

*   `git show <tagname>`

    > 查看一个标签

*   `git tag -d <tagname>`

    > 删除一个标签

*   `git push origin <tagname>`

    > 推送一个本地标签 到 远程

*   `git push origin --tags`

    > 推送全部未推送过的本地标签

*   `git push origin :refs/tags/<tagname>`

    > 删除一个远程标签

