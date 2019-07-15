
`$ git clone https://github.com/libgit2/libgit2 mylibgit`

`$ git status (verbose version)`

# Staging (aka Index)
`$ git add README.md # stages README.md
$ git add . # stages all files`

`$ git status -s  # (compressed version)`
```
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```
* ?? New files that aren’t tracked
*  A new files that have been added to the staging area
* M modified files
* D deleted files

There are two columns to the output — 
* Left: indicates the status of the staging area
* Right: indicates the status of the working tree. 

So for example in that output,
* README file is modified in the working directory but not yet staged,
* lib/simplegit.rb file is modified and staged.
* Rakefile was modified, staged and then modified again, so there are changes to it that are both staged and unstaged.

# Ignoring Files
https://github.com/github/gitignore
http://gitignore.io

1. Blank lines or lines starting with # are ignored.
2. Standard glob patterns work, and will be applied recursively throughout the entire working tree.
3. You can start patterns with a forward slash `/` to avoid recursivity.
4. You can end patterns with a forward slash `/` to specify a directory.
5. You can negate a pattern by starting it with an exclamation point `!`.
6. Glob patterns are like simplified regular expressions that shells use. 
7. An asterisk (*) matches zero or more characters; 
8. [abc] matches any character inside the brackets (in this case a, b, or c); 
9. a question mark (?) matches a single character;
10. brackets enclosing characters separated by a hyphen ([0-9]) matches any character between them (in this case 0 through 9). 
11. You can also use two asterisks to match nested directories;  a/**/z would match a/z, a/b/z, a/b/c/z, and so on.

## Example 
* ignore all .a files: `*.a`
* but do track lib.a, even though you're ignoring .a files above: `!lib.a`
* only ignore the TODO file in the current directory, not subdir/TODO: `/TODO`
* ignore all files in any directory named build:`build/`
* ignore doc/notes.txt, but not doc/server/arch.txt:`doc/*.txt`
* ignore all .pdf files in the doc/ directory and any of its subdirectories:`doc/**/*.pdf`

# View Staged and Unstaged Changes
`$ git diff` # shows diff between staged file and current state of file
`$ git diff --staged`# shows diff between staged file and checkout file
`$ git diff --cached` # same as staged

# Commiting
`$ git commit` # opens up editor for commit message
`$ git commit -v` # to also see the diff 
`$ git commit -m "Add your commit message inline"`
`$ git commit -a` # Skip staging. Direct commiting. i.e. Includes all currently changed files into this commit. Untracked (new) files are not included.
`$ git commit --amend` # to improve upon the last commit, like overwriting it

# Delete Files
`$ git rm cat.txt` # deletes the file from directory, as well as stages it
If you manually remove file, then you'll have to stage it as well.

# Untrack tracked files
Keep the file in your working tree but remove it from your staging area. In other words, you may want to keep the file on your hard drive but not have Git track it anymore. This is particularly useful if you forgot to add something to your `.gitignore` file and accidentally staged it, like a large log file or a bunch of .a compiled files. To do this, use the `--cached` option:
`$ git rm --cached README`

You can pass files, directories, and file-glob patterns to the git rm command. That means you can do things such as:
`$ git rm log/\*.log`
Note the backslash (\) in front of the *.
This is necessary because Git does its own filename expansion in addition to your shell’s filename expansion. This command removes all files that have the .log extension in the log/ directory. Or, you can do something like this:
`$ git rm \*~`
This command removes all files whose names end with a ~.

# Rename file
`$ git mv file_from file_to` # summary function for "mv", then "rm", then "add"

# View Commit History
`$ git log`
`-2` # shows last 2 commits
`--stat` # shows more detail with each commit
`--graph`
`--since=2.weeks` # Commits since past 2 weeks
`--until=1.weeks`
`--relative-date` # Display the date in a relative format (for example, “2 weeks ago”) instead of using the full date format.
`--pretty=oneline`
`--pretty=format:"%h - %an, %ar : %s"`

Useful options for git log --pretty=format

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

# Unstaging
`$ git reset HEAD` # removes all staged changes. Removed all untracked unstaged files.
`$ git reset HEAD <file>` # to unstage that file only

# Unmodifying file
`$ git checkout -- <file>` # discard changes in working directory and restore older copy of the file from index. otherwise last commit
`$ git checkout .` # discard entire working directory and restore older files from the index, otherwise last commit

# Remote Repos
`$ git clone <url>` # this command implicitly adds the origin remote for you
`$ git remote add origin https://github.com/abhinav4848/test.git` # linking a remote server with my local repo and using "origin" as an identifier name for it
`$ git remote -v` # shows the URLs that Git has stored for the shortname to be used when reading and writing to that remote
`$ git push -u origin master` # for pushing local code upto origin from "master" branch

`$ git fetch` # pulls but doesn't merge
`$ git pull` # pulls and merges

# Branches
`$ git branch testing` # creates a new branch
`$ git checkout testing` # switch HEAD to this branch
`$ git checkout -b <newbranchname>` # Creating a new branch and switching to it at the same time

# Merging
`$ git checkout master` # Checkout the branch you want another brnach to merge into
`$ git merge hotfix` # Merge "hotfix" to "master"
"Fast Forwarding" occurs when the trunk can automatically move ahead to the branch without any merge conflicts because they are in the same path of development.
`$ git branch -d hotfix` # delete "hotfix" branch now that it's merged to "master"

*From the book: It’s worth noting here that the work you did in your hotfix branch is not contained in the files in your iss53 branch (which diverged from master before hotfix was merged into master). If you need to pull it in, you can merge your master branch into your iss53 branch by running git merge master, or you can wait to integrate those changes until you decide to pull the iss53 branch back into master later.*

# Merge Conflict
`$ git status` # after a merge conflict occurs, type this to see which files are pending merger due to conflict.
```
<<<<<<HEAD:file.txt
here is conflicting line in Head's file.txt
========
here is conflicting line in branch's file.txt
>>>>>> branch:file.txt
```
After correcting the conflict, stage and commit

# Branch Management
`$ git branch `# with no arguments, you get a simple listing of your current branches. The checked out branch will have a * next to it (HEAD)
`$ git branch -v` # see the last commit on each branch
`--merged` # To see which branches are already merged into the branch you’re on
`--no-merged` # To see all the branches that contain work you haven’t yet merged in, you can run git branch

`$ git branch -d testing` # Trying to delete an unmerged branch will fail, and deleting a merged branch with no HEAD is no problem
`$ git branch --no-merged master` # what is not merged into the master branch?

# Push to remote
`$ git push origin serverfix` # push to origin branch called "serverfix"
`$ git push origin serverfix:awesomebranch `# in case the remote branch had a different name