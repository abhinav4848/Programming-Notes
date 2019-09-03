# Overview

A commit in a git repository records a snapshot of all the files in your directory. It's like a giant copy and paste, but even better!

Git wants to keep commits as lightweight as possible though, so it doesn't just blindly copy the entire directory every time you commit. It can (when possible) compress a commit as a set of changes, or a "delta", from one version of the repository to the next.

# Quick Edit

Download from repository and set user.

```bash
git clone URL local-folder-name
git config user.name "NAME"
git config user.email "EMAIL"
```

Make changes and upload back.

```bash
# verbose version
git status

# compressed version
git status -s

# stage (index) all edited files
git add .

# stages only README.md
git add README.md

git commit -m "MESSAGE"

git push origin master
```

**Staging** is also known as **Index**

# Getting help

```bash
# List all the commands.
git help

# Explain a specific command.
git help COMMAND
```
## Git Status
```
 M README
MM Rakefile
A  lib/git.rb           # new files that have been added to the staging area
M  lib/simplegit.rb     # modified files
?? LICENSE.txt          # ?? New files that aren’t tracked
D  deleteme.txt         # deleted files
```
There are two columns to the output — 
* Left: indicates the status of the staging area
* Right: indicates the status of the working tree. 

So for example in that output,
* README file is modified in the working directory but not yet staged,
* lib/simplegit.rb file is modified and staged.
* Rakefile was modified, staged and then modified again, so there are changes to it that are both staged and unstaged.

A previously cloned repo on a server can now pull just the changes (clone is used only the first time to create the local repo).

`git pull` - (`git fetch` + `git merge`)

Github repo > Clone to local > Push to Github > Pull changes to server

# Tips

Master branch = Timeline

`HEAD` = Last commit on current branch.

HEAD is the symbolic name for the currently checked out commit -- it's essentially what commit you're working on top of.

Commit messages should be in the present tense instead of past. They should tell what a commit does instead of what happened. Ex. Add new module vs Added new module.

Git uses vi as the default editor.

# Setup
To start a new git project, navigate to the folder and enter: 
```bash
# Initialize an empty repo inside a .git hidden directory.
git init
```

# .gitignore

This is a list of files/folders to be ignored in commits. To use it, create a `.gitignore` file in the working directory, **NOT** inside `.git`.

Each line in the file is a new ignore rule. Ex. To ignore `node_modules/`, just add that as one line.

## Untrack tracked files
If the ignoring is added after committing the files to be ignored, they need to be untracked.

```bash
# to untrack everything
git rm -r --cached

# to untrack specific file
git rm --cached README

# now do the staging or make commit
git add/git commit
```
**Careful as this can lose progress to files.**

