
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


git reset --hard HEAD
git reset --hard origin/branch_name
git revert commit_small_hash

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

## need to add
```
--force-reinstall
```
```
--no-cache-dir
```
```
pip install pip-review
pip-review --interactive
https://stackoverflow.com/a/56798606/2948523
```
