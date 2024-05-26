## Local branch creation
``` bash
git checkout main
git pull
git checkout -b illustration_workflow
```
## Commit 
``` bash
git add compute_sessions.py
git commit -m "illustration du workflow"
```

## Revert
Add a commit that reverts previous changes (that way we can create a new tag to deploy anew a previous version)
``` bash
git revert HEAD
```

## Rebase on main
``` bash
git checkout main
git pull
git checkout illustration_workflow
git rebase main
```
We can also use git fomo (see below aliases)
If we have conflicts, handle them manually, then
```
git push -f
```

## Push
``` bash
git push -u --force-with-lease origin illustration_workflow
```
We can configure git to directly send the current branch without having to specify those arguement (simple "git push"):  

``` bash
git config --add --bool push.autoSetupRemote true
```
or for older git version:  

``` bash
git config --global push.default current
```

## Reseting
``` bash
git reflog
git reset HEAD@{index}
```
git reset --hard HEAD to delete permanently modifications made after HEAD

## Add a small fix to last commit
``` bash
git commit -am --amend --no-edit
```
**Warning** Only on local commit that have not been pushed !
To change only the commit message:  

``` bash
git commit --amend
```

## Move the last commit from main to another branch
``` bash
git checkout new_branch
git cherry-pick master
git checkout master
git reset HEAD~ --hard
```

## Aliases
```
[alias]
     fomo = !git fetch origin main && git rebase origin/main
     ci = commit
     co = checkout -b
     st = status -sb
     sts = status -s
     br = branch
     tip = log -n 1 --abbrev-commit --decorate
     lol = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
     lola = log --graph --decorate --pretty=oneline --abbrev-commit --all
     unstage = reset HEAD
     cp = cherry-pick
     cam = commit -am
     last = log -1 --stat
     cl = clone
     dc = diff --cached
     lg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %Cblue<%an>%Creset' --abbrev-commit --date=relative --all
     dt = diff-tree --no-commit-id --name-only -r
     pushf = push --force-with-lease
     last = log -1 --stat
     oups = commit --amend --no-edit
     unadd = reset HEAD
     nvm = reset --hard HEAD
```

