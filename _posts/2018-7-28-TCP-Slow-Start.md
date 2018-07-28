---
layout: post
title: The TCP Slow Start scheme
categories: html
tags: [html,http,performance]
---

In order to render content to a user as quickly as possible, you should always try to only send critical HTML and CSS at first, the minimum amount required for the first render.  You may see the ideal maximum size of this critical load quoted as 14kb, but where does this number come from?

<!--more-->

When transmitting data across a network connection, overloading the connection by sending too much data is known as congestion which will cause reduced quality of service.

## Congestion Window

When you request a website the data is downloaded in packets, the size of which is known as the congestion window.  When this packet is received an acknowledgement is sent back indicating the next packet can be sent.  In this way downloading a website is split into a number of roundtrips, the less the better.

## Slow Start

In order to optimise the speed of the connection TCP implements a [Slow Start](https://en.wikipedia.org/wiki/TCP_congestion_control#Slow_start){{ site.new_tab }} strategy - each connection begins with up to 10 packets being sent on the first roundtrip, and then each time an acknowledgement is received the number of packets being sent on each roundtrip increases, up to the point where the TCP congestion control algorithms determine the connection is no longer stable.

## HTTP Persistent connection

Starting off with the minimum congestion window for each asset downloaded from a site would result in very poor performance for most websites, so since HTTP 1.1, all connections are considered persistent unless declared otherwise.  As only one connection is opened, each download does not have to go through the same slow start strategy.

## Initial size

The 10 packet limit on the first connection will give you a limit of around 14kb of data to send.  So limiting the first content to 14kb or less will only result in the fastest possible download, without requiring any additional roundtrips.  Obviously, this would be extremely hard to implement on most sites but gives you a target to aim for.  For mobile sites [Google](https://developers.google.com/speed/docs/insights/mobile#delivering-the-sub-one-second-rendering-experience){{ site.new_tab }} recommend keeping the above the fold content under 98kb, which would be downloaded in three round trips.
