# Linux File Management
`--help`

## 4.1 Listing Files
`ls --help`

. is the current directory

.. is the directory that's above

`ls -a` (includes (dot) files). The dot files are usually configuration files. As well as include the current firectory(.) and above directory(..)

`ls -la` also includes creation date and more.

`ls -lah` (h means human readable. Things like file size in kbs get converted to mb/gb)

`ls -la ric` (details only about the file named ric)

Executable files have green colour, . files have blue colour

## 4.2 File Permissions 
`touch ric` (creates file by name ric)

`ls -la ric` (will list file permissions on extreme left)

Permissions under Unix are grouped into User(u), Group(g), World(w)/Everybody.
Total 10 dashes are present for a file. 

First Dash tells if it's a directory(d) or special file (-), etc.
Next set of 3 dashes are for Owner, next set of 3 for Group, Last set for World.

Each dash in the set denotes following with a representative numerical value arranged as exponent of 2.
1. Read (r ): 4 (2^2)
2. Write (w): 2 (2^1)
3. Executable (X): 1 (2^0). In unix, we can make anything executable simply by putting an X flag on it. 

So to say, executable files are handled differently in Linux than Windows. 
Their sum total is 7 Which means all "rwx" are present.

`chmod u+w` (adds the write flag for the "User")
`chmod g-w` (removes write from "group")
`chmod 744` (owner gets all, Group and World get read)

## 4.3 File Management: Copy, Move, rename 
`~` just means home directory. It is `/home/abhinav`

`ls ~` is listing for home directory

Everytime you begin a path with /, it's absolute path so wherever you are, if you type `cd /usr/`, it takes you to the root folder "usr" otherwise if you type `cd usr` it'll take you to sub folder of current working directory.

