# NPM Basic use
Create an `index.html` file with basic html code, and an empty `main.js` file in a folder.
- First do `npm init`.
- It creates `package.json`
- Now install package. Ex: `npm i moment` or `npm install moment`
- It puts the stuff in node_modules, and creates package-lock.json

- put this line in `main.js`
    ```
    var moment = require('moment'); // require

    var mydate = Date();
    var mycooldate = moment(mydate).format('LL');
    console.log(mycooldate);
    ```

- The browser doesn't know about require. Since nodejs is server side.So install browserify

    **Globally:** `npm i browserify -g`, and run in command line: `browserify main.js -o bundle.js`

    or 
    
    **Locally (no -g)** and add to `package.json`. this:
    ```json
    "scripts": {
        "build": "browserify main.js > bundle.js"
    }
    ```
- Then run in command line: `npm run-script build`
- It'll create a `bundle.js`
- Now in `index.html`, set this js file as main entry point: `<script src="bundle.js"></script>`

