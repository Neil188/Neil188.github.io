---
layout: post
title: Installing Jekyll on Windows 10
categories: jekyll
tags: [jekyll,ruby]
---
Jekyll on GitHub Pages does not require any local installation to use, you can just update the config files or blog posts in your git repository, and when you push to GitHub, it will do the Jekyll build for you.  However, if you want to customise your Jekyll set-up, it will be a lot easier to do locally, and this will be a lot less risky for your live site!

Jekyll isn't officially supported on Windows, but if you are running Windows 10 the [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/about){{ site.new_tab }} set up, then you can use this to install and run Jekyll.

To do this follow the [guide on the Jekyll site](https://jekyllrb.com/docs/windows/){{ site.new_tab }}.  Once installed use command
```bash
jekyll -v
```
To check if Jekyll is correctly installed, then you can serve your site locally using
```bash
jekyll serve
```
Which should start your site on localhost port 4000 - you can now start breaking your site as much as you want!