* `pwd`: print working directory
* `cp ric Downloads` (copies the file to Downloads)
* `rm Downloads/ric` (deletes ric from downloads. There's no recycle bin here)
* `mv ric Downloads` (moves ric from "pwd" to Downloads)
* `cd Downloads/` (move pwd to to Downloads)
* `cd ..` move up one directory
* `mv ric myfile` (renames ric to myfile)
* `rm -i` filename (prompts if you really wanna delete)
* `rm *` removes all files (gives error for folders)
* `rmdir` removes folders (if empty only. user -r to remove recursively)
* `rm -rf *` (removes even the full folders)

## 4.4 Filesystem 
`/` is the root for all the filesystem. There's no Drives concept.

`ls /` is listing for as high as it goes.

1. bin: (binary) That's where programs live
2. etc: (pronounced etsy) All the configurations, programs and utilities is.
3. boot: OS is in there.
4. cdrom: Mount point. Here is where a CD inserted will mount into filesystem
5. dev: where all the devices are. Under linux, every device gets its own file. It's a pseudo file put in the file system by the OS so use can interact with the devices. 
6. home: where all the home directories for the different users are.
7. lib: libraries. (something like Windows dll)
8. lib64: where 64 bit libraries are
9. media: where a usb stick would be mounted. 
10. mnt: For mounting more removable media.
11. opt: Optional software. 
12. proc: Processor related data. Pseudo entry in the file system. Every process gets its own directory.
13. root: root Home directory.
14. run: where temp data is stored.
15. sbin: system binaries/system programs
16. srv: server
17. sys: system data. Pseudo file system
18. tmp: swap files
19. usr: user related data. User accessible binaries. bin and sbin in usr are more user oriented and the ones outside usr are system oriented.
20. var: any variable data. Here, logs are stored. 

## 4.5 Path Variable 
Its tells where the system should look for programs/executables.
`echo $PATH` (it looks in one of these directories)
Listing is separated by colons(:) here.

### There are two types commands here.
One is the inbuilt function of the shell which works whatever the path is.
The other is executable functions located at one of the paths listed in $PATH

`echo ~/bin` outputs:
/home/abhinav4848/bin (where abhinav4848 is my user id)

`echo ~` outputs:
/home/abhinav4848

`export PATH=$PATH:~/bin`:
this concatenates /home/abhinav4848/bin to PATH

However, if `export PATH=~/bin:/usr/local/games` is done, then PATH variable is overwritten with just the values that I input. 
Now, a function like "ls" won't work since it's an executable function located at one of the paths listed in `$PATH`. Shell will even tell you it's supposed to be at /bin/ls but that it could not be found since the path is not included in PATH. TO add that, just "export PATH=$PATH:/bin

You can run a program not in path by typing full location to the program. So `ls` is in bin. but if you remove bin from path, you could still call ls by using `/bin/ls` and clear is in `/usr/bin/`

However the PATH changes are only till shell is closed cuz the configuration file is not modified and it loads afresh on a fresh shell run.

## 4.6 Editing Files 
Files can be edited by programs like nano (simple stuff) or vi (complex stuff where to insert, you gotta type i first then insert characters, then add to the end of the line via capital A. Vi is older and quite popular as well, but very diff from any other editor out there)

`echo this is a file > myfile.txt`

`cat myfile` will show file contents once since it concatenated myfile with nothing and output that.

`cat myfile myfile` will show twice

`cat` (for concatenate) concatenates as many files as we want and prints them to screen

`nano myfile` will enter nano editor and edit the file.

`vi myfile` will enter vi

## 4.7 Pseudo Filesystems - dev, proc And sys 
They are in kernel, therefore all distros have it.

`ls /dev`: where all the devices of system are stored (as an entry) so we can directly interact with these devices

`ls /proc`: process id stored here
The numbers correspond to process ids of running process.
The very first process starting on linux is called init which gets its proc id as 1.

	cd /proc/1
	ls
	sudo cat cmdline

Above lines give `/sbin/initabhinav4848@ubuntu:/proc/1$`

This was the command line used to actually launch the program. (`/sbin/init`)

We can see other stuff like memory alloted to it, anything related to the network. 

`ls /sys`
here are the settings existing the kernel aka OS.
we may wanna change the flags on one of these system setting to have our OS behave behave in a particular way and we could do this either by digging through /sys directories till we find what we need or by using the `sysctl` program to make changes to these system control variables

## 4.8 File Sharing 
samba package does the file sharing bit.
It's in `/etc/samba`
Go there by cd

The config file is accessed `sudo vi smb.conf`

smbclient to connect to remote systems doing file sharing

## 4.9 Locating Files - find and which 
`which` looks for executables or programs. eg: `which python` or `which ls`. It searches the path to find location.

`sudo find / -name logs -print` means:
1. `sudo`: increase permission
2. `/`: start from root, or `.`: start from current directory
3. `-name`: 
	mention the filename called logs 
	even use wildcard: `"*log*"`
	or use `"*[dD]esktop"` (asterisk means anything before this is fine)
4. `-print`: whenever you find a file fitting criteria, print it

## 4.10 Redirection 
`>` is the redirect thing. It creates the file if it doesn't exist or overwrites the preexisting file with new data.
`>>` appends to a file (creats if non-existent)
Redirection is taking the output of one program and sending it to a file.

`ls > ls.txt` (redirects the output to the file instead of the screen)
`cat ls.txt` (displays the file content)

**I/O streams**
1. stdout
2. stderr (Numeric value for stderr is 2)
3. stdin

`find / -name passwd -print 2> errors.txt`
Find in "Root" for name "passwd" and catch the errors ("2") and instead of output to screen, redirect them to errors.txt and eventually it's going to print the files with value passwd in them. (i.e., it sends the errors elsewhere but prints the success onto screen)

## 4.11 Special Files - `devnull`, `devzero` And `dev/urandom` 
`find / -name passwd -print 2> /dev/null` # sends errors to sinkhole
`ls > /dev/null` # sends all output to devnull
This is a data sink. Any thing sent here is gonna disappear and you'll never see them again.

`ls > /dev/zero` works similarly to dev/null

`dd if=/dev/zero of=zero.txt count=250`
(disk dump command)

`cat zero.txt` gives no output cuz it's the "null zero" and not zero character"

`ls -la zero.txt` will give a file of significant size containing lots of null characters.

`cat /dev/urandom`
(that's a random number generator. The shell ends up showing non printable characters to screen cuz the /dev/urandom outputs numbers which the shell then tries to convert to ASCII characters from the number combinations. So you don't wanna dump urandom to screen but rather redirect to another file from where value will be interpreted.)

## 4.12 Dot Files 
`.` is local directory
`..` is directory above

`../..` will take you two steps up in the directory tree.
`../../etc/passwd` will take you 2 steps up then navigate to `/etc/passwd`

## 4.13 Symbolic And Hard Links 
### Symbolic link: 
`ln` creates the link to a filename. Symbolic link refers to the filename regardless of where data actually is. So if we delete the original file, the link will become a broken link.

`ln -s /etc/passwd passwd` creates a link to `/etc/passwd`

`ls -la passwd` shows link like `passwd -> /etc/passwd`

### Hard Link
`ln ls.txt lslink.txt` creates a hard link

`ls lslink.txt` is a file that points to the same place on disk as ls.txt, so in our output from ls, we'll see a "2" showing 2 references to that data on disk exist.

if we remove either of these files and do ls, then the number of reference links will become "1". Thus, the data on disk is still intact cuz there's still more reference to it. 

## Compression 
`zip myfile.zip myfile*` (create a myfile.zip with contents all files starting with myfile)
`ls -la myfile.zip` (to see the file is created)
`cd Downloads` (go to some other directory just to show unzipping here)
`unzip ../myfile.zip `

### TAR: tape archive

`tar cf mytar.tar myfile*` (to just bundle all files into one tarball without compression)

`tar cfz mytar.tar.gz myfile*`
1. `c`: create
2. `f`: filename rather than writing out tot tape or something else
3. `z`: compress
4. `v`: verbose (just to see what's going on)

`tar xfzv filename.tar.gz`
1. `x`: extract
2. `f`: filename (opposed to device name)
3. `z`: cuz we have zip file
	