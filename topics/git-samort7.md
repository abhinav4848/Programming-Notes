I would say that 95% of developers who use git, mainly use the CLI. Its a hurdle you need to just work hard enough until you get over it. And do lots of Googling. I've been using Git professionally for a while now and I _still_ have to google basic commands. My work monitor is covered in little stickies for Git things that I use a lot.

# Samort7's Super Simple Git Tutorial
##Installation
Download and install git from https://git-scm.com/ and make sure you select the option for adding git to your PATH (so your computer knows the location of git and you can use the `git` command in your console).

##Initial Setup
When I learned Git, I started extremely simple. I created a folder called `myTest` on my desktop and put one file in it called `test.txt`. It looked like this:

    AAAA
    BBBB
    CCCC

I then did the following steps (this is Linux, so adjust for Windows if needed):

    $ cd ~/Desktop/myTest
    $ git init

This marks the folder as a git "repository." A repository (or "repo") is just a folder that is tracked by Git and responds to Git commands. You'll know it's a repo because there will now be a `.git` sub-folder in there alongside your `test.txt` file.

##Adding to Staging

    $ git add ./test.txt

This adds my file to staging. Staging is just a preparation area where you prepare all your files before you make an official "commit." Think of a commit as permanently recording in your repo's history what changes you have made to your files. When you commit, whatever changes were staged will now be recorded in the history of your repository forever (there are ways to remove commits but its difficult and generally discouraged.)

##Making a Commit
    $ git commit -m "Created a file. It's my first commit!"

This commits the file with a message explaining the commit. 

At this point you now have a _local_ repository. Don't even worry about Github or pushing to Github at this point. Keeping things local makes life a lot easier when learning the basics. That means you don't have to worry about `git push`, `git fetch`, or `git pull`.

##Changing Stuff & A Second Commit
Cool, so next I made a change to `test.txt`:

    AAAX
    BBBB
    CCCC

And then I committed those changes:

    $ git status

This shows me the current status of my repo. I can see very clearly that something has changed.

    $ git add ./test.txt
    $ git status

Ok, so now I can see I have a file added to the staging area, but it's not committed yet. Let's change that!

    $ git commit -m "Changed first line!"
    $ git status

Now I can see that there are no changes left! Nothing to commit! Cool!

##Changing Stuff Again but Fancier!
Now change the second line, _but don't commit anything yet._

    AAAX
    BBBX
    CCCC

If you want to see the differences between your current changed version and the previous version, you may have heard about `git diff`. I hate `git diff`. I find it incredibly hard to read. But guess what? There's _an even better tool_ built into Git! Run this command:

    $ git difftool

A cool diff-viewing tool should launch showing you changes in a _much_ easier to read way! How cool is that?

Ok, anyway, when you're done checking that tool out, you can close it and make your commit.

    $ git commit -m "Change second line"

##Time-travelling with Git
If you've followed along, you should now have a repo with three different versions in it. Here's where I show you some _really cool fucking shit_.

Run this command. Don't worry what it means. The result is what is important:

    $ git log --oneline --decorate --graph --all

If you've done everything correct, you should see a neat graph explaining [the history of your local repository](https://xkcd.com/1296/). Pretty sweet, huh? Now, lets say that you fucked up big and you need to go back to an older version of your file, say, the first initial version you made. That's easy to do.

You should see on the graph some weird number like this `d8329f` (it won't match exactly, but that's fine). If you want to go the version of your file at that point, all you need to do is:

    $ git checkout d8329f

This will reset _your entire folder_ to the state it was at when you made that commit. If you open `test.txt` and look at it now, you should see that the first line is back to `AAAA` again.

If you do that `git log --oneline --decorate --graph --all` command again, you should see that your _HEAD_ has moved down to the first version of your file. That tells you something: HEAD is what version of the code you are currently looking at.

If you run `git status` again, you should also see the word `master`. That's the name of the "branch" you are on. It's the default name of a git repository there are ways to change it if you want. Better to just leave it as master though because its a convention that everyone uses. 

##Back to the Future
Anyway, if you want to go back to the latest version of your current branch, all you need to do is:

    $ git checkout master

