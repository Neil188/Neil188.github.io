---
layout: post
title: CSS Levels
categories: css
tags: css
---

In 2011 the W3C CSS Working Group published CSS 2.1 as a W3C Recommendation, but when they moved onto CSS 3 a decision was made to divide the spec into smaller modules, instead of maintaining one single specification.

<!--more-->

Due to the ever-growing number of features in CSS, working on a single specification to cover all features has become impractical.  Waiting for all features to be ready in order to declare CSS 3 finished would have resulted in a massive delay, and possibly never reach release candidate status.

## Modules

Beyond level 2 they decided to adopt a modular approach, splitting all features into level 3 modules which were based on the CSS 2.1 specification, with amendments and extra functionality.  Now each module has its own level, and new releases are independent of each other.
As these modules build on the CSS 2.1 specification, any new features that weren't of the CSS 2.1 spec, such as Flexbox, start as Level 1 specifications.

## CSS Level 3,4 and onwards

As each feature has been split into modules we no longer have a simple CSS level 3 including all the latest features, instead, we have the CSS2.1 with all the latest mature modules, including new features, layered on top.  This is what is considered as the CSS 3 release, however when features such as Flexbox at level 1 are added this makes the CSS 3 description rather confusing, and inaccurate.  We won't ever get a CSS 4 release, though we will get level 4 of single modules as work continues on them.

## CSS Snapshots and current status

To get a view of the current state of CSS features, the CSS Working Group produces a Snapshot which defines the current scope and state of stable CSS.  The last snapshot was produced in  
[2017](https://www.w3.org/TR/css-2017/  "CSS Snapshot 2017"){{ site.new_tab }}.

You can view the current levels of the CSS modules along with the level they are currently at the [CSS current work](https://www.w3.org/Style/CSS/current-work "CSS current work"){{ site.new_tab }} page.
