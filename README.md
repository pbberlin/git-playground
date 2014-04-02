init
=================================================================================
apt-get install git
apt-get install gitk

git config --global user.name  "Peter Buchmann"
git config --global user.email "peter.buchmann@idealo.de"

git config --global color.ui auto
git config --global core.pager "less -FRSX"


# prevent git from pushing ALL branches that have the same name on the remote
git config --global push.default tracking  


git remote add origin_playground git@github.com:pbberlin/playground.git
git clone

# or
git clone ssh://peter.buchmann@gerrit.ipx:29418/Puppet.git


making changes
===========================================================

# creating and checking out a feature branch that tracks "origin/feature"
git checkout -t origin/feature



git branch [-d], checkout


git add xxx                            #files I want push to gerrit

git commit    -m "my commit message"   
git commit -a -m "my commit message"   # -a => committing changes to ALL tracked files


git commit  --amend			# push typos posthumously , -m omit
#[im Editor]
  <last change ID from http gerrit> ENTER ENTER [my amend comment]

git notes add   # adding to last commit - does not change history



# non puppet: 
git push
git push -u [remote_repo]            [branch]  # -u for upstream - setting up tracking , default for branch is current
git push    origin_playground         master


# in puppet: 
git push    origin        HEAD:refs/for/master

if remote changes have occurred:
===========================================================
git fetch        # do NOT use git pull (mingling fetch+merge)
git merge [branch]

git merge origin_playground/master


git mergetool
git rebase







analyse - all
===========================================================
gitk
git log
git log --oneline --decorate       #  Show branches, tags in git log

git show 50f3754                   #  info on a commit
git show :/fix                     #  the last commit which has the word "fix" in its message
git show :/^Merge                  #  shows the last merge commit
git name-rev --name-only 50f3754   #  tell us the position of a commit relative to tags
git branch   --contains 50f3754    #  Find out which branch contains a change


analyse - current
===========================================================
git status
git status -s                  # short
git diff 
git diff --word-diff           # word wise git



squashing commits
===========================================================
# see git squash and pick


going back
===========================================================
git reset --hard [target commit hash ] 
# git reset --hard 48cffce1fe


# preserve uncommitted changes when changing branches
# ===========================================================
git stash

