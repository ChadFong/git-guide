[When in doubt](http://lmgtfy.com/?q=stack+overflow+git)

The goal of this guide is to make you git literate.  [Be the friend in git.txt](https://xkcd.com/1597/)

# Git Commands:

## `git status`: list files changed since last commit
You will see staged and unstaged changes.  Staged changes are what will be commited when running `git commit`  
* You can stage changes using `git add <file/directory>`  
* You can unstage changes using `git reset HEAD <file/directory>`
* You can **permanantely undo _unstaged_ changes** using `git checkout -- <file/directory>`
  * **Uncommited changes cannot be retrieved.**


## `git diff`: compare file contents since last commit
`git diff`  
Shows the lines added/removed locally since your last commit. Visual Studio's diff tool is also good.  
You can also use git diff to [compare between branches](http://stackoverflow.com/questions/9834689/comparing-two-branches-in-git)


## `git add`: add selected file(s) to the staging area  
`git add <dir/file/wildcard>`  
e.g., `git add ./assets/` or `git add readme.md` or `git add *.txt`


## `git commit`: take a snapshot of the current state of the repository and adds it to the current branch
`git commit`  
e.g., `git commit`  
Opens your [chosen editor](http://stackoverflow.com/questions/2596805/how-do-i-make-git-use-the-editor-of-my-choice-for-commits) so you can create your commit message.  Save and exit when finished.  
You should commit early and often, you can not have too many commits.  Ideally you should commit once you've made **one logical change**. This keeps your commit history easy to read and understand at a glance (and when you look back trying to figure out what you were doing).  
[Commit messages should be detailed](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html).  Use this opportunity to make your changesets clear for review. Reviewing by commit is simpler than reviewing by the entire changeset _if the commit messages are detailed and clear_.  
[Don't do this](https://xkcd.com/1296/)

## A quick tangent to define a phrase or two
#### `ref` is reference.  This can be a commit, a branch name, a tag, etc...
#### `HEAD` is a ref that points to wherever it is you're looking. Typically this is the tip of the current branch, but that is not always the case. `git reset` and `git checkout` move HEAD.


## `git tag`: mark a commit to be accessed by a particular name, e.g., '1.0'
A tag is like a branch that cannot be moved.  It is a ref to a commit hash.

* list tags with `git tag`, search with `git tag -l "<search-params>"` e.g., `git tag -l "1.*"`  
* create tags with `git tag <name>`. Tags current commit
* create annotated tag with `git tag -a <name> -m <message>` e.g., `git tag -a v1.0 -m "Launch of Product"`
  * it is advised to tag launches with annotated tags because the timestamp of the commit AND the timestamp of the tag are both saved. In case you want to know when a version was given to a commit, and not just when the commit itself was made.

[more on tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging)


## `git branch`: create a branch to append commits to
`git branch <branch name>`  
e.g., `git branch css-tree-fix`  
Branch names should be readable, and descriptive of the branch's goal.  "css-tree-fix" not "my-branch-3"  
This creates a ref which points to a particular commit hash. Namely whichever commit you were on when you created the branch.  
**Important:** This command does not change you to your newly created branch.  See `git checkout` below  
**Important:** We do not do work directly on master, use this command or `git checkout -b <branch name>` explained below


## `git checkout`: change working directly to model previously made commit
`git checkout <commit hash/ref(branch name/tag/etc...)>`  
e.g., `git checkout ef32eo` or `git checkout feat-branch`

* If you want to make a new branch **and** move to it in one step use `git checkout -b <branch name>`

This command cannot be made with changes in your working directory.
* If you do not want them anymore, run `git checkout -- .` to **permanently** discard these changes.
  * _You can also do this with the reset command, see below_
* If you do want them, commit them.
* If you do want them, but not _that_ bad, run [`git stash`](https://git-scm.com/docs/git-stash)

git checkout can be used to pull a branch that is not available locally if it exists on a remote  

## `git reset`: move HEAD to the designated location, optionally modify working directory
`git reset <commit hash/ref(branch name/tag/etc...)> <option>`  
This is used primarily with 3 settings
* `--soft`: Any differences between your current state and the ref you're moving to are retained.  _Staged changes remain staged_.
* `--mixed` (default): Any differences between your current state and the ref you're moving to are retained. _Staged changes are unstaged_.
* `--hard`: The working directory is set to exactly model the ref in question, any uncommited/unstashed changes are lost

Situations where you may use `git reset`:
* "I accidentally staged the password file!": `git reset HEAD ./passwords.txt` _(please don't make a passwords file)_
* "I've made a bunch of changes but I don't want them anymore because the mocks changed! Please help": `git reset HEAD --hard`

[helpful resource for --soft vs. --mixed vs. --hard](http://stackoverflow.com/questions/3528245/whats-the-difference-between-git-reset-mixed-soft-and-hard)


## `git log`: shows previous commits
`git log`  
This is helpful if you're looking in the past.

I would recommend making an alias for this variant of git log:  
`git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short`
It presents more information, cleaner, and in less space.


## `git show`: shows particular details of commit
`git show <commit hash/ref(branch name/tag/etc...)>`  


## `git push`: push local branch to remote git repository
`git push <remote> <branch>` or `git push <remote> <local branch>:<remote branch>`  
e.g., `git push origin dev` or `git push origin feature:feature`  
The latter command only needs to be done if the branch does not already exist at the destination, otherwise just specifying the branch name is sufficient.  
**Important:** Do not "~~git push~~"! Always specify all parameters for push/pull or you may end up in git hell. _You do not want to be there_.


## `git pull`: pull remote branch to local git repository
`git pull --rebase <remote> <branch>` or `git pull <remote> <remote branch>:<local branch>`  
e.g., `git pull --rebase origin dev` or `git pull origin develop:develop`  
The latter command only needs to be done if the branch does not already exist at the destination.  
(`--rebase` is an option which puts any changes that only exist locally **after** any remote changes. This prevents local history from becoming different from remote's history. If the workflow is followed properly this shouldn't be necessary, and I _know_ that everyone will follow the workflow perfectly, right?)  
**Important:** Do not "~~git pull~~"! Always specify all parameters for push/pull or you may end up in git hell. _You do not want to be there_.

## `git merge`: combine changes from two branches into one
###### merge? Where we're going we don't need "merge"  
`git merge <branch>`  
merge the specified branch _onto_ the branch you currently have checked out.
e.g.,
```
git checkout dev
git merge css-hack
```
*We are not using merge in our workflow, this is just for reference. [This is why we don't merge](http://i.imgur.com/Owiji5N.png)*

## `git cherry-pick`: pick particular commits from another branch and play them onto current branch
###### `git rebase` is more likely to be used by you in your time with git  
`git cherry-pick <commit>...`
cherry-pick puts the specified commits _onto_ the branch you currently have checked out.
```
git checkout big-feature
git cherry-pick ef23t9e e67e2e...
```
Puts commits ef23t9e and e67e2e onto branch big-feature. You may want to use this if there is a fix to a bug on a different branch to get that change/those changes onto your current working branch.

## `git rebase`: move a branch's root to a different place on the tree, 'rebase' it.  Cherry-pick all nodes on a branch to the specified location
`git rebase <upstream> <branch>` or `git rebase <branch>`  
The first option is syntactic sugar for:
```
git checkout <upstream>
git rebase <branch>
git checkout <branch>
```
either use that syntax or be sure to explicitly checkout your rebase target first:
```
git checkout develop
git rebase big-feature
```
If you prefer the latter it is recommended that you have you command prompt display your current git branch if a git repo is present.

When a rebase is initiated you will use do the following steps:
* Resolve any merge conflicts
* Stage resolved files with `git add`
* Use `git rebase --continue` to move to the next commit being rebased
* Repeat until git tells you you're done

At any point you can type `git rebase --abort` to pretend none of this ever happened and return your working directory to the state before you began the rebase.  

Visual Studio has a good rebase tool.

# Workflow
1. Get latest from from origin develop `git checkout develop` `git pull --rebase origin develop`
2. Create a feature branch for your work item `git checkout -b <branch name>`
3. Commit your work early and often. One commit per logical change.
  * 'git add` to stage your changes
  * `git commit` to commit
4. Repeat step 3. until work is complete.
5. Re-pull latest onto develop `git checkout develop` `git pull --rebase origin develop`
6. Rebase onto develop `git rebase develop <branch name>` (is is recommended to do this as you go as to save yourself from a huge merge at the end of the line. Visual Studio has a good rebase tool).
7. Checkout your feature branch and push to origin:
  * `git checkout <branch-name>`
  * `git push origin <local-branch-name>:<remote-branch-name>` e.g., `git push origin css-tree-fix:css-tree-fix`
8. Use Visual Studio Online to create a pull request
  * You can add commits locally and push again in order to update your PR.  No need to create a new code review
  * Reviewers can see you notes on each commit and follow your progression.
  * Provided you are rebased on the latest develop VSO will squash and merge for you automatically
  
[Our model is based on this, though this uses merge --no-ff instead of squash.  A good read but don't confuse yourself needlessly.](http://nvie.com/posts/a-successful-git-branching-model/)

# Suggested Git Aliases: 

## git
`git config --global alias.<alias-name> <command>`  
e.g., `git config --global alias.co checkout`  
Or, modify the .gitconfig file directly:

```
.gitconfig:
    [alias]
        <alias-name> = <command>
        co = checkout
```

[See the docs](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases)


## powershell
[See the docs](https://msdn.microsoft.com/en-us/powershell/reference/5.1/microsoft.powershell.utility/set-alias)
_(My bash command comments below are relevant here too)_

## bash
I prefer to use bash commands because I don't have to preface my alias with "git".
i.e., "git co" can instead be "gco"

###### These are my aliases, there are many like them, but these are mine:
```bash
.bash_profile:
    gs () { git status; }
    
    ga () { git add $1; }
    
    gc () { git commit $1 $2; }
    
    gd () { git diff $1; }
    
    gb () { git branch $1 $2; }
    
    gco () { git checkout $1 $2; }
    
    gcob () { git checkout -b $1 $2; }
    
    gh () { git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short; }
    
    gpull () { git pull $1 $2 $3; }
    
    gpush () { git push $1 $2 $3; }
```

