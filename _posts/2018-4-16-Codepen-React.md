---
layout: post
title: React in Codepen
categories: [react]
tags: [codepen]
---

Creating a React pen in [Codepen](https://codepen.io){{ site.new_tab }} is very easy, and will give you an easy playground to help you learn React.

<!--more-->

First, create a new pen using Create > New Pen

Once on the new pen page go to Settings, then select the JavaScript panel.  From here you need to set the preprocessor to Babel (if you are going to use JSX) and add the React libraries, which should be available in the Quick-add list.

This will pull the libraries in from the [cdnjs CDN](https://cdnjs.com){{ site.new_tab }} and include them in your pen, before the JavaScript in the editor.

![settings]({{ "/images/posts/codepen - react settings.png" | absolute_url }})  

Once saved, you can check if Babel has been enabled at the top of the JavaScript panel:

![Babel enabled]({{ "/images/posts/codepen - babel enabled.png" | absolute_url }})  
