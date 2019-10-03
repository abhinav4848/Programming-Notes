Node.js uses the chrome V8 engine to run javascript outside of the browser. 

# Install

```bash
# linux
sudo apt install nodejs -y

# Check installation success
node -v
npm -v

# update npm to latest
npm install npm@latest -g
```

Install the LTS version from [website](https://nodejs.org/en/download/). Works for updating node too.

# Create an npm project
Go to a project folder and run `npm init`. It will ask a few questions and generate a `package.json` file.

NPM allows to install packages 2 ways.
-   **Locally**: Runs only in the project folder. `npm install modulename`. Installs locally in project folder in a folder called `node_modules`, and adds it as a dependency into `package.json`'s dependencies section.
-   **Globally**: Can run anywhere. Mostly in terminal: `npm install -g live-server`. In this case, `package.json` won't get updated.

**Notes**
1. If installing gives admin errors, run as `sudo`. 
2. `npm install` installs all dependencies from `package.json`.
3. Dev dependencies are not needed for deployment so they're added to `package.json`'s dev dependencies.

**Scripts**:
`npm run scriptname` will run a bash command as specified in the `package.json`. eg:
```json
"scripts" : {
    "build": "browserify script.js > bundle.js",
    "buildtest": "browserify script.js > bundle.js && live-server"
}
```