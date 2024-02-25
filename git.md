## git
---

#### setup

    git config --global user.name “[firstname lastname]”      # set your name
    git config --global user.email “[valid-email]”            # set your email address
    git config --global color.ui auto                         # set automatic command line coloring

#### start

    git init           # initialize the current directory as a git repository
    git clone [url]    # retrieve an entire hosted repository
    git clone d1 d2    # clone directory d1 into directory d2

#### staging

    git status               # show modified files in working directory
    git add [file]           # stage a file in prep to commit
    git reset [file]         # unstage a file while keeping the working directory changes
    git diff                 # diff of what has changed, but not staged
    git diff --staged        # diff of what is staged, but not committed
    git commit -m 'message'  # commit staged content

#### branch & merge

    git branch                # list all branches. '*' marks the current branch
    git branch [branch-name]  # create a new branch
    git checkout              # switch to another branch and check it out to the current directory
    git merge [branch]        # merge the specified branch history to the current branch history
    git log                   # show all commits on the current branch

#### logs and diffs

    git log                   # show all commits on the current branch
    git log poc..main         # show the commits on branch main that are not on branch poc
    git log --follow [file]   # show all the commits of a file (works across moves/renames)
    git diff main..poc        # show the diff of what is in branch poc, but is not on branch main
    git show [SHA]            # show any object in git in human readable format

    git log --pretty=oneline  # git log entries as brief one liners

    # a log format w/ a little more readability
    git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit

    # make an alias for the log format above
    git config --global alias.logline "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
    # then
    git logline

#### delete or move

    git rm [file]                             # delete file from project and stage removal for commit
    git mv [current location] [new location]  # move a file to a new location and stage for commit
    git log --stat -M                         # show all commit logs with path changes

#### ignoring

    .gitignore                                    # repository wide ignore patterns
    git config --global core.excludesfile [file]  # system wide ignore patterns

#### sharing

    git remote add [alias] [url]  # add git url with alias
    git fetch [alias]             # fetch all branches from a remote repository
    git merge [alias]/[branch]    # merge a remote branch to your current branch
    git push [alias] [branch]     # push local branch commits to the remote repository branch
    git pull                      # fetch and merge any commits from the remote branch

#### rewrite

    git rebase [branch]           # apply any commits of current branch ahead of specified one
    git reset --hard [commit]     # clear staging area, rewrite the working tree from specified commit

#### stashing

    git stash         # save modified and staged changes
    git stash list    # list stashed file changes
    git stash pop     # pop a stash off the stack to the working directory (return most recent stash)
    git stash drop    # discard the changes from the top of stack (drop most recent stash)

#### tagging

    git tag <tag-name>                   # create a tag
    git tag -a <tag> -m "message"        # create annotated tag
    git tag -a <tag> HEAD -m "message"   # create annotated tag for last commit
    git tag                              # list tags
    git tag -n                           # add details
    git describe                         # get the latest tag
    git checkout tags/<tag> -b <branch>  # checkout a tag

#### branch/merge

    git branch NEWBRANCH                        # create a new branch
    git checkout BRANCH                         # or in one step ( git checkout -b NEWBRANCH )
    # modify add ...
    git checkout main                           # go back to main
    git merge BRANCH                            # merge changes

    git log --oneline --decorate                # see where branch pointers are pointing
    git log --oneline --decorate --graph --all  # see where branch pointers and  history divergence
