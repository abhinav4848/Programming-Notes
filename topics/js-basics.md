# HTML Elements
aka tagNames. `<a>, <li>, <body>...`
```javascript
var x = document.getElementById("myP").tagName; //returns in all Caps.
```
The HTML DOM Element Object- [Properties and Methods](https://www.w3schools.com/jsref/dom_obj_all.asp), [Styles](https://www.w3schools.com/jsref/dom_obj_style.asp), [Attribute Object](https://www.w3schools.com/jsref/dom_obj_attributes.asp), [DOM Events](https://www.w3schools.com/jsref/dom_obj_event.asp)
# Attribute
`<p class="nice">Demo text</p>`. "class" is the attribute and "nice" is the value.

# Nodes
According to the W3C HTML DOM standard, everything in an HTML document is a node:

```markdown
1. The entire document is a document node
2. Every HTML element is an element node
3. The text inside HTML elements are text nodes
4. Every HTML attribute is an attribute node (deprecated)
5. All comments are comment nodes
```

**DOM Root Nodes**: There are two special properties that allow access to the full document. Extract their contents by using `.innerHTML`

```javascript
document.body //The body of the document
document.documentElement //The full document
```

**Navigating Between Nodes**: You can use the following node *properties* to navigate between nodes with JavaScript: [Link to W3S](https://www.w3schools.com/js/js_htmldom_navigation.asp)

```javascript
1. parentNode
2. childNodes[nodenumber]
3. firstChild
4. lastChild
5. nextSibling
6. previousSibling
```

**Finding HTML Elements**

Method | Description
-|-
document.getElementById(id) | Find an element by element id
document.getElementsByTagName(name)	| Find elements by tag name
document.getElementsByClassName(name) | Find elements by class name
element = document.querySelector(selectors) | Returns the first Element within the document that matches the specified selector, or group of selectors. If no matches are found, `null` is returned.
document.querySelectorAll("p.intro") | Find all HTML elements that match a specified CSS selector (`id`, `class names`, `types`, `attributes`, `values of attributes`, etc). This example returns a list of all `<p>` elements with class="intro".
document.getElementsByName("up") | live list of elements with that name

Example of retrieving the text from `<title id="demo">DOM Tutorial</title>`:

```javascript
var myTitle = document.getElementById("demo").innerHTML;
var myTitle = document.getElementById("demo").firstChild.nodeValue;
var myTitle = document.getElementById("demo").childNodes[0].nodeValue;
```

**nodeValue**: specifies the value of a node.
```markdown
nodeValue for element nodes is null
nodeValue for text nodes is the text itself
nodeValue for attribute nodes is the attribute value
```

**Changing HTML Elements**

Property | Description
-|-
element.innerHTML =  new html | Change the inner HTML of an element
element.attribute = new value | Change the attribute value of an HTML element
element.style.property = new style; | Change the style of an HTML element

```javascript
var element = document.getElementById("id01");
element.innerHTML = "<div>Text</div>";

document.getElementById("myImage").src = "landscape.jpg";
document.getElementById("p2").style.borderBottom = "solid 1px red";
```

Method | Description
-|-
element.setAttribute(attribute, value) | Change the attribute value of an HTML element

Method | Description
-|-
Node.createElement(element) | Create an HTML element
Node.appendChild(element) | Add an HTML element to the end
Node.write(text) | Write into the HTML output stream
Node.hasChildNodes() | Returns a Boolean indicating if the element has any child nodes, or not.
Node.insertBefore() | Inserts a Node before the reference node as a child of a specified parent node.
child.parentNode.removeChild(child); | find the child node using getelementsby... then remove it in one go
parent.removeChild(child); | find both nodes first then remove child from parent
parentNode.replaceChild(newChild, oldChild); | Replaces one child Node of the current one with the second one given in parameter.

Note: `document` is a node.

```javascript
var para = document.createElement("p"); //<p></p>
var node = document.createTextNode("This is new."); //"This is new."
para.appendChild(node); //<p>This is new.</p>

var element = document.getElementById("div1"); //gets element with children
element.appendChild(para); //adds to the end of element
```

**Adding/Removing Events Handlers**:
[W3S Link](https://www.w3schools.com/js/js_htmldom_eventlistener.asp)

Method | Description
-|-
document.getElementById("myBtn").onclick = function(){code}| Adding event handler code to an onclick event. can give the function name or not. Doesn't matter
document.getElementById("myBtn").addEventListener(event, function [,useCapture]); | Event can be any [HTML DOM event](https://www.w3schools.com/jsref/dom_obj_event.asp), for useCapture write true or false
removeEventListener() | remove

**Using events**
```javascript
function buttonClick(e){
    console.log(e); // Logs the event.
    console.log(e.target); // The clicked element.
    console.log(e.target.id); // Clicked element id.
    console.log(e.target.className); // Clicked element class.

    console.log(e.ctrlkey); // Is control pressed boolean. altkey, shiftkey...
}
```

## Add or remove attributes
```javascript
document.querySelector("title").getAttribute("id")
document.querySelector("title").setAttribute("attribute", value)
```

# Others
**`tagName` vs `nodeName`**:
You can use the tagName property to return the tag name of an element. The difference is that `tagName` only return tag names, while `nodeName` returns the name of all nodes (tags, attributes, text, comments).

```markdown
Possible values:

1. Returns the tagname for element nodes, in uppercase
2. Returns the name of the attribute for attribute nodes
3. Returns "#text" for text nodes
4. Returns "#comment" for comment nodes
5. Returns "#document" for document nodes
```

## Links
1. [Nodes](https://developer.mozilla.org/en-US/docs/Web/API/Node) on MDN
2. [Document](https://developer.mozilla.org/en-US/docs/Web/API/Document) on MDN
3. [Window](https://developer.mozilla.org/en-US/docs/Web/API/Window) on MDN

## Notes
1. `.item(0)` is same as `[0]`
2. `onmouseup`,`document`, etc are properties of **Window** object. Hence `window.document.getElementById("header");` is same as `document.getElementById("header");`
3. `window.alert(0)` and `document.write("hi")` are correct and not vice versa.
