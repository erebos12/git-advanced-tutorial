# Advanced GIT

## Some facts

- Atomic working unit is a commit identified by SHA1 hash
- Branches are "pointers" to a certain commit which you can checkout, create, move, delete. You can have multiple branches pointing to same commit.
- Tags are fixed "pointers" to a commit.  

## Tips & Best-Practices

* Use a GIT GUI of your choice (SourceTree, GitKraken) to get an overview over commit history and branches.

* Continuously update your local repo (```git remote update``` / ```git fetch```).

* Prefer ```git fetch``` or ```git remote update``` over ```git pull```

## Committing

Amending:
```
git commit --amend
```

## Merging vs. Rebasing

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
```
git checkout feature
git rebase -i master
```

* Interactive rebasing gives you the opportunity to alter commits as they are moved to the new branch. By that you can change the branch commit history.

### Force Pushing
```
# YOU SHOULD NEVER DO THIS! GIT HOOKS AVOID THIS!
git push -f origin  master
```

But when changing your own feature branch locally , i.e. by interactive rebase, which you have pushed before, a ```git push -f origin my-private-feature-branch``` can be ok. _But be sure what you're doing! You overwrite the feature branch!!!_

## Updating locally - fetch vs. pull

### git fetch

```git fetch``` really only downloads new data from a remote repository - but it doesn't integrate any of this new data into your working files.
fetch will never manipulate, destroy, or screw up anything. This means you can never fetch often enough.

## git pull
```git pull```, in contrast, is used with a different goal in mind: to update your current HEAD branch with the latest changes from the remote server. This means that pull not only downloads new data; it also directly integrates it into your current working copy files.

Since "git pull" tries to _**merge**_ remote changes with your local ones, a so-called "merge conflict" can occur.

Like for many other actions, it's highly recommended to start a "git pull" only with a clean working copy. This means that you should not have any uncommitted local changes before you pull.
