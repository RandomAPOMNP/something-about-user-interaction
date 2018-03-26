when you want to drag some element on your page, the common idea will be add a listener on the element or on the parent element(for Event Delegation), here is example of this solution:(ignore the memory leak in this example:) )

```html
<div id="yourwidget">
</div>
```

```javascript

let yourwidget = document.querySelector('#yourwidget');
let shouldDrag = false;

yourwidget.addEventListener("mousedown", function(e) {
  // record mouse position
  shouldDrag = true;
})

yourwidget.addEventListener("mouseup", function(e) {
  shouldDrag = false;
})

yourwidget.addEventListener("mousemove", function(e) {
  if (!shouldDrag) return;
  // calculate the offsets, and set the new position for the element
)
```

The problem of this solution is:
if user moves mouse too fast and somehow the element cannot catch up with you mouse. At that time, the 'mousemove' event listener will not be trigged anymore, and user need to move the mouse back to the element to trigger the listener, which will offer a bad user experience.