Run `git log --oneline --decorate --graph --all` again, and you should see your HEAD is back at the top again. Open the `test.txt` file and you should see all your changes are back too!

##Branches and Merges
What happens if I check out an old commit and decide to make some new commits on top of it? Well, that is when we need to create a _branch_.

You do this with:

    $ git checkout -b myNewBranchName

You can then make changes and make commits to your heart's content.

You can see all of your current _local_ branches with `git branch`. A `*` indicates the branch you are currently on.

If, after making some commits, you do `git log --oneline --decorate --graph --all` again, you will be able to see your branch extending off of your main `master` branch. In order to get your branch and your `master` branch to connect again, you have to do a _merge_.

During a merge, Git compares the files in two different branches to see if they both change the same code or not. If they don't, then **poof!** You are back to a single `master` branch. However, if both branches have, let's say, changed the same line in the same file to two different things, you have what is called a _conflict_.

Git will actually modify the file to include both versions with some weird text that looks like this: `<<<<<<<<<<`. You then have to  _resolve_ the conflict, by editing the file to contain only the code that you want to keep.

An example of what a conflicted file looks like, and an explanation of how to read it can be found [here](https://stackoverflow.com/questions/24103490/need-help-to-understand-merge-conflict-example).

##A Note on Remotes and the word Origin
One thing that tripped me up forever learning Git. When you see the word "remote", just remember that it means an alias (aka a nickname) for a branch.

i.e. if I run this command inside my `myTest` folder:

    $ git remote add origin git@github.com:myGithubName/myRepoName.git

All I am doing is saying: "Hey Git on my local machine! Whenever I use the word `origin`, I am really talking about this repository on my Github account. Don't forget that! Thanks!"

So when you do something later like `git push origin master`, what you are _actually doing_ is saying "Hey Git! I want to save all of the commits of my current working branch to the `master` branch of 'origin'. You _do_ remember what 'origin' was, don't you?"

(This is different from `git push origin` which will save all of the commits on ALL of your local branches to the matching branches on `origin`.)

And Git says "Oh right! I remember! You told me that 'origin' was such-and-such repository on your Github account! I will save your work (and your work history) there!"

And you say "Right! Thanks Git! Let's have grab a beer later!"

The thing to remember is you don't have to call your remote "origin". It's just a nickname. It could be "MyDevGitHubRepo" for all I care. What this allows you to do is to have more than one repo you can push to. For example, you could have one remote at github and another remote at bitbucket. So you could do:

    $ git push MyDevGitHubRepo master
    $ git push MyProdBitBucketRepo master

## Dealing with Repos on a Server
Let's say that you have two computers you work at. Computer A and Computer B. Both are editing the same codebase stored on Github (or Gitlab, or even a personal server). If computer A makes a change and pushes it to the server, I believe your question is "How does computer B know about the change that now exists on the server?" The answer is that computer B needs to do a `git fetch` to get the latest changes. After doing a `git fetch` you will see how far "behind" your code is from the github repo you are linked to.

So you find out the code has changed and your code is behind. What do you do in this situation? Well, before you are allowed to `git push` your code to the server, you need to get the latest code changes and merge them into your local branch. You do this by first getting the latest changes (`git fetch` as explained previously) and then by doing a `git merge` and finally resolving conflicts as usual. Only then can you do a `git push` of your changes.

Because we do `git fetch` and then `git merge` so often, there is a convenience method that does both for us: `git pull`.

##Further Reading

Ok, that's enough for basics now. Try learning the difference between a `merge` and a `rebase`, cloning a remote repo, and how to link a local repo to a github repo. It's also really important to learn about the `.gitignore` file and the `.gitconfig` file.

To do this, just Google and read the Stack Overflow results. Honestly, I find them a lot easier to understand than the official Git manual pages.

Hope this helps! Good luck! Keep at it, and don't fight the command line! It's your friend :)

Also, be sure to read my [ADVANCED TIPS & TRICKS](https://www.reddit.com/r/learnprogramming/comments/al0ebi/anyone_got_an_eli5_version_for_basic_git/efd9ddj/) comment below!