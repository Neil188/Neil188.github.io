---
layout: post
title: Lazy Loading images with fade in
categories: css
tags: [codepen,performance]
---

Loading large images can cause a delay in the initial paint for your page, a possible solution for this is to show a placeholder image instead then swap in the original image after loading.

<!--more-->

Creating a low-res version of your image will reduce the file size, giving you a much quicker loading time but the resulting image will be of much lower quality when displayed at the original size.  You can apply CSS filters to blur this image, then once the original image has loaded use a CSS transition to gradually remove the blur and bring the full-res image into focus, without the user being aware a low-res image was initially loaded.

## Preload

In order to lazy load the image a `rel="preload"` link can be added to the page - see [Preloading content with rel="preload" on MDN web docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Preloading_content){{ site.new_tab }} this allows the resource to be downloaded as soon as possible without blocking the page render.

Once the resource has loaded a JavaScript function can be triggered using the onload attribute, which we can use to swap the full image for the low-res version.

Example preload link

```html
<link rel="preload" href="large-image.png" as="image" onload="swapImage">
```

## CSS Blur

The blur [filter](https://developer.mozilla.org/en-US/docs/Web/CSS/filter){{ site.new_tab }} will let us blur the original image when first displayed, and we can use a transition on the filter in order to gradually remove the blur when the full image is loaded:

```css
.before {
   filter: blur(10px);
   transition: filter 3s ease-in;
}

.after {
   filter: blur(0px);
}
```

## Example

For this example, I've used one of my pictures from [Flikr](https://flic.kr/p/27B53NN){{ site.new_tab }}, note the image has not been optimised in any way, normally you would make sure you are serving an image size as close to the final size as possible instead of just taking the image and resizing it in the browser.  The original image is over 1Mb, the reduced image is approx 1.2Kb, inlined as a data URI:

![image text]({{ "/images/posts/Lazy-Load-Waterfall-small.jpg" | absolute_url }} "Reduced size image")

Codepen demo of lazy load:

<p data-height="265" data-theme-id="0" data-slug-hash="RJPoPz" data-default-tab="css,result" data-user="Neil188" data-embed-version="2" data-pen-title="Lazy load fade in image" data-preview="true" class="codepen">See the Pen <a href="https://codepen.io/Neil188/pen/RJPoPz/">Lazy load fade in image</a> by Neil Lupton (<a href="https://codepen.io/Neil188">@Neil188</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
