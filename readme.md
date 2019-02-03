# Advanced GIT


## Table of Content
<!-- TOC depthFrom:3 -->
- [Some facts](#some_facts)
- [The three stages](#three_stages)
- [Committing](#committing)
  - [git commit --amend](#amend)
  - [Commit messages](#commit_messages)
- [Moving branches](#git_reset)
- [Forensics with git reflog](#reflog)
- [Bringing branches together](#cp_m_r)
  - [Cherry Pick](#cherry_pick)
  - [Merging](#merging)
  - [Rebasing](#rebasing)
    - [Interactive Rebase](#interactive_rebase)
- [Getting Remote](#getting_remote)
  - [Push to remote branch](#push_remote)
  - [Force Pushing](#force_push)
  - [Sync local with remote](#updating)
- [Tips & Best-Practices](#tips)
<!-- /TOC -->

<a name="some_facts"></a>
## Some facts

- Atomic working unit in GIT is a commit identified by SHA1 hash
- Branches are "pointers" to a certain commit which you can checkout, create, move, delete. You can have multiple branches pointing to same commit.
- Tags are fixed "pointers" to a commit.  
- HEAD is a reference to the last commit in the currently check-out branch. You can think of the HEAD as the "current branch" (`cat .git/HEAD`).

<a name="three_stages"></a>
## The three stages

Files in a repository go through three stages before being under version control with git:

* Untracked: the file exists, but is not part of git's version control
* Staged: the file has been added to git's version control but changes have not been committed
* Committed: the change has been committed

<table><tr><td>
<img align="center" src="git-staging-diagram.png" title="GIT Stages" width="500">
</td></tr></table>

<a name="committing"></a>
## Committing

<a name="commit_messages"></a>
#### Commit messages
Use editor for committing to write commit message instead of `git commit -m "your message"`.
By that you can add more comprehensive documentation to your commits.
Configure your editor of your choice like that:
```
git config --global core.editor vim
```
Check with `git config -l` your config (your config file you can find under `~/.gitconfig`).

<a name="amend"></a>
#### git commit --amend
Replace the tip of the current branch by creating a new commit.
By that you can add changes to the previous commit but a new commit (Hash) will be created.


Advanced committing options with interactive rebase (See "Interactive Rebase").

<a name="git_reset"></a>
## Moving branches

`git reset` moves current HEAD to the specified state (Moving branch to a specific commit). With options you can specify what happens to the changes/commits during reset.

`git reset [mode] where mode = hard|soft|mixed`

* hard = changes will be removed
* soft = changes will kept staged
* mixed = changes will be kept unstaged


<a name="reflog"></a>
## Forensics with git reflog

Reflog is a mechanism to record when the tip of branches are updated.
This command is to manage the information recorded in it. This command shows is a list of commits that Git still has in its storage.  
See `man git-reflog` for more details.

With `git reflog --all` you can see all reference modifications from you local repository. By that you can rescue "lost" commits which might be "invisible" after resetting branches accidentally.
```
$ git reflog --all  # find commit ID of your lost commit -> here ba7abb5
...
$ git checkout -b my-branch ba7abb5
Switched to a new branch 'my-branch'  # YEAH, rescued
```


<a name="cp_m_r"></a>
## Bringing branches together

There are 3 ways to bring commits from one branch to another:
* man git-**cherry-pick**
* man git-**merge**
* man git-**rebase**

<a name="cherry_pick"></a>
### Cherry Pick
Pick any commit(s) from another branches and set it on the tip of the HEAD.
`git cherry-pick <commit>`

<a name="merging"></a>
### Merging

* git merge combines two branches by one merge commit
* receiving branch = branch that will be merged into

```
git fetch
git checkout receiving-branch   # often master
git merge branch-to-merge   # often feature branches
```

* **ATTENTION**: Execute `git fetch` to get the latest remote commits before merging.

<a name="rebasing"></a>
### Rebasing

git-rebase sets HEAD with all commits from common base to the tip of another branch.

```
# This moves the entire 'feature' branch to begin on the tip of the 'master' branch
git checkout feature
git rebase master
```

* The major benefit of rebasing is that you get a much cleaner project history. You get a linear project history.

* **ATTENTION**: Rebasing re-writes the project history by creating brand new commits for each commit in the original branch.

* **Golden rule of git rebase**:  Never use it on public branches. Meaning never rebase master (public branch) onto feature branch. Rebase only local feature branches on master!

In case of merge conflict during rebase, resolve conflict, add file and then continue with `git rebase --continue`.
If you want to abort the interactive rebase then use `git rebase --abort`.

<a name="interactive_rebase"></a>
#### Interactive Rebase

Interactive rebasing gives you the opportunity to alter commits as they are moved to the new branch. By that you can change the branch commit history.

```
git checkout feature
git rebase -i master
```

<a name="getting_remote"></a>
## Getting remote

<a name="push_remote"></a>
### Push to remote branch
```
# This will push local 'master' to remote branch 'new'
# Branch will be created remotely in case it doesn't exist
git push origin master:new
```

<a name="force_push"></a>
### Force Pushing
```
# NEVER DO THIS! GIT HOOKS CAN AVOID THIS!
git push -f origin  master
```
But when changing your own feature branch locally , i.e. by interactive rebase, which you have pushed before, a `git push -f origin my-private-feature-branch` can be ok. _But be sure what you're doing! You overwrite the remote feature branch!!!_

<a name="updating"></a>
### Sync local with remote

* **git remote update** - will update all of your branches set to track remote ones, but not merge any changes in.

* **git fetch** - will update only the branch you're on, but not merge any changes in (`git remote update = git fetch --all`).

* **git pull** - will update and merge any remote changes of the current branch you're on (`git pull = git fetch + git merge.`).

<a name="tips"></a>
## Tips & Best-Practices

* Use a GIT Repository Browser of your choice (SourceTree, GitKraken, IDE-Plugins etc.) to get an overview over commit history and branches.

* Continuously update your local repo (`git remote update` / `git fetch`).

* Commit often, even when changes are not final! You can use `git commit --amend` for that.

* Best tutorials:
  - `man git-<command>`
  - https://git-scm.com/
  - https://www.atlassian.com/git/tutorials
  - http://gitready.com/
