# System Management
## 05_01-Package Sources And Updating
`cd /etc/apt`: where we store configuration files and various things related to configuration. 
`vi sources.list` is a file telling anywhere we can get software.
* deb: Debian (Debra + -ian is surname of guy who built debian, from which ubuntu has modded itself)
`sudo apt-get update` gets us list of packages from repositories that i can install. A software updater will also see to let us know if any outdated software exist.

## 05_02-Package Management - Search, Install And Remove 
`sudo apt-get upgrade` actually installs the packages. 
`apt-get update` **updates** the list of available packages and their versions, but it does not install or **upgrade** any packages. `apt-get upgrade` actually installs newer versions of the packages you have.

### Search for a software
`apt-cache search emacs` searches for any software that says emacs. Relevant or irrelevant. 

### Install
`sudo apt-get install emacs vi postfix` installs emacs, vi, postfix
`sudo apt-get remove emacs` removes only emacs. Not the other dependencies it installed along with emacs.

### Remove
`sudo apt-get autoremove` searches for all packages that are not being used in some way and removes them.

## 05_03-Logging And Log Management
