---
layout: post
title: "A workflow for scaling retina assets"
date: 2014-10-07 11:15
published: true
---

Yesterday I published a small Automator workflow for OS X that I use to generate @2x and @1x scaled versions of my @3x assets. Apparently it generated a bit of fuzz [on twitter](https://twitter.com/ksmandersen/status/519144015837814784), so I thought it might be worth posting here too.

I have been using this workflow for ages for downscaling @2x assets to @1x. It only made sense to update the workflow for @3x assets and share it with other lazy developers who hate manual work as much as me.

<center>
![Workflow example]({{ 'unretina.gif' | asset_path }})
</center>

You can get the workflow right here:

<center>
[![Unretina Workflow]({{ 'unretina_workflow_screenshot.png' | asset_path }})](http://kristian.co/downloads/unretina.workflow.zip)
</center>

## Sidenote

As pointed out by several people on Twitter it is not the ideal solution to downscale image assets for iOS apps. The ideal solution is to use vector images in PDF files that Xcode can automatically upscale without artifacts. It is however often not possible to generate vector graphics for all parts of an app which is why I use this script for those assets.