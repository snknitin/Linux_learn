# Linux_learn
Mastering the Linux Command Line 


Commands are just case-sensitive text, but when you execute them the meaning of those texts is interpreted by what is called a shell. Terminal is just a window to the shell. Interpretation of these commands depends on shells. Bash shell is most common.

* cal (cal -y or cal 2018)
* date
* clear (Ctrl + L)
* history (!4)  (!! most recent command)
* cut < file.txt --delimiter " " --fields 1

Command Structure :  **Command_name** -options input  
**echo $PATH** gives you all the bin folders separated by : with programs for these commands and it looks for them from left most folder. If it cannot find it in any of the folders it displays command not found. If the same named program is in multiple folders, it picks the one it encounters first from the left in the path. using **which command_name** will show you where to find it.

* options for commands can be short form like -u or long form like --universal. Multiple options can be chained. -a -b -c can be -abc or -bca but long forms cannot be chained.

# Using the Manual


|Section|		Contains|Description|
|------|-------------|-------------|
|1*|User Commands|Commands that can be run from the shell by a normal user (typically no administrative privileges are needed)|
|2|System Calls|Programming functions used to make calls to the Linux kernel|
|3|C Library Functions|Programming functions that provide interfaces to specific programming libraries|
|4|Devices and Special files|File system nodes that represent hardware devices or software devices.|
|5*|File formats and conventions|The structure and format of file types or specific configuration files.|
|6|Games|Games available on the system|
|7|Misc|Overviews of miscellaneous topics such as protocols, filesystems and so on|
|8*|Sys admin|Commands that require root or other administrative privileges to use|


    man -k which  # k is for search and gives section number
    man section_num which  # section_num =1 for user commands and can be ignored
    
|Section|		Meaning|Example|
|------|-------------|-------------|
|\[THING\]| Optional| which [-a] |
| \<THING\> |Mandatory| |
|THING ...| can run multiple repeatedly |which date cal|
|THING1 |Use THING1 OR THING2. Not Both.| |
|THING in italics| Replace THING with whatever is appropriate.| 
    
If you don't find a man page you can try the help command.

# Command I/O pipelines

Inputs : STDIN(0) and arguments  
Outputs : STDOUT(1) and STDERR (2)  

* Redirection 

    * cat 1> output.txt -  writing or redirecting stream 1 ie., standard output data stream to output file.  1 can be ignored but there is no space between 1 and >. This will remove everything in the output.txt and write the current stream (Truncation). Use cat >> output.txt to append
    * cat 2>>error.txt to write the error
    * cat 1>>output.txt 2>>error.txt
    * cat 0< input.txt to read from input
    * cat 0< input.txt 1>>output.txt to copy one file content to another
    * You can also pass data from one terminal to another as terminal is also treated as a file. Get the location using **tty**

* Pipeline
    * command1 **|** command2 - stdout of command1 is stdin of command2
    * **Tee** and **xargs**
        * Tee is like a t-junction for stdout from the first command to go down in two different paths. Eg : stdout1 goes to stdout_random(like a file) and stdin_command2. Uou can save your data and still keep your pipeline flowing
        
                ls - l | tee file.txt | less
        * xargs - Certain functions like echo don't take in stdin. They only use command line arguments. So we need to convert the stdin into command line arguments. We pipe stdout of command 1 into xargs
        
                date | cut --delimiter " " --fields=1 | xargs echo
                cat filestodelete.txt | xargs rm

# Aliases

Nicknames for commands or command pipelines to use them easily. First thing to do is create a file called **.bash_aliases** . The fullstop makes it a hidden file. 

* alias nickname = 'command or command pipeline'


# Directory Structure

Linux follows a tree structure. The root is / which has the following sub folders :

* bin - Stores common linux user common binaries like date , cal etc
* boot - Bootable kernel and config
* dev - represents devices
    * tty = terminal
    * ram =RAM
    * cd = cdROM
    * fd = floppy disk, sd= hd = hard disk
* etc - Administrative config files
* home -  users and their folders 
* media -
* lib - shared libraries needed by applications in bin and sbin
* mnt - moun external devices, superseded by media
* misc - directory used to automount filesystems on request
* opt - additional or optional softwares are stored here
* proc - Info about the system resources
* root - home folder for the root user
* sbin - administrative commands(binaries) for super user
* tmp - temporary files used by running apps
* usr - contains files pertaining to user that in theory don't change after installation
* var - contains directories of variable data like system log files that could be used by various applications

# Navigating the File system

~  means path to the current directory or rather the home directory. pwd means present working directory.

# File extensions

File command gives you info about a file like the header. Files whose extension is changed will still have the same header. Extensions may be important to a software but not to the linux file system
