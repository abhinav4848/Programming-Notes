## Bulk Event Listeners
```javascript
document.querySelectorAll('#myTable td')
.forEach(e => e.addEventListener("click", function() {
    // Here, `this` refers to the element the event was hooked on
    console.log("clicked")
}));
```
This creates a separate function for each cell; **instead, you could share one function without losing any functionality.**

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

# Promises
Going over a promise step-by-step.

1. First, try a pure `fetch`:
```js
fetch('https://jsonplaceholder.typicode.com/users')
>>> Promise {<pending>}
```

2. Now output the raw response
```js
fetch('https://jsonplaceholder.typicode.com/users')
    .then(response => console.log(response))
>>> Promise {<pending>}
>>> Response {type: "cors", url: "https://jsonplaceholder.typicode.com/users", redirected: false, status: 200, ok: true, …}
> body: ReadableStream
> bodyUsed: false
> headers: Headers {}
> ok: true
> redirected: false
> status: 200
> statusText: ""
> type: "cors"
> url: "https://jsonplaceholder.typicode.com/users"
> __proto__: Response
```

3. Now convert the response to json.
```js
fetch('https://jsonplaceholder.typicode.com/users')
    .then(response => response.json())
>>> Promise {<pending>}
```

4. Now output the json data.
```js
fetch('https://jsonplaceholder.typicode.com/users')
    .then(response => response.json())
	.then(data => console.log(data))
>>> Promise {<pending>}
>>> (10) [{…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}]
```

## Simple example of a promise
A promise starts working the moment it is created. The `.then` is  to process the output. 
```js
const promise = new Promise((resolve, reject) => {
    if (true) {
        resolve('Stuff worked')
    } else {
        reject('Error, it broke')
    }
})

promise
    .then(result0 => result0 + '!')
    .then(result1 => {
        console.log(result1);
        return result1 + '?'
    })

    .then((result2) => console.log(result2 + '...'))
    .then(() => {
        //result2 is lost cuz no return statement was present above
        //so no use of accepting arguments to this f()
        console.log('Dont mind me')
    })

    .catch(() => {
        //only runs for errors occuring before it
        console.log('Error!')
    })
    .then(result3 => {
        //result3 is undefined
        throw Error('Oops result is lost here.');
        console.log(result3 + '?') // line won't run cuz of throw Error above
    })
    .catch((e) => {
        //if error is not caught, promise is rejected
        console.log('Error at the end: ' + e)
    })
```

**Output:**

`if(true)`
```console
Stuff worked!
Stuff worked!?...
Dont mind me
Error at the end: Error: Oops result is lost here.
Promise {<resolved>: undefined}
```
`if(false)`
```console
Error!
Error at the end: Error: Oops result is lost here.
Promise {<resolved>: undefined}
```

## Independence of promises
Promises can be made separate and run together.

Only the variables take time to get their values. Not `Promise.all()`. It's just there to process it. It will only work when all have been resolved/rejected, but not pending. So if you run the variables and then after some time, do the `Promise.all`, it will run instantly cuz all variables will have been executed and will just be waiting to be processed. 
```js
const promise0 = new Promise((resolve, reject) => {
    resolve('Stuff Worked')
})

const promise1 = new Promise((resolve, reject) => {
    setTimeout(resolve, 100, 'HII');
})

const promise2 = new Promise((resolve, reject) => {
    setTimeout(resolve, 1000, 'Pookie');
})

const promise3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('Looking for me?');
    }, 3000);
})

Promise.all([promise0, promise1, promise2, promise3])
    .then(values => {
        console.log(values);
    })
```

Output:

```console
>>> Promise {<pending>}
>>> (4) ["Stuff Worked", "HII", "Pookie", "Looking for me?"]
> 0: "Stuff Worked"
> 1: "HII"
> 2: "Pookie"
> 3: "Looking for me?"
> length: 4
> __proto__: Array(0)
```
## Questions
1. Create a promise that resolves in 4 seconds and returns "success" string
```js
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve("success");
    }, 4000)
});
```

2. Run the above promise and make it console.log "success"
```js
promise.then(console.log)
// or
promise.then(resp => console.log(resp))
```

3. Read about `Promise.resolve()` and `Promise.reject()`. How can you make the above promise shorter with `Promise.resolve()` and console loggin "success"
```js
const promise = Promise.resolve(
    setTimeout(() => {
        console.log("success");
    }, 4000)
);
```

4. Catch this error and console log 'Ooops something went wrong'
```js
Promise.reject('failed')
    .catch((e) => console.log('Ooops something went wrong' + e))
```

5. Use Promise.all to fetch all of these people from Star Wars (SWAPI) at the same time. Console.log the output and make sure it has a catch block as well.
```js
const urls = [
    'https://swapi.co/api/people/1',
    'https://swapi.co/api/people/2',
    'https://swapi.co/api/people/3',
    'https://swapi.co/api/people/4'
]

Promise.all(urls.map(url =>
        fetch(url).then(people => people.json())
    ))
    .then(array => {
        console.log('1', array[0])
        console.log('2', array[1])
        console.log('3', array[2])
        console.log('4', array[3])
    })
    .catch(err => console.log('ughhhh fix it!', err));
```
6.  Change one of your urls above to make it incorrect and fail the promise. Does your catch block handle it?
```console
Yes. Only that wrong url will throw error. rest will work.
```

