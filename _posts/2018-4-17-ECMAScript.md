---
layout: post
title: ECMAScript and JavaScript
categories: [javascript]
tags: [javascript]
---
JavaScript is an implementation of the language standard [ECMAScript](https://en.wikipedia.org/wiki/ECMAScript){{ site.new_tab }}.  The [Ecma TC39](https://www.ecma-international.org/memento/TC39.htm){{ site.new_tab }} (Ecma Technical Committee 39) committee is responsible for evolving the ECMAScript programming language and authoring the specification.  It is also responsible for the official ECMAScript Conformance Test Suite, that can be used to check how closely a JavaScript implementation follows the ECMAScript specification.

<!--more-->

## Skin Disease!

JavaScript's original creator, Brenan Eich, has commented that ["ECMAScript was always an unwanted trade name that sounds like a skin disease."](https://mail.mozilla.org/pipermail/es-discuss/2006-October/000133.html){{ site.new_tab }}

## Versions

The latest version is ECMAScript 8 - published June 2017.  From the 6th Edition, the official name became 'ECMAScript <year>' so instead of ES6 we got ES2015
ES.Next is a dynamic name referring to whatever the next version is at time of writing.  ES.Next features are more correctly called proposals as the specification has not been finalised yet.

## Conformance

The Test262 ECMAScript Test Suite is used to attempt to measure conformity to the latest specification.  As of April 2018 [Wikipedia](https://en.wikipedia.org/wiki/ECMAScript#Conformance){{ site.new_tab }} shows this as:

| Scripting engine | Reference application(s) | Conformance ES5 | Conformance ES6 | Conformance Newer (2016+) |
| --- | --- | --- | --- | --- |
| SpiderMonkey | Firefox | 100% | 97% | 77% |
| Chrome V8 | Google Chrome, Opera | 100% | 97% | 93% |
| JavaScriptCore (Nitro) | Safari | 97% | 99% | 64% |
| Chakra | Microsoft Edge | 100% | 96% | 60% |

## TC39 5 Stage Process

New proposals follow the The [TC39 5 stage process](https://tc39.github.io/process-document/){{ site.new_tab }}  

Stage | Name | Description
--- | --- | ---
0 | Strawman | New proposals
1 | Proposal | Making the case for the addition
2 | Draft | Formalising the syntax and semantics of the proposal - still experimental and subject to change
3 | Candidate | Proposal now spec compliant, and only critical changes allowed
4 | Finished | Ready for inclusion in the next ECMAScript release

All new changes are tracked in the [proposals repository](https://github.com/tc39/proposals){{ site.new_tab }}  

* [Stage 0 Proposals](https://github.com/tc39/proposals/blob/master/stage-0-proposals.md){{ site.new_tab }}
* [Finished proposals](https://github.com/tc39/proposals/blob/master/finished-proposals.md){{ site.new_tab }}
* [Active proposals](https://github.com/tc39/proposals){{ site.new_tab }}
* [Inactive proposals](https://github.com/tc39/proposals/blob/master/inactive-proposals.md){{ site.new_tab }}
