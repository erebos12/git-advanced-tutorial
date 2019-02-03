# Advanced GIT

## Some facts

- Atomic working unit in GIT is a commit identified by SHA1 hash
- Branches are "pointers" to a certain commit which you can checkout, create, move, delete. You can have multiple branches pointing to same commit.
- Tags are fixed "pointers" to a commit.  
- HEAD is a reference to the last commit in the currently check-out branch. You can think of the HEAD as the "current branch" (`cat .git/HEAD`).


## Tips & Best-Practices

* Use a GIT Repository Browser of your choice (SourceTree, GitKraken, IDE-Plugins etc.) to get an overview over commit history and branches.

* Continuously update your local repo (```git remote update``` / ```git fetch```).

* Prefer ```git fetch``` or ```git remote update``` over ```git pull```

* Best tutorials https://git-scm.com/ or https://www.atlassian.com/git/tutorials or ```man git-<command>```


## Committing

#### git commit --amend
Replace the tip of the current branch by creating a new commit.
By that you can add changes to the previous commit but a new commit (Hash) will be created.

#### Commit messages
Use editor for committing to write commit message instead of ```git commit -m "your message"```.
By that you can add more comprehensive documentation to your commits.
Configure your editor of your choice like that:
```
git config --global core.editor vim
```
Advanced committing options with interactive rebase (See "Interactive Rebase").

## Merging vs. Rebasing

### Merging

* git merge combines two branches
* receiving branch = branch that will be merged into

```
git fetch
git checkout receiving-branch   # often master
git merge branch-to-merge   # often feature branches
```

* **ATTENTION**: Execute ```git fetch``` to get the latest remote commits before merging.


### Rebasing

```
git checkout feature
git rebase master
```

* This moves the entire _feature_ branch to begin on the tip of the _master_ branch

* The major benefit of rebasing is that you get a much cleaner project history. You get a linear project history.

* **ATTENTION**: Rebasing re-writes the project history by creating brand new commits for each commit in the original branch.

* **Golden rule of git rebase**:  Never use it on public branches. Meaning never rebase master (public branch) onto feature branch.

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

### Force Pushing
```
# YOU SHOULD NEVER DO THIS! GIT HOOKS AVOID THIS!
git push -f origin  master
```

But when changing your own feature branch locally , i.e. by interactive rebase, which you have pushed before, a ```git push -f origin my-private-feature-branch``` can be ok. _But be sure what you're doing! You overwrite the feature branch!!!_

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


## Special moves

### Undo n last commits and redo

`$ git commit ...`

`$ git reset --soft HEAD^n`

### Undo n last commits permanently

`$ git reset --hard HEAD~n`


###  Undo a merge inside a dirty working tree

`$ git pull `

`something goes wrong ...`

`$ git reset --merge`
