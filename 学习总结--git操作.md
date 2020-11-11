## git 命令列表

- 查找2个分支的共同父节点：git merge-base chucklu_zhCN(当前分支) master(比较分支)
- 查看分支历史记录:git reflog --date=local
- git config --list
- git config --global core.autocrlf true
- git log --graph --pretty=oneline --abbrev-commit

## git分支合并

### git merge

> git-merge 命令是用于从指定的 commit(s) 合并到当前分支的操作。合并策略是git自动选择的，可以通过 -s参数指定策略。
>
> `git merge topic`命令将会把在master分支上二者共同的节点（E节点）之后分离的节点（即topic分支的A B C节点）重现在master分支上，直到topic分支当前的commit节点（C节点），并位于master分支的顶部。**并且沿着master分支和topic分支创建一个记录合并结果的新节点，该节点带有用户描述合并变化的信息**。

#### merge策略

- resovle(解决)：**这种策略只能合并两个分支，首先定义某个次commit为祖先为合并基础，然后执行一个直接的三方合并** 
- recursive(递归)
- octopus(章鱼)：**当需要多个分支的时候，就可以用octopus来解决，这就是来同时合并多个分支的策略。** 

#### merge 三种情况

- 无冲突，合并成功，**可以在当前分支看到合并分支的提交路径，生成新节点带有用户描述合并变化的信息**
- 有冲突，解决冲突，提交信息，**可以在当前分支看到合并分支的提交路径，生成新节点带有用户描述合并变化的信息**
- 回退合并，**git relog 查看merge之前的记录，git  reset --hard 回退到merge之前的版本**

#### merge参数

- --ff :快速合并    
-  --ff--only:只快速合并（如果有冲突就失败）
- --no-ff: 非快速合并
- --squash:将合并过来的分支的所有不同的提交，当做一次提交，提交过来 
- --abort:抛弃当前合并冲突的处理过程并尝试重建合并前的状态。

### git rebase

```nginx
git checkout master
git pull
git checkout local
git rebase -i HEAD~2  //合并提交 --- 2表示合并两个 
git rebase master---->解决冲突--->git rebase --continue   //变基操作
git checkout master
git merge local      //快速合并(直接把master分支移到local分支，并移动head指针，此时master分支和local分支都指向最新提交，此节点为两个分支的公共节点)
git push
```

### git cherry-pick

```nginx
#将指定的提交（commit）应用于其他分支
git cherry-pick <commitHash>
#Cherry pick 支持一次转移多个提交。
git cherry-pick <HashA> <HashB>
#想要转移一系列的连续提交(不包含A提交)
git cherry-pick A..B 
#想要转移一系列的连续提交(包含A提交)
git cherry-pick A^..B 
```

### 代码冲突

- **`--continue`** ：用户解决代码冲突,使用此参数使过程继续执行
- **`--abort`**：发生代码冲突后，放弃合并，回到操作前的样子
- **`--quit`**：发生代码冲突后，退出当前操作，但不回到操作前的样子

## 创建远程分支

```nginx
git checkout -b newdev
git push  --set-upstream origin newdev
```

## 删除远程分支

```nginx
git checkout master
git push origin --delete [branch_name]
```

## 追加提交

```nginx
git commit --amend
```

