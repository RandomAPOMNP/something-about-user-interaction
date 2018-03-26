when you want to drag an element on your page, the common idea will be adding a listener on the element or on the parent element (for Event Delegation), here is example of this solution:

```html
<div id="yourwidget">
</div>
```

```javascript

let yourwidget = document.querySelector('#yourwidget');
let shouldDrag = false;

let handleMousedown = function(e) { 
  // record mouse position
  shouldDrag = true;
}
let handleMouseup = function(e) { 
  shouldDrag = false;
}
let handleMousemove = function(e) {
  if (!shouldDrag) return;
  // calculate the offsets, and set the new position for the element
} 

yourwidget.addEventListener("mousedown", handleMousedown);
yourwidget.addEventListener("mouseup", handleMouseup);
yourwidget.addEventListener("mousemove", handleMousemove);
```

The problem of this solution is:
if user moves mouse too fast and somehow the element cannot catch up with you mouse. At that time, the 'mousemove' event listener will not be trigged anymore, and user need to move the mouse back to the element to trigger the listener, which will offer a bad user experience.

some solution for this issue case:
1. you can use event delegation to add listener to a big enough element. This solution works well for most of the time. But adding event listener on a big element may have side effect on some of your existing listeners code.
2. (suggested) In the real production environment, what I used is to add some temprary capture phase event listener on document, see example below:

```html
<div id="yourwidget">
</div>
```
```javascript

let yourwidget = document.querySelector('#yourwidget');
let shouldDrag = false;

let handleDrag = function(e) {
  // avoid side effect on other listeners
  e.stopImmediatePropagation();
  
  // calculate the offsets, and set the new position for the element
}
let handleDragend = function(e) {
  e.stopImmediatePropagation();
  document.removeEventListener("mousemove", handleDrag, true);
  document.removeEventListener("mouseup", handleDragend, true);
  shouldDrag = false;
}
let handleMousedown = function(e) { 
  // record mouse position
  shouldDrag = true;
}
let handleMouseup = function(e) { 
  shouldDrag = false;
}
let handleMousemove = function(e) {
  if (!shouldDrag) return;
  document.addEventListener("mousemove", handleDrag, true);
  document.addEventListener("mouseup", handleDragend, true);
} 

yourwidget.addEventListener("mousedown", handleMousedown);
yourwidget.addEventListener("mouseup", handleMouseup);
yourwidget.addEventListener("mousemove", handleMousemove);
```
