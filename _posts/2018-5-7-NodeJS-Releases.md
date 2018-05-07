---
layout: post
title: NodeJS Release Management
categories: nodejs
tags: nodejs
---

Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine and is supported by the Node.js Foundation.  It has a strictly controlled release schedule, which aims to have a stable, safe version available at all times.

<!--more-->

## The Node.js Foundation TSC

The Node.js Foundation [Technical Steering Committee](https://github.com/nodejs/TSC){{ site.new_tab }} is the technical governing body of the Node.js Foundation.  One of TSC's Core Working Groups is the Release Working Group, who manage the release process for NodeJS

## [Releases](https://github.com/nodejs/Release){{ site.new_tab }}

Multiple types of releases are available for [download](https://nodejs.org/en/download/){{ site.new_tab }} :

### Current

The latest major release version, released every 6 months in April (even-numbered versions) and October(odd-numbered versions).  This has the latest features and is usually not used for production systems.  At the time of publishing the Current release is version 10.0.0, released April 2018.  In October 2018 version 11 will be released, and version 10 will move to LTS.

### LTS - Long Term Support

Each time an odd version of NodeJS is release, the previous even version is moved to LTS (so LTS will only have even-numbered releases), after which that version has 18 months of active support, followed by an additional 12 months of maintenance support, where only critical bugs, critical security fixes and documentation updates are added.  Version 8 is currently the active LTS version, with version 6 in maintenance support (which is scheduled to end April 2019).  The active LTS version has a focus on stability and security, and as such is most suitable for production systems.
All LTS are given codenames, which you may occasionally see used when describing a release: version 6 was Boron, 8 is Carbon, 10 is Dubnium, and version 12 will be Erbium.

### [Nightly](https://nodejs.org/download/nightly/){{ site.new_tab }}

The very latest versions of the Current branch, automatically built and released every 24-hours, as with any nightly release be very cautious about using these.

## Source

The runtime source is stored on [Github](https://github.com/nodejs/node){{ site.new_tab }}, this includes a guide to [building Node.js from source](https://github.com/nodejs/node/blob/master/BUILDING.md){{ site.new_tab }}, though this isn't going to be something the majority of users will ever need to do!
