# Getting started
go to https://create-react-app.dev/docs/getting-started. You don't even need to have `create-react-app` installed anymore. Just follow as advised.

```bash
npx create-react-app my-app
cd my-app
npm start
```
If you modify any file in `package.json`, run `npm install` to update.

# Default starting code
1. `index.js` is starting file and we use `index.css` for it.
2. If we don't mention extension, it assumes `.js`. Eg: `./App`.
3. If we start a file with capital letters, it's a component.
4. We use `classname` instead of `class` in html cuz latter is a reserved keyword.
5. Add this to your vscode `settings.json` as default for `.js` files.
```json
"files.associations": {
        "*.js": "javascriptreact"
    }
```
6. We can only return one root html parent from our function.

index.js: 
```jsx
import React from 'react';
import ReactDOM from 'react-dom'; //other packages: react-native

import './index.css';
import App from './App';

import * as serviceWorker from './serviceWorker';

//ReactDOM package, render function, <App /> has same name as import "App" above. 
//The file App.js can have any function name which it will then export
//The "root" id is at public/index.html
ReactDOM.render(<App />, document.getElementById('root'));

serviceWorker.unregister();
```

# Basic css framework
1. `npm install tachyons`. [Site](https://tachyons.io/components/).
2. `import 'tachyons';` in your `index.js`.
3. Use its classes in your components. (`tc`, `f1`, etc...)

# Basic Component
index.js:
```jsx
ReactDOM.render(
    <Hello greeting={'Hello, ' + (String)(2 + 2) + ' React Ninja'} />,
    document.getElementById('root')
);
```

Hello.js:
```jsx
import React, { Component } from 'react';
import './Hello.css';
import logo from './logo.svg';

class Hello extends Component {
  render() {
    return (
      <div className="tc">
        <h1 className="f1">Hi there</h1>
        <p>Welcome to React</p>
         <img src={logo} className="App-logo" alt="logo" />
        <p>{this.props.greeting}</p>
      </div>
    );
  }
}

export default Hello; 
```

# Multiple components
We have to wrap all components into one root component.

```jsx
ReactDOM.render(
    <div>
        <Hello />
        <Hello />
    </div>, 
    document.getElementById('root')
);
```

# Exporting and Importing
When only one thing being returned from a component, use `default` as export: 
```jsx
// export
export default FuncName;

// import
import AnyFuncName from './FuncFile';
```

When we are to export multiple, we export using a name, and import using destructuring: 
```jsx
// export
export const robots = [
    {
        id: 1
    },
    {
        id: 2
    }
]

// Import (via destructuring)
import { robots, cats } from './robots';
```

Same for exporting json. Basically, use `{}` to import, when not exporting `default`.
```jsx
// export
export const particlesOptions = {
  //json here
}

// import
import { particlesOptions } from './particleoptions';

// later in the code...
<p>{particlesOptions.particles.number.value}</p>
```