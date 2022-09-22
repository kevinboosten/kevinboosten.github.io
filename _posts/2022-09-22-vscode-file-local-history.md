---
layout: post
title: "VS Code Local History"
author: kevin
categories: [tools]
tags: [vscode]
image: assets/images/posts/2022-09-22/header.png
description: "Keep track of your local changes in your files with VS Code Local History."
featured: true
comments: true
---

We all have been in the situation where you did not yet committed your files to git and suddenly you lost everything due to some destructive action you did! You start searching in your git stashes or try to recover your work with ctrl/cmd+z but without success. And then you start to panic 😱.

So how can we get out of this hopeless situation? Do we really need to switch back to an IDE that you have used in the past that has this out-of-the-box 😏? Or can VS Code do this?

## VS Code Local History Extension

What I was planning to write in this tip was to point you to the VS Code extension [Local History](https://marketplace.visualstudio.com/items?itemName=xyz.local-history) that will help you to recover your work because it keeps track of your changes.
I've used this extension a couple of months and it supported me from time to time.

![img](/assets/images/posts/2022-09-22/local-history-plugin-tree.png)

## VS Code built-in Local History

But then I found out that this extension is not needed anymore because VS Code has this feature built-in! 🎉.
Apparently this feature was added in VS Code 1.66.0 as you can see in the [release notes](https://code.visualstudio.com/updates/v1_66#_local-history).
I completely missed this because I actually thought I was still using the Local History extension, while it actually was the new built-in Timeline feature 😅.

![img](/assets/images/posts/2022-09-22/local-history-vscode.gif)

So open your command palette (ctrl/cmd+shift+p) and type in "Timeline" and you will see the option "View: Focus on Timeline View". This will open the Timeline in your editor with all the changes that were made to the file.

VS Code can do it! 🚀
