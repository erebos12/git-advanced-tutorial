# Advanced GIT

## Some facts

- Atomic working unit in GIT is a commit identified by SHA1 hash
- Branches are "pointers" to a certain commit which you can checkout, create, move, delete. You can have multiple branches pointing to same commit.
- Tags are fixed "pointers" to a commit.  
- HEAD is a reference to the last commit in the currently check-out branch. You can think of the HEAD as the "current branch" (`cat .git/HEAD`).

## The three stages

Files in a repository go through three stages before being under version control with git:

* Untracked: the file exists, but is not part of git's version control
* Staged: the file has been added to git's version control but changes have not been committed
* Committed: the change has been committed

<table><tr><td>
<img align="center" src="git-staging-diagram.png" title="GIT Stages" width="500">
</td></tr></table>

## Tips & Best-Practices

* Use a GIT Repository Browser of your choice (SourceTree, GitKraken, IDE-Plugins etc.) to get an overview over commit history and branches.

* Continuously update your local repo (`git remote update` / `git fetch`).

* Prefer `git fetch` or `git remote update` over `git pull`

* Best tutorials:
  - `man git-<command>`
  - https://git-scm.com/
  - https://www.atlassian.com/git/tutorials
  - http://gitready.com/


## Committing

#### git commit --amend
Replace the tip of the current branch by creating a new commit.
By that you can add changes to the previous commit but a new commit (Hash) will be created.

#### Commit messages
Use editor for committing to write commit message instead of `git commit -m "your message"`.
By that you can add more comprehensive documentation to your commits.
Configure your editor of your choice like that:
`
git config --global core.editor vim
`
Advanced committing options with interactive rebase (See "Interactive Rebase").

## git reflog

Reflog is a mechanism to record when the tip of branches are updated.
This command is to manage the information recorded in it. This command shows is a list of commits that Git still has in its storage.  
See `man git-reflog` for more details.


## Cherry-pick / Merging / Rebasing

There are 3 ways to bring commits from one branch to another:
* man git-**cherry-pick**
* man git-**merge**
* man git-**rebase**

### Cherry picking
Pick any commit(s) from another branches and set it on the tip of the HEAD.
`git cherry-pick <commit>`

### Merging

* git merge combines two branches by one merge commit
* receiving branch = branch that will be merged into

`
git fetch
git checkout receiving-branch   # often master
git merge branch-to-merge   # often feature branches
`

* **ATTENTION**: Execute `git fetch` to get the latest remote commits before merging.


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

### Interactive Rebase

Interactive rebasing gives you the opportunity to alter commits as they are moved to the new branch. By that you can change the branch commit history.

```
git checkout feature
git rebase -i master
```

```
git rebase --continue
git rebase --abort
```

But when changing your own feature branch locally , i.e. by interactive rebase, which you have pushed before, a `git push -f origin my-private-feature-branch` can be ok. _But be sure what you're doing! You overwrite the feature branch!!!_

## Updating locally - fetch vs. pull

### git fetch

`git fetch` really only downloads new data from a remote repository - but it doesn't integrate any of this new data into your working files. Fetch will never manipulate, destroy, or screw up anything. This means you can never fetch often enough.

### git pull

`git pull`, in contrast, is used with a different goal in mind: to update your current HEAD branch with the latest changes from the remote server. This means that pull not only downloads new data; it also directly integrates it into your current working copy files.

Since "git pull" tries to _**merge**_ remote changes with your local ones, a so-called "merge conflict" can occur.

Like for many other actions, it's highly recommended to start a "git pull" only with a clean working copy. This means that you should not have any uncommitted local changes before you pull.


## Reseting current branch - git reset

Resets current HEAD to the specified state (Moving branch).

`git reset [mode] where mode = hard|soft|mixed|merge`


## Getting remote

### Force Pushing
```
# YOU SHOULD NEVER DO THIS! GIT HOOKS AVOID THIS!
git push -f origin  master
```

TBC
