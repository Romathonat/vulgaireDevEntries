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

## Rebase on main
``` bash
git checkout main
git pull
git checkout illustration_workflow
git rebase main
```
If we have conflicts, handle them manually

## Push
``` bash
git push -u --force-with-lease origin illustration_workflow
```
We can configure git to directly send the current branch without having to specify those arguement (simple "git push"):  
``` bash
git config --add --bool push.autoSetupRemote true
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
