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

# Files

File command gives you info about a file like the header. Files whose extension is changed will still have the same header. Extensions may be important to a software but not to the linux file system. You can have any random extension to a file but it won't fool the OS.

* Wildcards
    * This is a regualr expression. like `.`,`*`etc.  `ls *` 
* touch command - create a file
* mkdir -p bla/abc/poof   : this will create the path structure even if the folders don't exist
* Brace Expansion - 

        mkdir {a,b,c}_{1,2,3}     #a_1,b_1,c_1,a_2,b_2,c_2,a_3,b_3,c_3
        touch {a,b,c,d}_{1..5}/file{1..100}  # creates 100 files in each of the folders 

# Deletion

rm -r  
rmdir

# Copying

    cp source/ile1.txt destination/file2.txt
    cp -r source/ destination/                 # To copy folders
    
# Moving and rename

    mv oldname.txt newname.txt # This will rename the file  
    mv oldfolder/ newfolder/   #  This renames folder
    mv newfolder/* .           # move everything from the newfolder to current directory
    mv newfolder ~/Documents   # move the folder to new location
    mv ~/Documents/newfolder ./jackpot # move the folder and rename it
    
# Edit using Nano

Using nano and a filename will create the file and open a sort of an interface where you have controls in the bottom that are activated using Ctrl+<character present below>. 
    
    ^O - write out
    ^X - exit
    ^R - read file and insert it in the current file
    ^W - where the search string is which is case insensitive( more options like M-C - this means press Alt+C)
    ^\ - Replace
    
The config file for nano is in the following file. you can set spellchecker and other features in here by uncommenting them. Run with sudo

    sudo nano /etc/nanorc


# Search using Locate 

`Locate` command is case sensitive so use -i to make it insensitive. You can also limit the number of results using --limit <num>. It uses a database to get the info and you can try `locate S` to get the info. This is updated only once a day. 

       locate <search string or regex>
       locate -i --limit 10 *.conf
       locate -e *.conf                  # checks if they exist
       locate --follow *.conf            # check symlinks
       
       
Update this database using the `updatedb` command

    touch findme.txt    # creates
    locate findme.txt   # returns nothing
    sudo updatedb       # administrative command
    

# Find

Only typing `find` or `find \\ ` will display everything(files and folders) from that point or location, where locate only shows files. We can control some of the way it searches(depth)

    find . -maxdepth 3                          # it is - even though it is longform
    find . type d                               # show all directories
    find . type f                               # show all files
    find . -name "5.txt"                        # Search every location inside current direcrory for a specific file. String can be regex
    find . -iname "*.tXt"                       # case insensitive
    sudo find / -type f -size +100k             # find files greater than 100 kb
    sudo find / -type f -size +100k -size -5M   # and less than 5 mb. This is an AND condition
    sudo find / -type f -size -100k -o -size -5M   # -o is an OR condition
    sudo find / -type f -size +100k | wc -l        # send the output to wordcount command to see how many
    
Exec option lets you run another command on each of these results

    sudo find / -maxdepth 14 -type f -size +100k -exec cp {} ~/Desktop/copy_here \;  # \; is to end the exec command

create a file called needle in one random folder out of 500,each containing 100 files

    touch haystack/folder$(shuf -i 1-500 -n l)/needle.txt  
    find haystack/ -type f -name "needle.txt" -exec mv {} ~/Desktop/ \;
    
## Viewing files

* `cat` used to concatenate or read files
* `tac` is reverse of cat. It reverses a file/input vertically
* `rev` allows you to reverse each line horizontally
* `less` helps you scroll through line by line. `head` and `tail` let you see the top few or bottom few lines respectively. default is 10 lines. `head -n 2` gives top 2 lines 


## Sorting Data

    sort words.txt                  # Alphabetical order
    sort -r words.txt               # reverse
    sort nums.txt                   # This sorts based on the first digit and not the value of the number
    sort -n nums.txt                # Sort based on number value in ascending
    sort -u num0-9.txt              # Don't show duplicated. Unique
  
Use a key to sort by giving -k  (keydef)

    ls -l /etc |head -n 20| sort -k 5nr  # Sort by the 5th column based on the numeric-value/size in reverse
    
   
# Search within files

`grep` finds/displays all the lines that contain a particular search string and it is case sensitive

     grep e -hello.txt
     grep gadsby gadsby_text.txt
     grep -c e gadsby_text.txt      # Count of the search string matches
     grep -ic "our boys" hello.txt  # ignore case and count occurrences of the string
     grep -cv e gadsby_text.txt     # Number of line that DO NOT have e in them.  v is inVert
     
    ls hello/ |grep hello.txt       # Seach for filename in a folder
    ls -lF / | grep opt             # find all folders with opt in their name in the root directory
   
 # Archive and compress
 
 To create and restore backups. Make a tar ball and then compress it. Creating a tar will take in some extra space than the sum of files and then we compress them.
 
    tar -cvf archive.tar file{1..10}.txt      # create, verbose and f is necessary to pass a file to the tar command and make a .tar file
    tar -tf archive.tar                      # Test label, . this lets you see inside the .tar
    tar -xvf archive.tar                     # X is extract
    
`gzip` is faster and `bzip2` makes it smaller due to more computations and is used for really large files
    
    gzip archive.tar
    gunzip archive.tar.gz
    bzip2 archive.tar
    bunzip2 archive.tar.bz2
    zip archive.zip file1.txt file2.txt file3.txt

Create and compress. For extracting replace c with x

    tar -cvzf archive.tar file{1..100}.txt  # create a .tar.gz file. z is for zip
    tar -cvjf archive.tar file{1..100}.txt  # create a .tar.bz2 file. j is for zip

## Automating workflows using scripts

Create and run a bash script using a text file with .sh extension. This is read by an interpreter that goes line by line in order.

* `#!/bin/bash` is the interpretor and the text file has to start with this to make it a Bourne again shell script
* `#!/usr/bin/python3` for making it a python script

Move your shell scripts into a folder called bin in your home. 

    chmod +x <script_name>
   
Put this in .bashrc 

    PATH="$PATH:$HOME/bin"

Now you can run your scripts from anywhere like a command instead of giving full path to the bash script. Scripts are more powerful than aliases. You can run the bash scripts on a schedule using `cron` jobs. Etymology : Cronos- meaning time.  

* Each user hass a crotab which is a text file that hass a list of scritps that are run accoring to a schedule.
    
        crontab -e      # To edit using an editor. press 1 for nano
    
* Each row will have 6 columns . 5 for scheduling info and the last column is the command or script to be run. Add a new row

        # m h dom mon dow command    minutes hours date_of_month month(JAN/1)  day_of_week(0/SUN)
        20 23 1 JUN 0 echo "Hello"   # Spacing doesn't matter as long as there is a gap. You can use * for the values for every minute
        
        

















