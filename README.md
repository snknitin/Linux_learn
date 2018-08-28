# Linux_learn
Mastering the Linux Command Line 


Commands are just case-sensitive text, but when you execute them the meaning of those texts is interpreted by what is called a shell. Terminal is just a window to the shell. Interpretation of these commands depends on shells. Bash shell is most common.

* cal (cal -y or cal 2018)
* date
* clear (Ctrl + L)
* history (!4)  (!! most recent command)

Command Structure :  Command_name options input  
**echo $PATH** gives you all the bin folders separated by : with programs for these commands and it looks for them from left most folder. If it cannot find it in any of the folders it displays command not found. If the same named program is in multiple folders, it picks the one it encounters first from the left in the path. using **which command_name** will show you where to find it.

* options for commands can be short form like -u or long form like --universal. Multiple options can be chained. -a -b -c can be -abc or -bca but long forms cannot be chained.
* 
