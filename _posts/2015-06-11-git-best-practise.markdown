---
layout: post
title:  "Git Best Practise"
date: 2015-06-11 11:50:13
categories: Git Best Practise
---

## Useful URLS
1. http://sethrobertson.github.io/GitBestPractices

## Command Cheat Sheet

{% highlight bash %}

// branch
git branch branch_name commit
git branch -b new_branch_name commit

git branch
git show-branch

// delete branch
git branch -d branch_name

//diff
//between working directory and index
git diff

// between index and respository
git diff --cached 

//below two commands are same
git diff master bug/pr-1
git diff master ..bug/pr-1

//searches the past 20 commits to the master branch for changes containing string
git diff -S"string" master~20

// remove
git rm aaa

git rm --cached aaa //remove add from stage area, keep it in local work dir.

// moving file
git mv aaa bbb 

{% endhighlight %}

## Change the last commit mesage, or add file to last changes

{% highlight bash %}

git commit -m 'initial commit'
git add forgotten_file
git commit --amend

{% endhighlight %}

## Working with remote

{% highlight bash %}

//showing your remote
git remote -v


// adding remoting repositories
git remote 

git remote add [shortname] [url]
git remote add pb https://github.com/paulboone/ticgit

// pushing to your remotes
git push origin master // git push [remote name] [branch]


// inspecting the remote
git remote show origin


//removing or renaming remotes
git remote rename pb paul

//remove remote
git remote rm paul

{% endhighlight %}

## Git tagging

{% highlight bash %}
git tag //show tags
git tag -l "v1.8.5*" // search tag with pattern


// lightweight tag and annotation tag, annotation tag is prefered.

git tag -a 'v1.0' -m 'version 1.0' // create v1.0 annotation tag.

git show v1.0 // show the details of tag v1.0


git tag -a 'v0.9' 18eead0 -m 'tag on history commit' // tag on history commit

//sharing tag
git push origin [tagname]
git push origin v1.0


//checkout tags
git checkout -b [branchname] [tagname]

{% endhighlight %}

## Git branch

{% highlight bash %}

git branch
git branch -v // see last commit on each branches

git branch --merged // filter which branches already mereged into currently branch on.
git branch --no-merged // filter which branches has not been merged into current branch on.


//Remote branch
git remote show (remote)

git remote add (remote) (url) // add new remote (remote)

git fetch (remote)

git push (remote) (branch)

$ git checkout --track origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'serverfix'

// To set up a local branch with a different name than the remote branch, you can easily use the first version with a different local branch name:
git checkout -b new_name (remote/branch)

//If you already have a local branch and want to set it to a remote branch you just pulled down, or want to change the upstream branch you’re tracking, you can use the -u or --set-upstream-to option to git branch to explicitly set it at any time.
git branch -u (remote/branch)

//delete remote branch
$ git push origin --delete serverfix
To https://github.com/schacon/simplegit
 - [deleted]         serverfix


{% endhighlight %}

## Git rebasing

{% highlight bash %}

$ git checkout experiment
$ git rebase master

git rebase master topic = git checkout topic, then git rebase master

// onto
git rebase --onto master server client

{% endhighlight %}

## Git config

Git comes with a tool called git config that lets you get and set configuration variables that control all aspects of how Git looks and operates. These variables can be stored in three different places:

/etc/gitconfig file: Contains values for every user on the system and all their repositories. If you pass the option --system to git config, it reads and writes from this file specifically.

~/.gitconfig or ~/.config/git/config file: Specific to your user. You can make Git read and write to this file specifically by passing the --global option.

config file in the Git directory (that is, .git/config) of whatever repository you’re currently using: Specific to that single repository.


{% highlight bash %}

//get all setting git can find
git config --list

//set global developer configuration
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com


//sample gitconfig for yubo
[user]
	name = Yubo Yue
	email = yubo@amazon.com
[core]
	excludesfile = /Users/yubo/.gitignore
	pager = less -FMRiX
[color]
	ui = auto
[push]
	default = simple
[alias]
	dag = log --graph --format='format:%C(yellow)%h%C(reset) %C(blue)\"%an\" <%ae>%C(reset) %C(magenta)%cr%C(reset)%C(auto)%d%C(reset)%n%s' --date-order
    co = checkout
    st = status
    br = branch
    ci = commit

    unstage = reset HEAD --
[amazon]
	append-cr-url = true
	pull-request-by-default = true
[diff]
    #tool = opendiff
    tool = sourcetree
[difftool "sourcetree"]
    #cmd = opendiff \"$LOCAL\" \"$REMOTE\"
	cmd = /Applications/SourceTree.app/Contents/Resources/opendiff-w.sh \"$LOCAL\" \"$REMOTE\"

[merge]
    tool = sourcetree
[mergetool "sourcetree"]
	cmd = /Applications/SourceTree.app/Contents/Resources/opendiff-w.sh \"$LOCAL\" \"$REMOTE\" -ancestor \"$BASE\" -merge \"$MERGED\"
	trustExitCode = true


{% endhighlight %}

## Git ignore

{% highlight bash %}

# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
doc/**/*.pdf

{% endhighlight %}

