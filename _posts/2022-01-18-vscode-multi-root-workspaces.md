---
layout: post
title: 'VSCode Multi-root Workspaces'
author: kevin
categories: [tools]
tags: []
image: assets/images/posts/2022-01-18/2022-01-18.png
description: 'VSCode Multi-root Workspaces to increase focus and speed up development'
featured: true
comments: true
---

I'm a big fan of VSCode nowadays. And one of the great features is [VSCode Workspaces](https://code.visualstudio.com/docs/editor/workspaces).

> A Visual Studio Code "workspace" is the collection of one or more folders that are opened in a VS Code window (instance). In most cases, you will have a single folder opened as the workspace but, depending on your development workflow, you can include more than one folder, using an advanced configuration called Multi-root workspaces.

And the Multi-root workspaces is what can really help you to focus on the directories that matter for your context. Imagine the following directory structure if you are a front-end developer:

![img](/assets/images/posts/2022-01-18/1.png)

The code that you are probably interested in is located in the directories `front-end-a` and `front-end-b`. To focus on this, you can leverage Multi-root Workspaces with a config like this:

![img](/assets/images/posts/2022-01-18/workspace-setting.png)

Opening your project with this Workspace file will eventually look like this:

![img](/assets/images/posts/2022-01-18/front-end-focused-workspace.png)

That can only increase your productivity if you ask me! 🚀

Oh! And before I forget, the Workspace file brings a lot of more productivity boosters like context aware Terminals, run Tasks in a context and more. Definitely check out the docs for all the available features.
