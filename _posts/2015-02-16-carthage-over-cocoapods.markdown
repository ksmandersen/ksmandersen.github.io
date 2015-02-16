---
layout: post
title: "Carthage Over CocoaPods"
date: 2015-02-16 14:16
published: false
category: development
tags: development, ios
---

Carthage
Pro:
- It builds frameworks
- There is no central repository
- You only compile the code once and not everytime you compile your own project
- You manage the framework yourself.
- It doesn't touch your Xcode project
- You don't need a workspace
- Library  maintainers dont need to maintain a Podfile
- If a library already supports frameworks nothing needs to be done

Con:
- All collaborators need to have carthage installed to compile the project due to the copy-frameworks thing
- There is no discovery features like with CocoaPods
- Crashes in libraries are not symbolicated?

CocoaPods
Pro:
- All the infrastructure is inplace
- There is a huge repo of supported libraries in place. It is already defacto standard
- You can do everything from the command line. No dragging frameworks in and adding build phases

Con:
- Sometimes it screws up in a big way
- It is always threaned of things changing in Xcode
