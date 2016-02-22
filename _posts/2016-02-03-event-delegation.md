---
layout: post
title: Event Delegation
---

Event delegation is an important methodology in JavaScript. Have you ever found yourself reapplying event listeners and handlers to an element because it was dynamically created? Well, event delegation fixes that problem
by the use of event bubbling. It allows you to add an event listener to a parent element which will fire off for all its descendants.

### How it works

Let's say we have an UL element with several LI elements. The basic HTML structure looking like this:

```
<ul id='list'>
  <li>apples</li>
  <li>bannas</li>
  <li>kiwis</li>
  <li>oranges</li>
</ul>
```

If we wanted to pop up an alert box each time a LI element is clicked, we could attach a click event listener to each individual one. The problem with this approach, however, is that if we dynamically add new LI elements, those new elements would not receive the click event listener. However, if we add the click event listener to the UL element instead:

```
<ul id='list' onclick=alert(event.type + '!')>
  <li>apples</li>
  <li>bannas</li>
  <li>kiwis</li>
  <li>oranges</li>
</ul>
```

Each time a LI element is clicked, an alert box will show. This is possible due to event bubbling/propagation, which is the firing off of an event that bubbles up the DOM tree, triggering parent elements that are listening to the same event. This means that anytime you click on the LI element, you are actually clicking the entire HTML document.

Example:

```
<body onclick='alert("hello, world")'>
  <ul id='list' onclick=alert(event.type + '!')>
    <li>apples</li>
    <li>bannas</li>
    <li>kiwis</li>
    <li>oranges</li>
  </ul>
</body>
```

If we click any one of the LI elements, an alert box will pop up with the message 'click!', followed by another alert box with the message 'hello, world'.

I hope this helped you visualize and understand what event delegation is.
