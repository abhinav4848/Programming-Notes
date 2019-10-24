# Javascript Snippets
## Read the data-attributes and do ajax stuff
```html
<a href="#" class="delete_history" data-id="volla">Click me</a>
```
```javascript
$(".delete_history").click(function(e) {
    e.preventDefault();

    if (confirm("Sure you want to delete this entry?")) {
        var element = this;
        $.ajax({
            type: "POST",
            url: "functions.php",
            data: {
                id: element.dataset.id // reads the data attribute
            },
            success: function(data) {
                alert(data); // data is the echoed out results of the php page
            }
        });
    }
});
```

# Questions
1. [Difference between $(this) and event.target?](https://stackoverflow.com/a/21667010/2365231)
2. 