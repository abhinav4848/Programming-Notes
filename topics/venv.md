# VENV
Source: [How to Use Virtual Environments with the Built-In venv Module- Corey Schafer](https://www.youtube.com/watch?v=APOPm01BVrk)

## Create Project
in cmd, type `mkdir project_name`, then enter your project directory

## Virtual environment
### Making
In cmd, enter: `python -m venv venv`

#### Venv with all system libraries
In cmd, enter: `python -m venv venv --system-site-packages`

**Legend**:
* -m: *searches for a module of specified name after -m*
* venv: *module name*
* venv: *create a folder with this name and put a virtual environment inside*
* *system-site-packages*: copies all system libraries to this venv. Any new library you install in this env is still available locally only, not affecting the system.

### Activation
Then run `venv\Scripts\activate.bat` to activate virtual environment. 

### Deactivation
Just type `deactivate`.

### Note:
You can create virtual environment anywhere and activate it and work on your project code anywhere. But by convention, you enter your project directory and create and activate virtual environment inside it. 

By default, you name the venv as *venv*. 

You **do not** put any of your project files inside *venv*. Your project files and venv should be at the same directory level. i.e. Immediately inside Project folder.

## Ways to check python location
`where python` tells current python location at the topmost of python installation locations.

You'll also see `(venv)` at the start of every command line as long as virtual environment is active.

## Handling Packages
`pip list` only lists packages installed in this environment
`pip install requests` installs *requests* in the virtual environment.
`pip freeze` gives a list of these packages which you can copy and paste into project folder in a **requirements.txt** file. The list may be longer than what you installed, cuz even dependencies of packages are installed when you install *requests*.

`pip list --local` for only local packages installed in this venv. Same logic for `pip freeze --local`.

## Duplicating a Venv
Make and activate your own venv.
In cmd: `pip install -r requirements.txt` where the requirements.txt is full path to the file.

# CMD Tips
### Delete folder
`rmdir folder_name /s` will delete folder_name and also delete subfolders thanx to the `/s`

### Clear Screen
`cls`

# VSCODE
[Corey Schafer Windows VSCODE guide](https://youtu.be/-nh9rCzPJ20?t=2866)

### .gitignore
You only commit the **requirements.txt** file, not the *venv* itself.  So in *.gitignore*, 
* add the folder name for 'venv'. 
* add '.vscode' for vscode related files.