# Async-Await
**Example 1:**

*Old method*: ***[ES2015/ES6]*** Assume a fetch that returns a promise.
```js
fetch('https://jsonplaceholder.typicode.com/users')
    .then(response => response.json())
	.then(data => console.log(data))
```

*New method*: ***[ES2017/ES8]*** Async await is a syntactic sugar.
```js
// use async to let js know we using async await
async function fetchUsers(){
    const response = await fetch('https://jsonplaceholder.typicode.com/users'); // pause till this await works
    const data = await response.json(); //then pause till this await works
    console.log(data);
}

fetchUsers(); // run it
```

**Example 2:**

Let these be the URLs
```js
const urls = [
    'https://jsonplaceholder.typicode.com/users',
    'https://jsonplaceholder.typicode.com/posts',
    'https://jsonplaceholder.typicode.com/albums'
]
```

*Old method*: ***[ES2015/ES6]*** Multiple URLs being handled in one Promise. if any url is wrong, or an error is thrown, it will be handled only for that url. 
```js
Promise.all(urls.map(url => {
    return fetch(url).then(resp => resp.json())
})).then(results => {
    console.log(results[0])
    console.log(results[1])
    console.log(results[2])
}).catch((e) => console.log('error'))

alert(0) // will run earliest
```
The above will output all of the urls. But in the meanwhile, the program will proceed ahead and `alert(0)` will run as usual.

*New method*: ***[ES2017/ES8]***
```js
const getData = async function() {
    try {
        const [ users, posts, albums ] = await Promise.all(urls.map(url =>
            fetch(url).then(resp => resp.json())
        ))
        console.log('Users', users)
        console.log('Posts', posts)
        console.log('Albums', albums)
    } catch (err) {
        console.log('Oops', err)
    }
}

getData()
```

*Newer method*: ***[ES2018/ES9]*** `Await-of` feature, using `for of`.
```js
// Basic for-of example
const loopThroughUrl = (urls) => {
    for (url of urls) {
        console.log(url)
    }
}
```
In below case, `for-await` takes each item from the array and waits for it to resolve. You'll get the first response even if the second response isn't ready yet, but you'll always get the responses in the correct order.

Using `for-of` on previous code example.

```js
const getData2 = async function () {
    const arrayOfPromises = urls.map(url => fetch(url));
    for (const request of arrayOfPromises) {
        console.log(request);
    }

    for await (const request of arrayOfPromises) {
        const data = await request.json();
        console.log(data)
    }
}

getData2()
```

# Finally
```js
const urls = [
    'https://swapi.co/api/people/1',
    'https://swapi.co/api/people/2',
    'https://swapi.co/api/people/3',
    'https://swapi.co/api/people/4'
]

Promise.all(urls.map(url => fetch(url).then(people => people.json())))
    .then(array => {
        throw Error
        console.log('1', array[0])
        console.log('2', array[1])
        console.log('3', array[2])
        console.log('4', array[3])
    })
    .catch(err => console.log('ughhhh fix it!', err))
    .finally(() => console.log('extra action here'))
```
Output
```console
Promise {<pending>}
ughhhh fix it! ƒ Error() { [native code] }
extra action here
```

# JSON
[JSON.parse](https://www.w3schools.com/js/js_json_parse.asp), [JSON.stringify](https://www.w3schools.com/js/js_json_stringify.asp)

## `JSON.parse()`
### Standard method:
```js
var txt = '{"name":"John", "age":30, "city":"New York"}'
var obj = JSON.parse(txt);
document.getElementById("demo").innerHTML = obj.name + ", " + obj.age;
```
### Using Ajax to receive JSON:
Imagine a json response from a file called `json_demo.txt`, whose output is:
```json
{
    "name":"John",
    "age":31,
    "pets":[
        { "animal":"dog", "name":"Fido" },
        { "animal":"cat", "name":"Felix" },
        { "animal":"hamster", "name":"Lightning" }
    ]
}
```
Old way of using Ajax and reading json the json response:
```js
var xmlhttp = new XMLHttpRequest();
xmlhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    var myObj = JSON.parse(this.responseText);
    document.getElementById("demo").innerHTML = myObj.name;
  }
};
xmlhttp.open("GET", "json_demo.txt", true);
xmlhttp.send();
```
New way:
```js
fetch('json_demo.txt')
    .then(response => response.json())
	.then(data => document.getElementById("demo").innerHTML = data.age)
```

## `JSON.stringify()`
```js
var obj = { name: "John", age: 30, city: "New York" };
var myJSON = JSON.stringify(obj);
document.getElementById("demo").innerHTML = myJSON; 
// Stringified output
// {"name":"John","age":30,"city":"New York"}
```

# Questions
1. [Difference between $(this) and event.target?](https://stackoverflow.com/a/21667010/2365231)
2. 