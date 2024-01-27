- basic operating system navigation
- file and text manipulation
files folders systems hierarchiacl 
these are called paths
C:\users\cindy\desktop
in windows, file systems are assigned a letter

Root directory, the parent directory for all other directories on the sstem

subdirectories are by backslashes in windows, linux uses forward slashes
path root, then terminates at the end of path

List directories in CLI
`ls` give it the path...

parameters = value with command

`ls C:\`

the c drive is what we call a parent directory (this is too easy right now I can't pay attentino gg.

`get -help -full`

`ls -Force C:\`
lists hidden/non-standard files?

let's see what this process looks like in linux

it starts with the root...

`ls /`
lists the root directory
bin is similar to our windows program files
etc stores some pretty important system config

home = user documents, pictures, like windows users

/proc contains currently running properties

usr is user-installed software

var = logs and files that constantly changed

similar to windows, Linux uses flags, instead of parameters

`ls --help`
we can also try `man` for manual page

do `man ls`, it's like the other, but more detailed

`ls -l/`
similar to windows show properties, `ls -l` will tell us more granular information

1. file permissions
2. links the file has
3. file owner, the group the file belongs to
	1. groups are another way to specify access
4. file size
5. date last modified + time
6. the file or directory name

`ls - a` shows us all files including the hidden ones
order of the flag doesn't matter

you can hide a file or directory by prepending a dot to it

absolute path starts from the main directory
relative path starts from the current directory

powershell usually starts in the home directory
`pwd` print current working directory

cd c:\users\cindy\Documents
	changes to the documents directory...

just go up one folder?

shortcut to the directory above = `..`

`cd ..`
let's go to the desktop folder....

`cd ..\desktop`

pwd - desktop folder

`cd ~` is the shortcut for our home directory
`cd ~\Desktop`

done quite a bit of typing so far
shell has a built in feature called tab completion omfg

`.` just means current directory

USE BASH

very similar/identical commands

`cd /home/cindy/Documents`

or `cd ../Documents`

`cd ~/Desktop` ~ takes us back to the home directory

instead of cycling through options... tab will show us all possible choices

how to add directories 
new folder on Windows, wow.

` mkdir my_cool_folder`



/etc 

