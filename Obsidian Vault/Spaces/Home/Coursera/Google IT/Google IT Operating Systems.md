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

can either do

`mkdir 'my cool folder`
or escape characters
```
mkdir my` `cool` `folder`
```

bash new directory command is the same as windows

the escape character in Bash is `\`

`mkdir my\ cool\ folder`
`mkdir 'my cool folder'`

COMMAND HISTORY IS SICKO

`history` will show you history in powershell

- there is a better use
- i can cycle up and then just do some shit i already know how to do
`ctrl+R` will allow you to search through your history

you can try `# (oldcommand)` which should do the same
let's clean up our shell with the `clean` command

the same `history` command is used in Linux
clear

## Copying files & directories
- copy = `cp`
- `cp mycoolfile.txt C:\Users\Cindy\Desktop``

wildcard 
- character used to select based on a specific type of pattern
- you can select everything with the same file extension, for example
`cp *.jpg C:\Users\Cindy\Desktop`

copy a colder called bird pictures (now we need some recursion up in this bitch)
- again, only copies the directory, but not the files
- need a `-Recurse` parameter with copy to copy everything

`cp -r 'Cat Pictures' ~/Desktop`

what if we wanted to rename something we created
- we can use the `mv` item command
- you can move multiple files by using wild cards
`rm` is only usable by users with permissions to use it

`-Force` 

## THIS IS SO FUCKING BORING
 sys admin role in linux
i can view fiel contents using `cat`
this will dump contents into shell

the `more` command will enter a new program
enter advances the page
space advances 1 page
the q key allows you to leave

`cat fruits.txt -Head 10`
first 10 lines

`cat fruits.txt -Tail 10` 
last 10 lines

can use the same commands in Bash
- the cat key is very similar
- you can do `less` instead of `more`
	- less is more lmao
	- up and down keys
	- pg up pg down
	- g = start, G=end
	- /word_search = search for a word or phrase
	- q, quit out of less
	- 


`head fruits.txt`
`tail fruits.txt`

no good default in the terminal, but we can open notepad++ from cli

`start notepad ++ \\ then filename`

linux, many popular text editors 
- Nano is a very popular text editor
- type `nano` and we can start editing like any other text editor
- Caret symbol ^ = ctrl

PowerShell is a complex and powerful command language
- need some real commands
- - get-help
	- can use ls to look at our alises
`Get-Alias ls`


![[CleanShot 2024-01-26 at 21.20.50.png]]

Get-ChildItem C:\

In Windows, we have 3 ways to execute commands
`cmd.exe` are commands from the old DOS days
- this is equal to `dir`
- `/?` for other commands like `dir /?`
- this is not transferrable
- we are using Powershell and alias commands
windows has a service called the Windows Search Service
- many windows servers, the serach service isn't installed or disabled

let's indexing some shit
- indexing let's us search
- change the settings for the user folder, then click "advanced"
- select file types tab, index properties and file contents
	- click ok
- close out of indexing options
#commandline 
we can use notepad++ as well to do this
- ctrl+shift+f = cow
	- find all, and it returns results
- can search within command line
`sls` select-string command
- matches a pattern that you dreive
	- this is made using regular expressions, or `regex`
	- `Select-String cow farm_animals.txt`
	- `-Filter` lets us search within a directory
	- `ls C:\Program Files\' -Recurse -Filter *.exe`
		- filter the results for file names that match a pattern
bash, we can use the `grep` command
- `grep cow farm_animals.txt`
- 

`echo` is an alias for `Write-Output`
- we use `I/O Streams`
- stdin, stdout, stderr
	- at the CLI the input goes to stdin stream
	- then communicates by putting data in the stdout (where cli outputs)
	- the `>` is a redirector operator, change where want our output to ag o
		- stdin can go to a file, where it overwrites, otherwise, it can create
	- `>>-Append`
	- `echo woof >> dog.txt`
		- send the input of one command to another command
	- `cat words.txt`
		- list of words...
		- let's use the pipe line to pass output
		- `cat words.txt | Select-String st`
			- list of words with the string st
		- `cat words.txt | Select-String st > st_words.txt`
		- `cat st_words.txt`
			- gives us the same results as above
			- `street, blast, last`
	- stderr = standard error
		- i'm going to remove another protected file
		- `rm secure_file`
			- what if we wanted to not see this?
			- redirection operator can work here..
		- `rm secure_file 2> errors.txt`
			- `cat errors.txt`
				- i can see the error output in the txt file!
				- 2 means stdout
					- don't want to put them ina file?
					- can redir it to `$null`
						- special variable that contains nothing
						- specifically for the purposes of nothing
				- `rm secure_file 2>$Null`
					- now output is filled with error messages
				- `Get-help about_redirection`
			- may take a bit of time to get the hang of it,
				- but your command-fu skills will level up