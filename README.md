# git-guide
Git lunch and learn supplementary material

[When in doubt](http://lmgtfy.com/?q=stack+overflow+git)


## Git Commands:

#### `git status`: list files changed since last commit
You will see staged and unstaged changes.  Staged changes are what will be commited when running `git commit`  
* You can stage changes using `git add <file/directory>`  
* You can unstage changes using `git reset HEAD <file/directory>`
* You can **permanantely undo _unstaged_ changes** using `git checkout -- <file/directory>`
  * **Uncommited changes cannot be retrieved.**
---

#### `git diff`: compare file contents since last commit
`git diff`  
Shows the lines added/removed locally since your last commit. Visual Studio's diff tool is also good.  
You can also use git diff to [compare between branches](http://stackoverflow.com/questions/9834689/comparing-two-branches-in-git)
---

#### `git add`: add selected file(s) to the staging area  
`git add <dir/file/wildcard>`  
e.g., `git add ./assets/` or `git add readme.md` or `git add *.txt`
---

#### `git commit`: take a snapshot of the current state of the repository and adds it to the current branch
`git commit`  
e.g., `git commit`
Opens your [chosen editor](http://stackoverflow.com/questions/2596805/how-do-i-make-git-use-the-editor-of-my-choice-for-commits) so you can create your commit message.  Save and exit when finished.  [Commit messages should be detailed](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html).  Use this opportunity to make your changesets clear for review. Reviewing by commit is simpler than reviewing by the entire changeset _if the commit messages are detailed and clear_.
---

#### A quick tangent to define a phrase or two
##### `ref` is reference.  This can be a commit, a branch name, a tag, etc...
##### `HEAD` is a ref that points to wherever it is you're looking. Typically this is the tip of the current branch, but that is not always the case.  See `git reset`.
---

#### `git tag`: mark a commit to be accessed by a particular name, e.g., '1.0'
A tag is like a branch that cannot be moved.  It is a ref to a commit-hash.

* list tags with `git tag`, search with `git tag -l "<search-params>"` e.g., `git tag -l "1.*"`  
* create tags with `git tag <name>`. Tags current commit
* create annotated tag with `git tag -a <name> -m <message>` e.g., `git tag -a v1.0 -m "Launch of Product"
..* it is advised to tag launches with annotated tags because the timestamp of the commit AND the timestamp of the tag are both saved. In case you want to know when a version was given to a commit, and not just when the commit itself was made.

[more on tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging)
---

#### `git branch`: create a branch to append commits to
`git branch <branch-name>`  
e.g., `git branch css-fix`
This creates a ref which points to a particular commit-hash. Namely whichever commit you were on when you created the branch.
**Important:** This command does not change you to your newly created branch.  See `git checkout` below

---
#### `git checkout`: change working directly to model previously made commit
`git checkout <commit-hash/ref(branch-name/tag/etc...)>`  
e.g., `git checkout feat-branch` or `git checkout ef32eo`

* If you want to make a new branch **and** move to it in one step use `git checkout -b <branch-name>`

This command cannot be made with changes in your working directory.
* If you do not want them anymore, run `git checkout -- .` to **permanently** discard these changes.
.. * _You can also do this with the reset command, see below_
* If you do want them, commit them.
* If you do want them, but not _that_ bad, run [`git stash`](https://git-scm.com/docs/git-stash)


---
#### `git reset`: move HEAD to the designated location, optionally modify working directory
`git reset <commit-hash/ref(branch-name/tag/etc...)> <option>`  
This is used primarily with 3 settings
* --soft: Any differences between your current state and the ref you're moving to are retained.  _Including staged changes_.
* --mixed (default): Any differences between your current state and the ref you're moving to are retained. _All staged changes are unstaged_.
* --hard: The working directory is set to exactly model the ref in question, any uncommited/unstashed changes are lost

Situations where you may use `git reset`:
* "I accidentally staged the password file!": `git reset HEAD ./passwords.txt` _(please don't make a passwords file)_
* "I've made a bunch of changes but I don't want them anymore because the mocks changed! Please help": `git reset HEAD --hard`

[helpful resource for --soft v --mixed v --hard](http://stackoverflow.com/questions/3528245/whats-the-difference-between-git-reset-mixed-soft-and-hard)

---
#### `git log`: shows previous commits
`git log`  
This is helpful if you're looking in the past.

I would recommend making an alias for this variant of git log, because it presents more information, cleaner, and in less space:
`git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short`

---
#### `git show`: shows particular details of commit
`git show <commit-hash/ref(branch-name/tag/etc...)>`  

---
#### `git push`

---
#### `git pull`

---
#### `git merge`

---
#### `git cherry-pick`

---
#### `git rebase`


## Suggested Git Aliases: 

### .gitconfig: 
#### `git config --global alias.<alias-name> <command>`
e.g., `git config --global alias.co checkout`  
Or, modify .gitconfig file directly:

.gitconfig:
```
[alias]
    <alias-name> = <command>
    co = checkout
```

[See the docs](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases)


### bash
I prefer to use bash commands because I don't have to preface my alias with "git".
i.e., "git co" can instead be "gco"

###### These are my aliases, there are many like them, but these are mine.
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

### powershell
[See the docs](https://msdn.microsoft.com/en-us/powershell/reference/5.1/microsoft.powershell.utility/set-alias)

