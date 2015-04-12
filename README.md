## Terms
HEAD is the tip of the current branch.
TIP is the most recent commit of a branch.
ORIGIN is an alias for the remote repository. It is created by git clone, during cloning

## Init Git itself

    apt-get install git
    apt-get install gitk

    git config --global user.name  "Peter Buchmann"
    git config --global user.email "peter.buchmann@web.de"

    git config --global color.ui auto
    git config --global core.pager "less -FRSX"

Prevent git from pushing ALL branches that have the same name on the remote

    git config --global push.default tracking  

### Remove a Repository
If the folder is the top level of a git repository, 
and you actually want to completely remove the entire git repo associated with the folder, 
then simply delete the (hidden) .git directory from the folder:

    cd foldername
    rm -r .git 

### Init a Repository

    git init
    git remote add     origin_playground git@github.com:pbberlin/playground.git
    git clone

or

    git remote add     origin_playground https://github.com/pbberlin/graphite-setup-debian.git
    git remote set-url origin_playground https://github.com/pbberlin/graphite-setup-debian.git
    

or

    git clone ssh://peter.buchmann@gerrit.ipx:29418/Puppet.git
    git clone ssh://peter.buchmann@gerrit.ipx:29418/Puppet.git  [target dir - distinct of {1}.git]
    git clone https://github.com/pbberlin/graphite-setup-debian.git
    
    

Create a completely new Repository 

    cd [working dir]
    git init
    git push originXX master

Change remote repository URL from https to ssh

    git remote set-url origin ssh://peter.buchmann@gerrit.ipx:29418/Puppet.git
    cd .git/hooks/
    scp -P 29418 peter.buchmann@gerrit.ipx:hooks/commit-msg .



Rename a remote repository

    git remote -v
    git remote rename [old_name]  [new_name]


Check a remote repository
    
    git remote show origin

Here we also see
* new remote branches (remotely created)
* stale remote branches (remotely deleted)
* remote "tracked" branches
* automatically pushed/merged branches


# Making Changes

creating and checking out a feature branch that tracks "origin/feature"

    git checkout -t origin/feature

    git branch [-d], checkout

    git add [files]                            # stage files, files I want push to repo ORIGIN or repo GERRIT
    git rm  [files]                            # unstage

    git commit    -m "my commit message"   

    git commit -a -m "my commit message"   # -a => committing not just the staged files - since last ADD, but ALL
    git commit  -a -m --allow-empty-message

Amend towards last commit

    git commit -a -m "my commit message"   --amend

Amend towards a gerrit repo

    git commit  --amend            # push typos posthumously ,  omit -m
    
*im Editor*
```Shell
[paste last change ID from http gerrit]ENTER
ENTER
[enter my amend comment]
```

Adding notes

    git notes add   # adding to last commit - does not change history



### Pushing - non Gerrit
    git push
    git push -u [remote_repo]            [branch]  # -u for upstream - setting up tracking , default for branch is current
    git push -u  github_pbberlin_playground    master  #  example


### Pushing - to Gerrit
Here we push to the magic branch that creates reviews that target the master branch. 
For every branch Gerrit tracks there is a magic refs/for/<branch_name> that you push to create reviews.

    git push    origin        HEAD:refs/for/master   

### If Remote Changes have Occurred:
### on CONFLICTS

    git fetch        # do NOT use git pull (mingling fetch+merge)
    git merge [repo]/[branch]

For example

    git merge github_pbberlin_playground/master      # merging remote master (after git fetch)
    git merge                            issue56     # merging local branch issue56
    git mergetool

#### if nothing else helps

    git checkout --ours   filename.c    # ours and theirs are constant options
    git checkout --theirs filename.c
    git add filename.c
    git commit -m "using theirs"

   
Instead of merging, there is also rebase.

    git pull --rebase 

Or even

    git rebase # extreme caution !

Beware of rebasing towards public repos, cause it changes previous commits and forces other contributors to repeat their merging work.



# Analyse - big picture
    gitk
    
    git branch -a
    git branch -r
    
    git log
    git log --oneline --decorate       #  Show branches, tags in git log
    git log --author="Jon"
    git log --author="\(Adam\)\|\(Jon\)"
    
    git log -- [path]
    git log -- modules/site_idealo/manifests/role/role_mysql.pp    

    git show 50f3754                   #  info on a commit
    git show :/fix                     #  the last commit which has the word "fix" in its message
    git show :/^Merge                  #  shows the last merge commit
    git name-rev --name-only 50f3754   #  tell us the position of a commit relative to tags
    git branch   --contains 50f3754    #  Find out which branch contains a change


# Analyse - current commit
    git status
    git status -s                  # short
    git diff 
    git diff --word-diff           # word wise git


## Squashing Commits
See git squash and pick

    git rebase -i HEAD~3  # last three commits
    git rebase -i master

```
Ctrl \
pick
squash
A
```
    
    git rebase --abort  # abort 
    
    


## Going Back 
    git reset --hard [target commit hash ] 
For example

    git reset --hard 48cffce1fe
    

### Preserve Uncommitted Changes when Changing Branches
    git stash


# marker


# remember login and password

http://git-scm.com/docs/git-credential-store
    git config --global credential.helper cache
    git config --global credential.helper store
