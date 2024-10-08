



# git
<p align="left">
    <a href="https://git-scm.com/" target="_blank"> <img src="https://www.vectorlogo.zone/logos/git-scm/git-scm-icon.svg" alt="git" width="40" height="40" /> </a>
    <a href="https://www.sourcetreeapp.com/" target="_blank"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/sourcetree/sourcetree-original-wordmark.svg" alt="sourcetree" width="40" height="40" /> </a>    
    <a href="https://tortoisegit.org/" target="_blank"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/tortoisegit/tortoisegit-original.svg" alt="tortoisegit" width="40" height="40" /> </a>
</p>












https://www.youtube.com/watch?v=ecK3EnyGD8o

npm is a package manager for the JavaScript programming
 -  **official**: https://docs.npmjs.com/cli/v7/commands
 - https://www.freecodecamp.org/news/npm-cheat-sheet-most-common-commands-and-nvm/
 - https://www.sitepoint.com/npm-guide/


## Git Cmds
```
git status
git pull
git fetch
git branch

git add .
git commit -m "Update .gitignore"

git checkout bug_fix_v_0.1
git checkout -f another-branch
git checkout -b new-branch commit-ssh-1d0fa4fa9ea961182114b63976482e634a8067b8

git tag Before_15Jan2021
git push --tags

git rev-parse HEAD
git rev-parse --short HEAD

git merge branchName	#Git Rebase vs Git Merge https://www.youtube.com/watch?v=Lo3u3TV7t5I
git rebase branchName

git add file.md 			#https://stackoverflow.com/questions/572549/difference-between-git-add-a-and-git-add
git add -A 					#stages all changes
git add . 					#stages new files and modifications, without deletions (on the current directory and its subdirectories).
git add -u 					#stages modifications and deletions, without new files

git push --force
git push --force-with-lease

git reset --hard						#undo all uncommitted or unsaved changes
git reset --hard HEAD
git reset --hard HEAD~1					#undo last commit or merge
git reset --hard origin/branch_name

git revert commit_small_hash

git log									#https://stackoverflow.com/a/1340245/2948523
git log --grep=13929					#where commit message like 13929
git log --author=Dipon					#where autor like dipon
git log --first-parent develop/13929	#where branch name develop/13929
git log --grep=13929 --author=Dipon --first-parent develop/13929_Product_Hiererchy_Fix --reverse --pretty=format:"%h"


git diff --name-only --diff-filter=U --relative							#List conflicted files https://stackoverflow.com/a/10874862
git config --global alias.conflicts "diff --name-only --diff-filter=U"

git rev-list							#https://manpages.ubuntu.com/manpages/bionic/man1/git-rev-list.1.html

git cherry-pick af02e0b					#https://stackoverflow.com/questions/1670970/how-to-cherry-pick-multiple-commits	
git cherry-pick d573ed1bd 21ea438e2		#multiple-commits	
git cherry-pick af02e0b --no-commit		#apply only changes without commit
										#https://stackoverflow.com/questions/15687346/git-cherry-pick-range-of-commits-and-exclude-some-in-between
										
HEAD VS Origin
git show head		#https://stackoverflow.com/questions/8196544/what-are-the-git-concepts-of-head-master-origin

git log      --grep=13929 --author=Dipon --branches develop/13929_Product_Hiererchy_Fix --reverse --pretty=format:"%H"
git rev-list --grep=13929 --author=Dipon --branches develop/13929_Product_Hiererchy_Fix --reverse origin..HEAD
git rev-list --grep=13929 --author=Dipon --branches develop/13929_Product_Hiererchy_Fix --reverse origin..HEAD | git cherry-pick --stdin
```
## Sample
**Commit Message**
```
13929: Code review fixes
-Drop previous product_hierarchy table and use new one
-Use temp tables in procedure
-Change Hierarchies to Hierarchy in C# end
-Validation fixes
```
## Git Ignore
**.gitignore samples**
https://github.com/github/gitignore


**.gitignore**
```
/tools                      # ignore everyting inside tools folder

/tools/*                    # ignore everyting inside tools folder except packages.config
!/tools/packages.config
```
 re use .gitignore file
```
git rm -r --cached .
```

## Story

**Change message of previous commit**
https://medium.com/@igor_marques/git-basics-adding-more-changes-to-your-last-commit-1629344cb9a8
```
git add -A
git commit --amend --no-edit
git push -f

git commit --amend -m "Your new commit message"		#edit the last commit
git push -f
```

**Check how file got deleted**
https://stackoverflow.com/questions/6839398/find-when-a-file-was-deleted-in-git
```
git log --full-history -- Presentation/Nop.Web/package-lock.json
git log --full-history -1 -- Presentation/Nop.Web/package-lock.json
```

**Undo a merge**
https://www.freecodecamp.org/news/git-undo-merge-how-to-revert-the-last-merge-commit-in-git/
```
git reset --soft HEAD~1					#undo last commit or merge, and changes as uncommit
git reset --soft commit-ssh				#undo all commits after commit-ssh, and changes as uncommit

git reset --hard HEAD~1					#undo last commit or merge
git reset --merge HEAD~1
```

**Rebase and recommit all the changes**
https://www.youtube.com/watch?v=kMvLn8WcAII
https://stackoverflow.com/a/68586918/2948523
https://stackoverflow.com/questions/8939977/git-push-rejected-after-feature-branch-rebase

https://stackoverflow.com/questions/25755933/force-overwrite-existing-branch-missing-from-tortoisegit-push-dialogue
```
git rebase dev
resolve conflicts if any, do a new commit
git rebase --continue

git push --force-with-lease

get last commit of dev branch 6e6f0093bf1602baf074230378fe470e6447cc6a
git rev-parse dev

git reset --soft 6e6f0093bf1602baf074230378fe470e6447cc6a
do new commit

git push --force
```

**Squash**
https://www.youtube.com/watch?v=RwvTrSm7zEY
```

```

## Errors
```
git status
```
**error: index uses extension, which we do not understand fatal: index file corrupt**
```
https://stackoverflow.com/a/1115956/2948523
del .git\index
git reset
```
**fatal: not a git repository (or any of the parent directories): .git**
```
https://stackoverflow.com/questions/20413459/fatal-not-a-git-repository-or-any-of-the-parent-directories-git
git init

```
**error: cannot lock ref 'refs/remotes/origin/Develop/Bug_14222_Top3ClimbersInActivityFeed': Unable to create 'D:/workstation/channelassist/Git/ChannelManager/.git/refs/remotes/origin/Develop/Bug_14222_Top3ClimbersInActivityFeed.lock': File exists.**
```
https://stackoverflow.com/questions/72574209/git-pull-error-cannot-lock-ref-ref-remotes-origin-xxx-exists-cannot-crea
git fetch --prune

or(tested)
Go to D:/workstation/channelassist/Git/ChannelManager/.git/refs/remotes/origin/Develop/
delete Bug_14222_Top3ClimbersInActivityFeed and Bug_14222_Top3ClimbersInActivityFeed.lock

or remove folder
D:/workstation/channelassist/Git/ChannelManager/.git/refs/remotes/origin
```

## Title
**Avout**
```
--force-reinstall
```
```
--no-cache-dir
```