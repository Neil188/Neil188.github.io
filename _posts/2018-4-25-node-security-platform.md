---
layout: post
title: node security platform
categories: [tools]
tags: [npm, security]
---

The node security platform - [nsp](https://github.com/nodesecurity/nsp){{ site.new_tab }} is a tool to help you identify known vulnerabilities in your projects. 

<!--more-->

Every time you install a new dependency for your project, including all the dependencies that it requires, the number of potential security problems you could be exposed to increases.  Keeping track of all these dependencies and any new security risks that arise can be a big problem for even small projects, and this is where nsp can help you - it will check all current dependencies against known vulnerabilities giving you a report listing these potential problems, and in which version the problem has been fixed.

And recently [npm has acquired nsp](https://medium.com/npm-inc/npm-acquires-lift-security-258e257ef639){{ site.new_tab }}, this is excellent news as it is a big push for increased security and trust in JavaScript code.

## Running nsp

Running nsp on your project is easy, and with npx doesn't require you to install anything, just run:

```bash
npx nsp check
```

This will give you a report such as:

```bash
npx: installed 115 in 15.29s
(+) 2 vulnerabilities found
┌────────────┬────────────────────────────────────────────────────────────────────┐
│            │ Prototype pollution attack                                         │
├────────────┼────────────────────────────────────────────────────────────────────┤
│ Name       │ hoek                                                               │
├────────────┼────────────────────────────────────────────────────────────────────┤
│ CVSS       │ 4 (Medium)                                                         │
├────────────┼────────────────────────────────────────────────────────────────────┤
│ Installed  │ 2.16.3                                                             │
├────────────┼────────────────────────────────────────────────────────────────────┤
│ Vulnerable │ <= 4.2.0 || >= 5.0.0 < 5.0.3                                       │
├────────────┼────────────────────────────────────────────────────────────────────┤
│ Patched    │ > 4.2.0 < 5.0.0 || >= 5.0.3                                        │
├────────────┼────────────────────────────────────────────────────────────────────┤
│ Path       │ expensify@0.1.0 > node-sass@4.7.2 > request@2.79.0 > hawk@3.1.3 >  │
│            │ hoek@2.16.3                                                        │
├────────────┼────────────────────────────────────────────────────────────────────┤
│ More Info  │ https://nodesecurity.io/advisories/566                             │
└────────────┴────────────────────────────────────────────────────────────────────┘

┌────────────┬────────────────────────────────────────────────────────────────────┐
│            │ Prototype pollution attack                                         │
├────────────┼────────────────────────────────────────────────────────────────────┤
│ Name       │ hoek                                                               │
├────────────┼────────────────────────────────────────────────────────────────────┤
│ CVSS       │ 4 (Medium)                                                         │
├────────────┼────────────────────────────────────────────────────────────────────┤
│ Installed  │ 2.16.3                                                             │
├────────────┼────────────────────────────────────────────────────────────────────┤
│ Vulnerable │ <= 4.2.0 || >= 5.0.0 < 5.0.3                                       │
├────────────┼────────────────────────────────────────────────────────────────────┤
│ Patched    │ > 4.2.0 < 5.0.0 || >= 5.0.3                                        │
├────────────┼────────────────────────────────────────────────────────────────────┤
│ Path       │ expensify@0.1.0 > webpack@3.11.0 > watchpack@1.5.0 >               │
│            │ chokidar@2.0.2 > fsevents@1.1.3 > node-pre-gyp@0.6.39 > hawk@3.1.3 │
│            │ > hoek@2.16.3                                                      │
├────────────┼────────────────────────────────────────────────────────────────────┤
│ More Info  │ https://nodesecurity.io/advisories/566                             │
└────────────┴────────────────────────────────────────────────────────────────────┘
```

Consider adding this tool to both your development and maintenance process.
