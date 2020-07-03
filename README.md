# GIT - policy

We are using master branch as our main branch. Its requires to create your sub-branch based on master everytime and merge back to master with PR. Once branch is merged, please delete the sub-branch from github. 


## **Best practice of GIT:**  :wink:
1. Git pull to update your master and always branch out from master
2. Commit code to your sub-branch daily.
3. If your sub-branch contains lots of changes, dont select "delete it" on your PR. Clean up later.
4. Always keep your local branch in your local computer and cleanup later when you have time.
5. Visit github every month to cleanup your old branches that you dont need
6. Always add ticket number in the commit message
7. Our main branches are test/dev, stage, master.  Ideally, there should not be more than these branches in the git repo
8, Before you merge, use squash and merge or rebase to merge

## Git readme format:
[format](https://help.github.com/en/github/writing-on-github/basic-writing-and-formatting-syntax)

## logic flow:
[symbols](https://www.gliffy.com/blog/how-to-flowchart-basic-symbols-part-1-of-3)


## Sub-Branch Naming:
```
T-XXX-title-of-the-ticket or T-XXX-summary-of-your-ticket
```

## Commit message formate:
```
<ticket number>: summary
```

## Git PR template:
```
Title for your PR: OS-XXX-title-of-the-ticket

Description: 
Summary:
1. xxx
2. xxx
3. ...

Resolved Issues:
<url>

Testing Done:
manual tests / unit tests

Length Justification:
N/A

Review Checklist:

- [x] Naming convention
- [x] Add ReadMe instructions
```

# Git Common Commands

## update all git repo in a folder
```
for D in ./*; do
    if [ -d "$D" ]; then
        cd "$D"
        git checkout master
        git pull
        cd ..
    fi
done
```

## Root Git Release
```
cd root
git branch release1
git pull --recurse-submodules
cd app1
git checkout release1
cd ..
git add app1
cd app2
git checkout release1
cd ..
git add app2
…
git commit -m "Release release1"
git push
```

## Change git location
1. make sure your code is up-to-date 

```
git checkout master
git pull
```
 

2. change to new URL and push your code to the branch

```
git remote set-url origin https://xxxx.git
git remote -v 
git push 
```

## Change git URL and sync 

```
git remote set-url origin https:.//xxx.newurl.git 
git remote -v
git pull
```
## Setup SSH with github
1. generate private and public key if needed
https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
2. add key to github
https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account


## Change git URL from https to SSH or Vice versa
https://help.github.com/articles/changing-a-remote-s-url/ 


## Undo last push to remote
with checkout the branch you want to recall the push

```
git reset HEAD^ --hard
git push origin -f
```

## Undo last commit in local (use one of the following)
```
git reset HEAD@{1}
git reset --soft HEAD^
```

## Submodule [ https://git-scm.com/book/en/v2/Git-Tools-Submodules ]
### Clone with submodule
```
git clone https://github.com/chaconinc/MainProject
git submodule init
git submodule update

OR:
git clone --recurse-submodules https://github.com/chaconinc/MainProject
```

## Delete submodlue
```
to really remove a submodule:
1. git submodule deinit -f path_of_the_submodule
2. rm -R path_of_the_submodule
3. Remove the entry from .gitmodule
4. commit (very important, if the file is changed but uncommited git still consider the module as existing)
```

### Create new submodule
Git create new submodule
```
git submodule add https://github.com/chaconinc/DbConnector
git status
git diff --cached DbConnector   (git will not track the diff for submodules)
git diff --cashed --submoudle
```
Git commit submudle
```
git commit -am ":added DbConnector module
git push origin master
```


Git pull with submodule
```
git submodule update --init --recursive
```
Update submodules, use of one of the following:
```
git submodule update --recursive --remote
git pull --recurse-submodules
```

Git fetch update for submodule 
```
git submodule update --remote DbConnector
git log -p --submodule
```


Git merge or rebase
```
git submodule update --remote --merge
git submodule update --remote --rebase
```

## Make Change on Wrong branch or switch to another branch and stash local change for later
currently checkout develop branch and made some changes locally. But you actually want to make this change in a new sub-branch "features". Use `git stash` to save all changes. 
```
git stash 
git checkout -b features
git stash pop  OR git stash apply
```

## Squash Git Commits for your own branch
You have worked on your own branch and have lots of commits and you would like to clean up the commit message and combine them together as one big commit before merge to main branch. For example, combine the last 10 commits from your current HEAD. 

```
git reset --soft HEAD~10 && git commit -m "new message"
git push --force
```
reference:
https://stackoverflow.com/questions/5189560/squash-my-last-x-commits-together-using-git


## Discard All your Local Changes
`git reset --hard`


## Delete all none-branch files in your local
`git clean -f -d`


### current branch is based on dev and need to set base branch of your current branch to master

`git rebase --onto origin/master dev`


## **Cherry Pick** 
Case1: in my feature branch (eg. feature2/COM-3b), there are couple of latest changes (commit b, c, d) that needs to cherry-pick to develop branch. 

Good instructions using **source tree**: https://www.youtube.com/watch?v=O9giwMWpxLM
Here is the **git_bash** commands:
1. `git checkout develop`
1. `git pull`
1. `git checkout -b newBranch`
1. `git cherry pick 92627d1c6d5d02dc1e61fe1260be5e42b92a372f^..063893548c373f2d080c02c3849a4ddd1e06e43e`    (SHA1 of **b^..d**)
 _(error message and warning: resofving the conflicts and use git add ... or git rm ..)_
1. `git status`
 _(see the list of the files that has conflict and you need to solve the issue one by one. If you view the files and would like to take all the changes from b,c,d commits. use the following command)_
1. `git checkout --theirs path/file1 path/file2 path/file3`  (**theirs** = versions in feature2/COM-3b; **ours** = your current branch version)
See more here: https://easyengine.io/tutorials/git/git-resolve-merge-conflicts/
1. `git add path/file1 path/file2 path/file3`
1. `git rm path/file4`
1. `git push --set-upstream origin newBranch`  (this branch will be based on develop + b, c, d)

## Checkout Tag and create sub-branch
```
git checkout tag/<tag name> -b <sub-branch-name>
```
### List tags
```
git tag
```
### See a tag data along with the commit that was tagged
```
git show <tag name>
```
### Create a new tag
```
git tag <tag name>
```
### Tag an older commit
```
git tag -a <tag name> <checksum>
```
### Push a tag to remote
```
git push origin <tagname>
```
### Delete a remote tag
```
git push --delete origin <tag name>
```

## **Merge and solve conflict** Git bash command
**Case1: feature1 feature branch has its own config and does not want to commit to develop (feature1->develop)**

```
git checkout develop
git pull
git checkout -b merge feature1
git merge feature1
```

 _[conflict file localsetting.json and we would like to keep develop branch version]_

```
git checkout --ours localsetting.json
git checkout --ours -- .  (for all conflict files)
git status
```

<details>
<summary><i>[when we would like to keep <b>feature1</b> branch version]</i></summary>

```
git checkout --theirs localsetting.json
git checkout --theirs -- .  (for all conflict files)
git status
```
</details>

_[web.config also needs to use develop branch version]_

```
git reset web.config
git checkout web.config
git status
git add .
git commit
git push --set-upstream origin mergefeature1
```
Last, create PR merge feature1 -> develop

**Case2: sync feature2-api branch with develop branch, but local settings files need to use the version in feature2-api**
```
git checkout feature2-api
git pull
git checkout -b mergefeature2
git merge develop
```

 _[conflict file localsetting.json and we would like to keep feature2-api branch version]_

```
git checkout --ours localsetting.json
git status
```
Review the settings file if anything you would like to update and update it. 

_[web.config also needs to use feature2-api branch version]_

```
git reset web.config
git checkout web.config
git status
git add .
git commit
git push --set-upstream origin mergefeature2
```
Last, create PR mergefeature2-> feature2-api


## Git UI
Couple of options for Git GUI.​

### Free

GitX - https://rowanj.github.io/gitx/​​

GitUp - http://gitup.co/​

SourceTree - https://www.sourcetreeapp.com/​


### Paid

Tower - https://www.git-tower.com/​​


## Branching Strategy
Team works and creates feature branches off of `develop` branch.  The `master` branch is updated for production releases.


## Branching Convention
### Feature
Branch with name: `feature/<JIRA ID or short description of feature>`

### Bug
Branch with name: `issue/<JIRA ID or short description of bug>`

When conflict occurs during merging after the peer review process, it is highly recommended to rebase against said branch and resolve any conflicts.
In the event where there are a lot of commits to fix in the rebase process, please use interactive rebase to squash the commits into one commit and then resolve the conflicts.
More information about rebase is covered below under 'Advanced Git Techniques'.


## Releases
Recommended convention: maintain releases via git tags, an overview can be found here: https://git-scm.com/book/en/v2/Git-Basics-Tagging

Release tag naming:  `release/<major version>.<release version>[.patch id]`

​
## Advanced Git Techniques
### Force
'-f' refers to force. This can be applied to a lot of Git commands. Appending this option to the command tells it to ignore any warnings of rewriting history.

This is to be used with caution. However, there may be times where you're required to do so, like below.


### Cherry-Picking
​'cherry-pick‘ When you have a commit from another branch that you would like to commit on your current branch, cherry-pick is the right tool for you.

**Usage:**
`git cherry-pick <sha>` on the branch you want applied to.​ There might be conflicts within the files, and after resolving the conflicts, stage the final changes and run git cherry-pick --continue.

**some example on multiple Cherry-picking**
https://stackoverflow.com/questions/1670970/how-to-cherry-pick-multiple-commits


**​Original**
![Cherrypick-Original.png](.attachments/Cherrypick-Original-db6d73d7-ce30-4602-a075-c80a2ec6bd93.png)

**​Cherry-picking commit 'C'**
![Cherry-picking.png](.attachments/Cherry-picking-46d09f3b-52b9-493e-8809-8c843d4dfa97.png)​​​

Note: After cherry-picking, the newly created commit has a new SHA, denoted in the diagram as F.


Remember the `--continue`, we are going to use this a lot for the next section.


## ​​Rebasing (Cherry-Picking++)
​​What happens when you need to cherry-pick multiple commits onto one branch, that is when you use the rebase command.

On the branch with the changes you want, `git rebase <name of base branch where you want the changes applied>`.

This is equivalent of getting the SHA of every commit and cherry-picking them on the branch. As you might expect, there may or may not be conflicts for each commit applied. After resolving the conflcts and staging the commit, use `git rebase --continue` to continue to the next commit.

For a branch with a lot of commits and a lot of conflicts, this can be a nightmare to deal with.

### Interactive Rebasing
For that, let's use interactive rebase '-i'.

Interactive rebase is a very useful tool, in this section, I'll mainly cover the squash to deal with the 'merging commits'. You are able to change the commit message, reorder commits, and even delete specific commits.

To start, find a sha that is below the commits you want to squash.

`git rebase -i <sha>`
This will load up the vi editor in terminal.

You should see this: 

```
pick <sha> commit

pick <sha1> commit 2

pick <sha2> commit 3
```



There is a handy list of commands below in the editor.

We want to use 's' for squash. Change 'pick' of 'commit 2' and 'commit 3' to squash. This results in commit 3 squashing into commit 2 and then commit 2 squashing into commit, resulting in 1 commit.

At this point, doing the other rebase will be much easier.


## Difference Between Merging and Rebasing

In the above diagrams, we are trying to integrate feature branch with two commits into the main branch.

Merging will create a new commit F and the Git tree will look like so.

Rebasing "recreates" the commit C and E with new SHAs (hence F & G) on top on the current commits/


More information here: https://www.atlassian.com/git/tutorials/merging-vs-reba​​sing


## Remove a commit from PR:
https://stackoverflow.com/questions/36168839/how-to-remove-commits-from-a-pull-request


## Reference:

http://fle.github.io/git-tip-keep-your-branch-clean-with-fixup-and-autosquash.html​
