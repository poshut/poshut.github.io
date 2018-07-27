---
layout: post
title: 'The Beauty of Git'
categories: software-engineering
---

At first, there was nothing. And then Linus Torvalds created Linux. And Linus saw it was good. The second day, he created Git.

When thinking about modern Software Engineering, it is impossible not to think of **Git**, and with that, I don't mean the idiot.

For anyone who does not know git already: Git is a distributed **Version Control System** (**VCS**). It allows you to track changes
you make to your code (or any other files for that matter), roll them back and view your project at each state it previously had.
Git also allows you to create something called **branches** Lightweight deviations from the main codebase to try something out or
implement a new feature without disrupting the work of those working in the main codebase.

The **distributed** part of Git means that every developer has a full copy of the repository. This makes it so that inividual developers
do not need to have an active internet connection at all times; instead they can interact with their local copy and **push** their changes
as soon as they come online again to enable others to see them.

This is where I find the beauty of Git: Git enables teams of any size to work together, with maximum efficiency.

This kind of workflow is supported by one very important feature Git has: **merging**. As I said previously, with Git, you can create 
branches which are deviations from the main codebase, or any other branch. If the thing you try is successful, or you finished implementing
a feature, the branches have to be **merged** together, because while you were changing your feature or test branch, another person might have changed
the branch you based yours of, creating a fork in the history. **Merging** just describes the process of putting together both sets of changes, without
losing anything. Most of the time, if there aren't conflicting changes, the ```git merge``` will merge automatically, without needing anything from you.
Yet, if there are conflicting changes, ```git merge``` will stop and prompt you to resolve **merge conflicts**. Many IDEs have support for finding these,
and resolving them is pretty easy most of the time, if you know your changes and the codebase.

Effortless committing, creating branches and merging makes it very easy to do those things a lot: Branches are lightweight, and you can commit relatively often,
so that you can test functionality and rollback to every individual state.

Now, I will show you some of my favorite Git commands not everybody knows about.

When your **local** history and the remote's history diverge, don't just ```git pull```. This creates a merge commit which pollutes the commit history
and confuses everybody using graphical interfaces such as **GitHub**. Instead of merging the local and remote history, consider doing a rebase onto the remote
with ```git pull --rebase``` instead. This keeps the commit history clear for every who looks at it.

Another of my favorite Git commands is improved logging output. Instead of running ```git log```, which often gives me way too many details, you can run
```git log --oneline --graph``` (you might even considering aliasing it). This will show every commit as one line, and the entire history as a graph, 
clearly marking which branch the commit was on.

A last thing is actually a configuration option. To make merging even easier, you can enable Git's three-way-merge output with ```xxx```. This does not only show 
both versions of the lines, it also shows the **merge base**, therefore better showing the difference between the changes in both branches.

Git has truly changed the way I develop my own software, and the way teams work and program together.