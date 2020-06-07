---
layout: post
title: "Setup Latex in Docker with VSCode"
author: "Xiaozhe Yao"
mathjax: true
catalog: true
tags:
    - others
---

In this article, I will introduce how I set up my environment for writing $\LaTeX$ in VSCode. Usually I do not share how to configure the environment, but I love the settings and really want to share it with you.

## Prerequisite

You will need to have ```Docker``` and ```VS Code``` installed on your system.

## Install LaTex in Docker

Run the command in your terminal: ```docker pull tianon/latex```. The dockerfile can be found [here](https://github.com/tianon/dockerfiles/blob/master/latex/Dockerfile).

## Install VS Code Extensions

There are two extensions that I am currently using: the [LaTex Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop) and [Grammarly](https://github.com/znck/grammarly). The LaTex Workshop can be installed directly in VS Code marketplace.

The grammarly extension is a bit different as it has been unpublished in the marketplace. Fortunately, we can still download the ```.vsix``` file from [Releases](https://github.com/znck/grammarly/releases). Then in VS Code, we can manually install the extension from the downloaded file. [Here's the instructions on how to do it](https://code.visualstudio.com/docs/editor/extension-gallery#:~:text=Install%20from%20a%20VSIX&text=Using%20the%20Install%20from%20VSIX,vsix%20file).

## Enable the LaTex in Docker

In the VS Code preferences, find extensions, and the LaTex workshop. Then enable the Docker settings in this extension.



