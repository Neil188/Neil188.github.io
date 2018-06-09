---
layout: post
title: HTML5 Drag and Drop API
categories: javascript
tags: [codepen,html]
---

HTML5 introduced the Drag and Drop API which allows users to manipulate pages using drag and drop actions.

<!--more-->

## Identifying what is draggable in HTML

To make an element draggable it must have the `draggable='true'` attribute added:

```html
   <div class="dragme" draggable='true'></div>
```

Note this does not work as a boolean attribute - using the shortcut `<div class="dragme" draggable>` would not work.  When not set the default value of `auto` will use the browser's defaults, which will allow text selections, images and links to be dragged.

## The `ondragstart` event handler

As well as the draggable attribute you must also set up an event handler for the `dragstart` event, using either the `ondragstart` attribute or adding an event listener for the `drag` event:

```html
   <div class="dragme1" draggable="true" ondragstart="console.log('Starting drag...')"></div>
   <div class="dragme2" draggable="true"></div>
   <script>
      function onDragStart() {
         console.log('Starting drag...');
      }
      const target = document.querySelector('.dragme2');
      target.addEventListener('dragstart', onDragStart);
   </script>
```

The event handler can set the `dataTransfer` property that can set the data type and contents of the dragged data, for instance:

```javascript
   e.dataTransfer.setData("text/plain", "This is the data");
```

to store a string in the drag data.

## The drag image

The `dataTransfer` object also allows you to set an image to use instead of the default translucent image generated from the drag target:

```javascript
   e.dataTransfer.setDragImage(img, 10, 10);
```

## The drag effect

When dragging an object you can trigger different types of operations on drop.  The `dataTransfer.effectAllowed` attribute determines this and can be set to values such as `copy` to indicate the object will be copied, `move` to indicate the object will be moved to the target or `link` to indicate a connection will be made between the target and source.

## The `dragenter` and `dragover` events

Up to this point, we have only been concerned with allowing an element to be dragged, however, the default handling for most targets on a page is to not allow a drop, giving you no targets to drag your source to.  In order to allow a drop, you must prevent this default handling of drag events, either by returning false from either the `ondragover` or `ondragenter`

```html
   <div class="target1" ondragover="return false"></div>
   <div class="target2"></div>
   <script>
      function onDragEnter(e) {
         e.preventDefault;
      }
      const target = document.querySelector('.target2');
      target.addEventListener('dragenter', onDragEnter);
   </script>
```

## The Drop event

When the user releases the mouse the drag operation ends, and if dropped on a valid drop target (where the `dragenter` or `dragover` events cancel the default behaviour) the `drop` event will fire at the drop target.

Note if the drop is not over a valid target the `drop` event will not be fired and the drag operation is cancelled.

The drop event holds the data that was set by `dataTransfer.setData`, and the `getData` method can now be used to retrieve that data, with a key of the data type to retrieve:

```javascript
   function handleDrop(e) {
     const data = e.dataTransfer.getData("text/plain");
     e.target.textContent = data;
     e.preventDefault();
}
```

## The `dragend` event

Finally, the `dragend` event is fired at the source of the drag operation.  This will be triggered whether the drag was a success or if it was cancelled.  If cancelled, the `e.dataTransfer.dropEffect` property will be set to `none`.  The `dragend` event handling can be used to perform any cleanup required on the source element.

## Examples

### Moving an image between boxes

The following CodePen is a simple demonstration of dragging an image between different boxes:

<p data-height="265" data-theme-id="0" data-slug-hash="oyBvEL" data-default-tab="js,result" data-user="Neil188" data-embed-version="2" data-pen-title="Drag and Drop" data-preview="true" class="codepen">See the Pen <a href="https://codepen.io/Neil188/pen/oyBvEL/">Drag and Drop</a> by Neil Lupton (<a href="https://codepen.io/Neil188">@Neil188</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### Using dataTransfer.getData to transfer data

The following CodePen is a mock character set up for a game, which allows you to choose a character class by dragging your choice up the main character panel.

<p data-height="265" data-theme-id="0" data-slug-hash="rKjybr" data-default-tab="css,result" data-user="Neil188" data-embed-version="2" data-pen-title="Drag And Drop Characters" data-preview="true" class="codepen">See the Pen <a href="https://codepen.io/Neil188/pen/rKjybr/">Drag And Drop Characters</a> by Neil Lupton (<a href="https://codepen.io/Neil188">@Neil188</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
