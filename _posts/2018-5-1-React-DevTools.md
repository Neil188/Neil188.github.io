---
layout: post
title: React Developer Tools
categories: [devtools, react]
tags: [chrome,devtools,react]
---

The React Developer Tools extension is a very useful tool available for Chrome and Firefox that helps you work with React components.

<!--more-->

You can install the Chrome extension from the [Chrome webstore](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi){{ site.new_tab }}.  Once installed the React Developer Tools icon will show as active for any website that has React components, and when you open DevTools a new React panel will be available.  Within this panel you can inspect the React component hierarchy, viewing the state and props for any component.

The React tab is easy to navigate, showing the tree view to the left, and the state and props to the right:

![react developer tools panel]({{ "/images/posts/React Developer Tools panel.png" | absolute_url }})

## React Tree View

You can use the tree view to navigate to any component, any element the Tools recognise as a React component instead of a standard element will be shown in a different colour.  In addition, any stateful components will also have a different coloured collapser, for instance:

![react developer tools component]({{ "/images/posts/React Developer Tools collapser.png" | absolute_url }})

In this both App and Menu have been recognised as React components and div as a standard component.  The collapser for the App component indicates that this a stateful component, whereas the other two elements do not have their own state.

Right-clicking on any element in the tree view will give you options such as scroll to node (on current page), copying the element name or props, or Find the DOM node which will take you to the Elements panel with the element selected.

![react developer tools right-click]({{ "/images/posts/React Developer Tools right click.png" | absolute_url }})

## Selecting an element

In addition to finding an element via the tree view, you can use the search box to only list elements that match the value you enter (when you right-click and select 'Show All <element>' it populates the search box with the current element name, and so only shows elements with the same name).

Also if you select an element on the Elements panel, then switch to the React panel the element will be automatically selected in the tree view (though I've found this doesn't always work, sometimes you need to switch back to the Elements panel and re-select the element).

The target icon allows you to select an element on the current page, this component will then be automatically selected in the tree view, in the same way as the standard DevTools 'select an element in the page' tool works.

## Last selected React component

Similar to how DevTools stores the last selected element in a $0 variable, the last selected React component will be stored in a $r variable

![react developer tools last selected]({{ "/images/posts/React Developer Tools last selected.png" | absolute_url }})

## Props and state

On the right-hand props and state panel, you can view the current values, and also change them to see the live impact on your app.  For all values, you can also right click and select 'Store as global variable' to copy the current object to a $tmp variable.

![react developer tools props and state]({{ "/images/posts/React Developer Tools props and state.png" | absolute_url }})

Note you need to right-click on the object value not the name for this menu to appear.

## ReactDevTools git repo

For more info you can view the [Facebook react-devtools Github repo](https://github.com/facebook/react-devtools){{ site.new_tab }}.
