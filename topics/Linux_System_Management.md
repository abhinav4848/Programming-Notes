# System Management

#### Man
shows a manual page for that option. Like help

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
Traditionally, the logger is called `syslog`. On linux/Ubuntu, the implementation is `rsyslog`.

`.d` stands for directory.

it's in `/etc/rsyslog.d`.The configuration is in `rsyslog.conf`. The conf file will tell where the logging is being done. Most likely `/var/log`


## 05_04-Service Management
Services are the programs that run in the background. 
Startup services are Found in `/etc/init.d`.
Run level services in `/etc/rc3.d`
You can run status on a service like `sudo /etc/init.d/snbd status`
or `sudo service --status-all`

We can start and stop services from here.

## 05_05-Process Management - ps And top
Process is a running instance of a program. It has a process entry in the process table.
`ps`: Process statistics
`ps a`: More details
`ps eaf`: Lots of details
`ps aux`: shows all processes running on system regardless of which user

`top`: Shows auto updating list of CPU process using processes.

## 05_06-Process Management - kill And killall
get the process id by searching for something like `ps aux | grep firefox` and copy the process id. Now type `kill <process number>`

or `killall firefox`
or `killall -9 firefox` Kills process regardless of what state it is in.

## 05_07-Building Software
* say you get a zip file of source code.
* Extract it
* go into the extracted directory
* Run `./configure` script that is provided in the package to make sure all components are in place to build software.
* There exists a `makefile` that sets variables. don't bother with it
* Next type `make` which compiles and builds the software
* Next type `sudo make install`

## 05_08-Useful Utilities - grep, sed
grep: Global replace. Used to search string patterns in files. 
`ps aux | grep firefox`: narrow down the results of `ps aux` to ones containing firefox.
The pipe `|` means take the output of first command and send it to the second one.
`grep file *`: search for the word 'file' among everything in the current directory but not deeper.
`grep -r file *` does recursive search into subfolders as well.

`sed`: Stream editor

`echo this is true`
`echo this is true | sed 's/true/false/'`
`grep file * | sed 's/file/non-file/` replaces the first instance only. add `/g` at the end to replace all instances.
`cat myfile | sed 's/file/foo/' > myfoo` opens myfile but sends output to sed which replaces file with foo and then send this output to myfoo.

## 05_09-Kernel Modules


## 05_10-Kernel Config - sysctl
## 05_11-User Management - Files
## 05_12-User Management - Adding And Changing Users
## 05_13-User Management - Groups
## 05_14-Using Cron To Automate Tasks
## 05_15-Using Sudo
