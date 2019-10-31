## Welcome to Git!
#### What is git?
Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency ([!]([https://git-scm.com/](https://git-scm.com/))).

#### Some simple command for git
| Command   | Description |
| -------------  | ------------- |
| `git init`  	 | Initialize local git repository  |
| `git status`   | Check the current status of tree  |
| `git add .`    | Add all the changes file to the staging ( allow git to track those file) |
| `git add {fileName}` | Add specific file to the staging |
| `git commit -m'Message'` | commit stages changes |
| `git checkout commitNo` | To go to certain commit to see changes |
| `git reset --hard commitNo` | To reset to certain commit |
| `git checkout -- .` | To reset all unstaged changes |
| `git branch` | check branches |
| `git checkout -b branch-name` | Create a new branch |
| `git log` | See Git log of all branches|
| `git checkout master` | Change to (**master**) branch |
| `git merge (name of branch to be merged)` | Merge branches |
| `git branch -D (name of branch to be deleted)` | Delete a branch |
| `git merge --squash branch-name` | Get only the final changed info from branch to master| 

#### Stash
If we want to save the working copy of our code, we use stash

| Command | Description|
|-----------| ---------|
| `git stash` | Create stash and clean the code|
| `git stash list` | See the stash list |
| `git stash apply n` | Apply nth stash of the list|
| `git stash push -m"Message"`| Create stash with message|
| `git stash drop n` | Drop nth stash of the list |
| `git stash pop n | Apply nth stash of the list and delete the stash|
| `git stash clear` | Clear all the stashes|