## Source of readymade gitignore rules
* [Github repo](https://github.com/github/gitignore)
* [Gitignore.io](http://gitignore.io)

**Glob Patterns:**
1. Blank lines or lines starting with `#` are ignored.
2. Standard glob patterns work, and will be applied recursively throughout the entire working tree.
3. You can start patterns with a forward slash `/` to avoid recursivity.
4. You can end patterns with a forward slash `/` to specify a directory.
5. You can negate a pattern by starting it with an exclamation point `!`.
6. Glob patterns are like simplified regular expressions that shells use. 
7. An asterisk `*` matches zero or more characters; 
8. `[abc]` matches any character inside the brackets (in this case a, b, or c); 
9. a question mark `?` matches a single character;
10. brackets enclosing characters separated by a hyphen `[0-9]` matches any character between them (in this case 0 through 9). 
11. You can also use two asterisks to match nested directories;  `a/**/z` would match a/z, a/b/z, a/b/c/z, and so on.

## Example 
* ignore all .txt files: `*.txt`
* but do track lib.a, even though you're ignoring .a files above: `!lib.txt`
* only ignore the TODO file in the current directory, not subdir/TODO: `/TODO`
* ignore all files in any directory named build:`build/`
* ignore doc/notes.txt, but not doc/server/arch.txt:`doc/*.txt`
* ignore all .pdf files in the doc/ directory and any of its subdirectories:`doc/**/*.pdf`

# Staging

```bash
# What changed since last commit?
git status

# Stage an untracked file for committing.
git add FILENAME

# Multiple files.
git add FILENAME FILENAME

# A folder.
git add FOLDER/

# All specific file types.
git add *.js

# ... in a folder.
git add FOLDER/*.js

# ... in whole project.
git add "*.js"

# Track everything.
git add .

# Unstage files.
git reset HEAD FILENAME
```

# View Staged and Unstaged Changes
```bash
# shows diff between staged file and current state of file
$ git diff

# Shows diff between staged file and checkout file.
# Show unstaged differences since last commit.
$ git diff --staged

# same as staged
$ git diff --cached

# Show differences between current and specific commit
git diff <sha-1>

# diff between 2 hashes
git diff <sha-1> <sha-1>
```

## Tree

```bash
# A visual tree with branch names included.
git log --oneline --decorate --all --graph

# Add the "tree" alias as a shortcut.
git config --global alias.tree "log --oneline --decorate --all --graph"
```

# Commiting
Each commit moves the HEAD further up the timeline.

```bash
# opens up editor for commit message
$ git commit

# to also see the diff
$ git commit -v

$ git commit -m "Add your commit message inline"

# Skip staging. Direct commiting. i.e. Includes all currently changed files into this commit. Untracked (new) files are not included.
$ git commit -a

# to improve upon the last commit, like overwriting it. It overwrites the commit message as well as commit Hash. i.e. It changes the commit history.
# Bad idea if you push and others have pulled it.
# So git revert is better for multiple people collaborating.
$ git commit --amend -m "Add your new commit message"
```

If the `-m` is ommited, the screen will move to the `vi` text editor, which can be exited with `:q`.

# Tagging

A reference to a specific commit, used mostly for release versioning.

```bash
# Create tag.
git tag -a v0.0.1 -m "Version 0.0.1"

# List tags.
git tag

# Open a specific version.
git checkout v0.0.1

# Push tags to remote repo.
git push --tags
```

# Undo

DON'T DO THESE AFTER PUSHING!

## Unstaging
```bash
# removes all staged changes. Removed all untracked unstaged files.
git reset HEAD

# to unstage that file only
git reset HEAD <file> 
```

## Unmodifying file
```bash
# Revert a file to the last commit version.
# discard changes in working directory and restore older copy of the file from index. Otherwise last commit
git checkout <filename>

# discard entire working directory and restore older files from the index, otherwise last commit
git checkout .
```

## Moving commit from one branch to another
Useful if you accidentally commit to the wrong branch. So 2 things:
1. You need to copy the commit to actual branch
2. You need to undo the commit from current branch

```bash
# get the hash of the commit you want to copy elsewhere
git log

# checkout the actual branch
git checkout actual-branch

# git cherry-pick duplicates the commit into this branch
git cherry-pick <sha-1>
```

## Uncommiting
```bash
# UNDO commit and move everything back to staging. The carrot on the HEAD means move to the previous commit. 
git reset --soft <sha-1>|HEAD^

# mixed reset is default option. 
# This UNDOes commit and moves everything back to WORKING DIRECTORY
git reset <sha-1>

# DELETE everything after the specified commit.
# it reverts all the tracked files to the state they were
# but leaves untracked files alone
git reset --hard <sha-1>

# HEAD^^ means DELETE the last 2 commits. 
# i.e. it undoes the commit and makes everything look like the specified commit.
git reset --hard HEAD^^

# get rid of untracked files
# d: rid of untracked directories
# f: force
git clean -df
```

## Undo Git reset hard
```bash
# show every action taken in git
git reflog

# checkout the particukar hash you want to restore to
git checkout <sha-1>
```
Now we'll end up at a detached HEAD state. So we risk losing the files. So create a new `backup` branch of this state. So now checkout to master, and merge.

## REVERT
safe for multi user git project
```bash
git revert <sha-1>
```

## Misc
```bash
# Force change on GitHub. Git interprets the "^" after the hash as the parent of this very commmit, and the "+" as a force push.
git push origin +hash^:master
```

# Delete Files
`$ git rm cat.txt` deletes the file from directory, as well as stages it. If you manually remove file, then you'll have to stage it as well.

You can pass files, directories, and file-glob patterns to the `git rm` command. That means you can do things such as:
`$ git rm log/\*.log`

Note the backslash `\` in front of the `*`.

This is necessary because Git does its own filename expansion in addition to your shell’s filename expansion. This command removes all files that have the .log extension in the log/ directory. Or, you can do something like this:
`$ git rm \*~`

This command removes all files whose names end with a `~`.

# Rename file
```bash
# summary function for "mv", then "rm", then "add"
git mv file_from file_to
```

# View Commit History
* `$ git log`
* `-2` # shows last 2 commits
* `--stat` # shows more detail with each commit. Tells which files were changed within the commit.
* `--graph`
  * `--since=2.weeks` # Commits since past 2 weeks
  * `--until=1.weeks`
  * `--relative-date` # Display the date in a relative format (for example, “2 weeks ago”) instead of using the full date format.
  * `--pretty=oneline`
  * `--pretty=format:"%h - %an, %ar : %s"`

**Useful options for `git log --pretty=format`**

| Option | Description of Output                           |
| ---    | ---                                             |
| %H     | Commit hash                                     |
| %h     | Abbreviated commit hash                         |
| %T     | Tree hash                                       |
| %t     | Abbreviated tree hash                           |
| %P     | Parent hashes                                   |
| %p     | Abbreviated parent hashes                       |
| %an    | Author name                                     |
| %ae    | Author email                                    |
| %ad    | Author date (format respects the --date=option) |
| %ar    | Author date, relative                           |
| %cn    | Committer name                                  |
| %ce    | Committer email                                 |
| %cd    | Committer date                                  |
| %cr    | Committer date, relative                        |
| %s     | Subject                                         |

# Workflow

it's generally considered good practice to avoid merges where possible.

A good workflow is to leave the `master` for production and do all the coding in a separate `develop` branch. The `develop` branch can expand into `feature` branches which would later be merged back. When a certain stable version is reached in `develop`, it can be merged with `master` along with a version tag.

Additonal brances such as `release` and `hotfix` can be introduced between the `master` and `develop` ones.

If a branch is not shown, it's due to fast-forwarding i.e. no changes were made in the branch being merged into, hence the simpler merge log.

# Example Project

```
*   e67ccc4 (HEAD -> master) Merge branch 'header'
|\
| * ce37b16 (header) Fix original header
* |   707692c Merge branch 'footer'
|\ \
| * | 2dc6e90 (footer) Expand footer
| * | fd61a63 Create footer.html
* | | 8083e87 Add fifth bla in CAPS
* | | 8d56117 Add fourth bla in index.html
* | | 86c50f0 Added third bla in index.html
|/ /
| | * 26326bb (header2) Fix header2 version
| | * d83bebc Modify header into version 2
| |/
| * c2d4091 Create header.html
|/
* 9524ae9 Create index.html
```

# Configuration

```bash
# Setting up owner of changes for all repos. Github contributions count only with the github email.
git config --global user.name "NAME"
git config --global user.email "EMAIL"
```

# Remote Repos
A project can have multiple remotes ex. origin, test, production...

```bash
# Add a remote i.e. bookmark a repo i.e. This NAME = this URL. The name is usually "origin", but it can be anything.
# Eg: Linking a remote server with my local repo and using "origin" as an identifier name for it
# git remote add origin https://github.com/abhinav4848/test.git 
git remote add <NAME> <URL>

# List all remotes.
# shows the URLs that Git has stored for the shortname to be used when reading and writing to that remote
# Eg: 
# origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
# origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
git remote -v

# Check a remote.
git remote show origin

# Stay up to date with a remote ("upstream") in your "master" branch
git pull upstream master

# Remove a remote.
git remote rm NAME
```

## Contrib to open source stuff
Create a fork, go your own fork, clone it to pc, create a new branch, work on it and push. Create a pull request on the original project.

**Keeping forks up to date**: [Syncing a fork](https://help.github.com/en/articles/syncing-a-fork)

# Clone

```bash
# Create a local repository from a remote one.
# this command implicitly adds the `origin` remote for you
git clone URL

# ... with a different name.
git clone URL NEW_NAME
```

Cloning does:

-   Download entire repo into a new local one.
-   Add "origin" remote, pointing to the clone URL.
-   Check out initial branch. (Set head to master)

# Push

Define which local branch (usually master) to push to which repository (usually origin). It asks for user and pass.

```bash
# for pushing local code upto origin from "master" branch
# git push -u REPO_NAME BRANCH_NAME
git push -u origin master
```

`-u` remember the repo and the branch, so that only `git push` can be used.

# Pull

Used to update the local repo with the latest changes. Should be done often.

```bash
# pulls but doesn't merge
git fetch

# pulls and merges
git pull
```

Behind the scenes, this creates an origin/master branch which is automatically merged into the master one, unless there is a merge conflict.

## Pull Requests

Once someone completes a feature, they don’t immediately merge it into master. Instead, they push the feature branch to the central server and file a pull request asking to merge their additions into master.

This gives other developers an opportunity to review the changes before they become a part of the main codebase.

You can think of pull requests as a discussion dedicated to a particular branch.

For example, if a developer needs help with a particular feature, all they have to do is file a pull request. Interested parties will be notified automatically, and they’ll be able to see the question right next to the relevant commits.

Once a pull request is accepted, the actual act of publishing a feature is much the same as in the Centralized Workflow. First, you need to make sure your local master is synchronized with the upstream master. Then, you merge the feature branch into master and push the updated master back to the central repository.

# Branches
**Do not mess with the master**. The master branch is deployable production code, meant to be stable. Instead, work on new features in separate branches, which would then be `merged` or `rebased` into master. 

**Branches are local**, meaning they cannot be worked on at the same time.

Branches in Git are incredibly lightweight as well. They are simply pointers to a specific commit -- nothing more. This is why many Git enthusiasts chant the mantra: **branch early, and branch often**.

Because there is no storage / memory overhead with making many branches, it's easier to logically divide up your work than have big beefy branches.

A branch essentially says "I want to include the work of this commit and all parent commits."

Switching branches will only show the files in that branch.

#### Create Branch

```bash
# Create new branch. HEAD still on master (Use checkout to switch).
git branch <name>

# Creating a new branch and switching to it at the same time
git checkout -b <branch_name>

# Create a remote branch. Usually origin.
# Eg: git push -u origin personal.  We only need to use -u once to link local branch to remote and track it too.
git push <repo_name> <branch_name>
```
From example, we see that `-u` causes `Branch 'personal' set up to track remote branch 'personal' from 'origin'.`

#### Navigate Branch

```bash
# Check which branch we are on.
git branch

# Check remote branches.
git branch -r

# Move to a specific branch (Set HEAD from master to <branch_name>). This is like switching timelines.
git checkout <branch_name>
```

#### Delete Branch

```bash
# Delete a branch.
git branch -d <branch_name>

# Delete a remote branch
git push <repo_name> --delete <branch_name>
```

## Merging Branch
**The merging is done from the perspective of where we merge `INTO`.**

Merging in Git creates a special commit that has two unique parents. A commit with two parents essentially means "I want to include all the work from this parent over here and this one over here, and the set of all their parents."

```bash
# Move (checkout) to the branch (master) you want to merge into
git checkout master

# and use:
git merge BRANCH_NAME

# Example:
git merge hotfix # Merge "hotfix" to "master"

# delete "hotfix" branch now that it's merged to "master"
git branch -d hotfix
```
`master` now points to a commit that has two parents. If you follow the arrows up the commit tree from master, you will hit every commit along the way to the root. This means that master contains all the work in the repository now.

Merging is very easy if the master branch is not modified. This is fast-forwarding.

If both branches were modified, a commit is created to do the merge. (Vi editor opens for the message)

```
0
|\
2 1  - 2 master, 1 branch
|/
3    - master assimilates branch
```

"Fast Forwarding" occurs when the trunk can automatically move ahead to the branch without any merge conflicts because they are in the same path of development.

*From the book:* It’s worth noting here that the work you did in your hotfix branch is not contained in the files in your issue53 branch (which diverged from master before hotfix was merged into master). If you need to pull it in, you can merge your master branch into your issue53 branch by running git merge master, or you can wait to integrate those changes until you decide to pull the issue53 branch back into master later.

### Merge Conflict
After a merge conflict occurs, type `git status` to see which files are pending merger due to conflict.

*Example Output:*
```
<<<<<<HEAD:file.txt
here is conflicting line in Head's file.txt
========
here is conflicting line in branch's file.txt
>>>>>> branch:file.txt
```

After correcting the conflict, stage and commit

## Branch Management
```bash
# with no arguments, you get a simple listing of your current branches. The checked out branch will have a * next to it (HEAD)
git branch 

# see the last commit on each branch
git branch -v

# To see which branches are already merged into the branch you’re on
git branch --merged

# what is not merged into the master branch?
git branch --no-merged master 

# To see all the branches that contain work you haven’t yet merged in, you can run git branch
git branch --no-merged 

# Trying to delete an unmerged branch will fail
# but deleting a merged branch with no HEAD is no problem
git branch -d testing
```

## Push to remote
```bash
# push to origin branch called "serverfix"
`$ git push origin serverfix` 
# in case the remote branch had a different name
`$ git push origin serverfix:awesomebranch`
```

## Rebase

**The "merging" is done from the perspective of where we merge `FROM`.**

The second way of combining work between branches is rebasing. Rebasing essentially takes a set of commits, "copies" them, and plops them down somewhere else i.e. it serializes the history if you do not want a lot of branches in the final history.

While this sounds confusing, the advantage of rebasing is that it can be used to make a nice linear sequence of commits. The commit log / history of the repository will be a lot cleaner if only rebasing is allowed.

Move (checkout) to the branch (BRANCH_NAME) you want to merge from, and use:

```bash
git rebase master
```

**This makes it look like two features were developed sequentially, when in reality they were developed in parallel.**

```
0
|\
1 1  - 1 branch
|
2    - 2 master
|
3    - branch (1) was added to the original timeline.
```

# Git Stash
https://www.youtube.com/watch?v=KLEDKgMmbBI&list=PL-osiE80TeTuRUfjRe54Eea17-YfnOOAx&index=3


# Useful

While we all used git (and Github) in college to collaborate and save code, we mainly stuck to the the basic "add, commit, push" flow and didn't really go outside of that much, mainly because we didn't know it all that well at the time. But looking back after 2+ years of using [git] in a professional environment, here was out shortlist of things we wish we knew git could do (in order of importance)

1. **git reset HEAD**  
   If you could learn one thing about git besides how to commit and push, this would be it. When you commit to git, those commits generate a log that is identified by a commit hash. By using the command `git reset HEAD <commit-hash>` (there are other variants, this is the most straightforward), you can essentially go back in time in your code. What this is great for is if you are in a point in your project where you are pretty certain _you're going to fuck something up_, committing is like doing a quick save in a video game right before a boss fight. Something goes wrong, reset to the last commit.

2. **Branching and Pull Request Workflow**  
   In college we all worked off a single branch pushing our code up to save it. While this worked, the occasional push by a teammate who didn't properly test their code caused our project to crash and one of us having to fix it (this usually happened <12 hours before deadlines). Branching and pull requests would have eliminated that. Not only are they working on a different branch so pushing up won't mess with the original copy. When they want to merge their code, going through a PR (pull request) process will involve the code getting reviewed by another person to make sure it all works.

3. **git init**  
   When I first started with git I always thought that you had to have a remote somewhere to use it (ie on github or gitlab). As I have learned that is not the case, you can initialize a git repository in any directory and commit any files, you are only restricted from pushing until you add a remote (duh). This is great to pair with the 1st tip, even if you are working by yourself. Having the ability to go back if you mess something up is essential.

TBH that's really it. Git is a _very_ complex application that even I haven't fully learned yet. However, it has a fairly easy learning curve letting you take advantages of the most important bits quickly. If you have any questions feel free to post below and I'll be happy to answer best I can.

---

Sources:
* [8483](https://github.com/8483/notes/blob/master/topics/git.md)
* [Git-Scm](https://git-scm.com/book/en/v2)