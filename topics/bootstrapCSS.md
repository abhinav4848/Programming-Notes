# Bootstrap
## Add a search box with cancel button, and JS instant Search features
```html
 <input type="search" name="searchTerm" class="form-control" id="searchField" autocomplete="off" placeholder="Enter search term here">
```

```css
/*restore the cancel button on searchbox that bootstrap breas*/
input[type="search"]::-webkit-search-cancel-button {
    -webkit-appearance: searchfield-cancel-button;
}
```

```js
// if any key is entered, fire searchField()
document.querySelector('#searchField').addEventListener('keyup', searchField, false);
// if the "x" button is clicked on search box, fire searchField()
document.querySelector('#searchField').addEventListener("search", searchField, false);
```