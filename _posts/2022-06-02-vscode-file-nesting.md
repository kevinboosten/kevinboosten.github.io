---
layout: post
title: 'VSCode Explorer File Nesting'
author: kevin
categories: [tools]
tags: [vscode]
image: assets/images/posts/2022-06-02/header.png
description: 'Nest related files in your VSCode File Explorer'
featured: true
comments: true
---

*Since a couple of months I left my previous company to start working at [Kambr](https://www.kambr.com/)🛫. Where I was reunited with an old colleague. He and I were always debating about Webstorm vs VSCode; and we still are 😏. So the other day he was showing me some code when I noticed some specific file nesting behavior I hadn't seen for quite some time...and so it started...can VSCode do that?*

Today I want to show you a nice new feature of VSCode called: [Explorer File Nesting](https://code.visualstudio.com/updates/v1_67#_explorer-file-nesting).
It has been made publicly available since the release of April 2022 (version 1.67).  

![img](/assets/images/posts/2022-06-02/nesting.gif)


What it basically does, is to allow you to nest certain files that are related to each other. So in my example I'm using Vue and each `*.Vue` file has an associated `*.spec.ts` test file. This can help you to free up some space in your IDE and mental view. 

There are several settings to control this behavior:

- `explorer.fileNesting.enabled` - Controls whether file nesting is enabled at-large. It can be set either globally or for a specific workspace.
- `explorer.fileNesting.expand` - Controls whether nested files are expanded by default.
- `explorer.fileNesting.patterns` - Controls how files are nested. The default configuration provides nesting intelligence for TypeScript and JavaScript projects, but you're encouraged to modify this to fit your own project's structure. Some examples:

## File Nesting Configs + Extension
Having trouble finding the correct configuration? Definitely check out this repo that has a lot of valuable configs: 
<https://github.com/antfu/vscode-file-nesting-config>

It even comes with an [VSCode extension](https://marketplace.visualstudio.com/items?itemName=antfu.file-nesting)!


