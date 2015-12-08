---
layout: post
title:  "Git Best Practise"
date: 2015-06-11 11:50:13
categories: Git Best Practise
---


## Common Commands

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

{% endhighlight %}

