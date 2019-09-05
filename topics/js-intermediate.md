## Bulk Event Listeners
```javascript
document.querySelectorAll('#myTable td')
.forEach(e => e.addEventListener("click", function() {
    // Here, `this` refers to the element the event was hooked on
    console.log("clicked")
}));
```
Thit creates a separate function for each cell; **instead, you could share one function without losing any functionality.**

```javascript
function clickHandler() {
    // Here, `this` refers to the element the event was hooked on
    console.log("clicked")
}

document.querySelectorAll('#myTable td')
.forEach(e => e.addEventListener("click", clickHandler));
```

**Example**
```javascript
var hover = document.querySelectorAll('.hover');

// The functions are cointained inside another one to prevent execution on load.
hover.forEach(e => e.addEventListener("mouseover", () => mouseOver(e)));
hover.forEach(e => e.addEventListener("mouseout", () => mouseOut(e)));

function mouseOver(e) {
    e.classList.remove("w3-black");
    e.classList.add("w3-teal");
}

function mouseOut(e) {
    e.classList.remove("w3-teal");
    e.classList.add("w3-black");
}
```

## Submit
Submitting a form registers an event for a split second and it vanishes. In order to prevent that and make it persistent, we need to use the event method `preventDefault()` i.e. it stops the normal form submission to a file.  

```javascript
form.addEventListener("submit", runEvent);

function runEvent(e){
    e.preventDefault();
    console.log(e.target.value); // Logs out the input value. Or selector value.
}
```