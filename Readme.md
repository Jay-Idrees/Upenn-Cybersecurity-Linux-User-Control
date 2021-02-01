
# Linus operating system file structure assessment and user control

Date of publication: 1/22/2021

I am writing this readme to reinforce and document things I leant during the University of Pensylvania Cybersecurity Engineer certification training so I can circle back to it later

## Random useful things
`ctrl-k` will delete the current line in Nano
`cd /home/sysadmin`
`sudo cp /etc/shadow shadow_copy` This command with copy the shadow file from the etc folder to the current directory and then name the copied file shadow_copy
- `sudo` stands for superuser do. It also keeps a log of all the commands used by a particular user

- If you hit tab while partially typing the desired file name then it will display a list of all the filenames in the directory that hace the overlapping name spellings. 

- When you are the root user nothing in the system will stop you- you wont be asked about any password prompts. 

- In a general sense If we talk of one user with super control its the `root` and when we talk about a group of users with super control its the `admin`

- The sudo command in that sense is kind of a user group to which users can be added- It adds another layer of security by prompting the user for a password 

- Linus recognizes the users and user groups by their ids not the name root id is 0, administrative accounts are 1-999 and normal user account ids are usually 1000+

- After using `less` while you are in the middle of viewing the file, if you type `!bash` it will take you to the root directory. This is a trick that hackers use to get into the 

## File structure

- Every file and program on Linux have permissions that can be controlled for a particular user

Folders:<br>
- `/home` - Contains user info, files: passwd and shadow (shadow contains the hash, passwd doesnt, but both contain usernames that can be hacked. These two files are therefore very important to protect agaist hackers)
- `/root` - Its like a user directory that is outside the etc and alows system wide configurations such as preventing all users from accessing facebook.com etc
- `/etc` - usernames, imp files shadow and 
- `/bin` - Contains exe files. It has applications and files for all the commands that are accessible to a user
- `/sbin` - This directory is for adminstrators and contains special programs that should be inaccessible to normal users. These include programs like `adduser` and `passwd`
- `/var`  - This folder contains the system files that change over time such as the log files (`var/log`)
- `/temp` - This folder contains temporary files that get deleted on reboot. Hackers commonly store their malacious files 





## Useful paths

> Getting to the root directory - Contains users

`cd /` 

> Getting to the user folders - Contains a folder for each of the user- This is not the root directory and some commands will need to be run using sudo. Sudo is another command that controls access to other commands for a particular user. For example you can tie certain top level commands to sudo. Those commands will only be able to run when that particular user writes sudo before the command and maybe prompted to give a password. What pseudo does is temporarily invoke 'root' level privilidges for a particular selected command. 

`cd home/`

> Getting to the etc folder

`cd etc` and then `sudo cat shadow` or `sudo less /etc/shadow`. Similarly you can also visit `sudo less /etc/passwd`
- Important files here are the shadow (most imp as it contains the hashed pw) and the passwd - both contain the usernames
- Below is an extract from a sample shadow file. Notice the hashed pw

`adam:$6$v2/mxc36Kx4HWHuk$41kXVPzWJEZJM3i2.qm0xnZjl2RTQhvIRV2nhYks.vr4c7t0KthtVeneg1ctxDemgdqH1hDKwvbEKAQnYQV/n1:18562:0:99999:7:::`

- If a row in `/etc/shadow` contains a `!` instead of a password hash, means that the account is locked

- If a row in `/etc/shadow` contains a `*` instead of a password hash, that the user is not allowed to login.
  - This is usually implemented on a user reserved for the system, and not a _human_ user.

  > Checking Logs

  `cd var/log`

- Some important files to keep in mind ` cd var/log/ufw.log` which stores attempts by the user to visit unauthorized websites and `cd var/log/auth.log` which stores infomation regarding failed login attempts to login as root

# Managing the Processes in Linux

Three commands to keep in mind:
-  `top` for generating the list of active processess and obtaining an overview of memory consumption. Note that if you type `x` it will display actie processes in the order of CPU consumption
-  `ps`  To look at specific processes that are running
-  `kill` To end processes 

- After running these commands you can come back to the terminal by typing `q` 

**Dynamic Analysis** is when we run a malacious script in a controlled environment such as a virtural machine to assess 
- Note that when you use `top` the PID is the process id that is used to refer to the process when you want to kill it. 

- Example commands:

`ps aux | grep bash` or `ps aux | less`

- `a` displays all the processes involving the terminal
- `u` specifies the username that started the process
- `x` displays all the processes that DO NOT involve terminal - so toghether with a and x you can look at all the processes there are
- `grep bash` here is further tailoriing to display only the shortlist of the processes called name

- Note that in order to kill a process it must be referred by its PID or process id. 

- Killing a process called stress

`sudo killall stress` 

This effectively kills all the processes and the sub-processes that were initiated by theat process. 



## Installing packages

Linux has its own command for installing packages like in npm instal for node packages linux packages can be installed with

`sudo apt -y install package_name another_packagname`

- Note how multiple packages can be installed at the same time and `-y` lets you atromatically answer yes to all the question prompts before installation. 

- Examples of the package names: 

`emacs` is a files editor

## John the Ripper for password cracking

- The software accepts username:hash info that can be provided in the form of a shell script. If we just copy paste from the shadow will - the whole row associated with a particular user information. Below are some steps in the sequence

1. Copy the shadow file `sudo cp /etc/shadow shadow_copy` typically only the root user has access to the shadow file

2. Delete the irrelevent hashes/usernames  (Ctrl+k to delete the entire line in the nano editor)

3. Crack the password `sudo john shadow_copy` where shadow_copy is the name of the file- If you make a copy from the rot directory it will carry the same set of permisions and therefore we are using sudo. 

- Note that the length of the password is more important security wise 16 characters is very secure 
 8 characters is cracked instantly. 12 in 4 weeks, 16 in 35 thousand years


 ## The Magical `sudo` and its Sisters in Linux

 Permits application of the principal of least privilige by limiting the control of a user to only specific commands and allowing temporary escalation to the root privilidge only for the time when a command is run. In other words it will only alow the user to complete actions that he/she has access to. Sudo also keeps a log of all the commands used by the user 

 **Related commands**
 - `whoami` Reveals the current username when this command is typed in the terminal
 - `su`  Switching users
 - `sudoers` file containing the list of users and descriptions of their access - must be used with cat, less etc 
 - `chmod` Changing the permissions RWX (4,3,1)
 - `chown` Asigns uername to user folder- Changing the owner and group for a file
 - `exit` exits the user and takes you to the default user sysadmin 
 - `su -l` lists all the commands the exisiting user has access to

> Switching to the root directory so you can run root level commands. This command must be used to switch to the root directory before you can make changes to the user access. 

`sudo su` 
`root@UbuntuDesktop:/etc#` This is what the terminal prompt will look like 

> Checking which user you are currently to confirm that you are now in the root

`whoami`

- you can test if whether you have sudo priviidges by trying to install package updates

`apt update`

> Inspecting the sudoers file. This file contains all the users and groups and the details regarding their access limits

- Note that you have to be in the root folder and then navigate to etc where the sudoers file is located. It can be opened with the following command when you are inside the etc folder
`cat sudoers`  This file looks something like this

```
# User privilege specification
root	ALL=(ALL:ALL) ALL
vagrant ALL=(ALL:ALL) NOPASSWD:ALL
# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo	ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "#include" directives:

#includedir /etc/sudoers.d
max  ALL=(ALL:ALL) /usr/bin/less
```
- [`USER` or %`GROUP`]  `HOST`=(`USER`:`GROUP`) `COMMAND` this is the general format for permissions in sudoers. Also note that host is the machine itself which the admin can control e-g if there is a need to limit access to specific files on some machines

## Adding a user to the sudo group

`sudo usermod -aG sudo sally` aG asign group. Note that the sudo privilidges are also configured in the sudoers file

## Admin CRUD functions: CREAT READ UPDATE AND DELETE functions in Linux

**Creating a user or usergroup**


 - Creating a new username - it will prompt for password
`sudo adduser joseph`

`groups joseph` Displays all the groups this username has been assigned to

- Creating a new group
`sudo addgroup developers` If you are in the root directory already then you do not need to use sudo

- Creating a folder by the new username in the home directory e-g mike

`mkdir /home/mike`

- Giving the mike username access to the mike folder

`chown -R mike: /home/mike`
 Here R is the recursive flag which ensures that all the files in the user's home folder as well 






**Reading existing user excess**

-In order to determine which user you are currently, you can navigate to the etc directory and open the `sudorers` file
-When you are in the etc directory, type

- `cat sudoers` Here you can also use `more, less, tail` instead of `cat` as you prefer

- you can also use `ls /home` to list all the user folders

- `cat /etc/sudoers` If you are in the root directory then you can directly type 

**Modifying/Updating user by Giving existing user access to a new command**

- `sudoers` file can be edited by typing `visudo` in the terminal - This command is like the nano editor for sudoers file - just the visudo is sufficient- no need to add sudoers or file name

- `sudo -lU usename` for listing commands to which the user has access to. Or just type `sudo -l` to list for the current user

- open the sudoers file and paste something like below
`john  ALL=(ALL:ALL) /usr/bin/apt` This gives John access to the apt command 

- To exit the current user type `exit` then you will be taken to the root directory

- type `visudo` to edit sudoers

- Now check to see if the command can be searched for the max user 
`grep apt etc/sudoers` This would display the entire line of text from sudoers and you can see the max username. This is an important trick as essentially you can search for a specific comand in the sudoers to see which users have access to it

- Then re-test by running `su max` and then `sudo apt update` - Note that using sudo here is important



> **Updating/Controling Permissions for the files**

- You can inspect permissions for a folder or a file with `ls -l`

- You will see permissions organized in 3 groups (rwx- read write execute, if - then it means that particular permission is absent or not allowed)

`drwxr-xr-x 2 sysadmin sysadmin  4096 Jan 31 17:57 example-dir`

- Permissions are arranged in the order of `owner-group-otherusers`

- Above is the sample display of permissions folders that is created d= directory. 
- Note that there are 9 permissions spots controling `rwxrwxrwx` The first 3 represents the permissions for the file `owner`, next 3 `group` and the last 3 for other `users`
- You can also control the function by numbers: `1-execute` `2-write` `4-read` `- no permission,0` this assignment is fixed so that you can even use it in the commands
- `4` is just read
- `7` all 3, read(4), write(2) execute(1)
- `6` read, write
- `5` read,execute


**Deleting a user or usergroup**

> `deluser` for Deleting a user 

 `sudo deluser --remove-home mike`

 - Here --remove-home refers to removing the folder of the username mkike from the home directory


## `usermod` for Assigning groups to the users and locking an existing user

>  Assigning groups to to username using `-G` Note the capital letters

`sudo usermod -G mike mike`  Note that this is an interesting command. Here I have a username mike and I am assignining it to the group named mike as well. 

> Locking `-L`

`sudo usermod -G mike mike`

## Acces control- This opertes at the file level controling access to the functions of READ, WRITE, EXECUTE

- You can look at the file permissions by navigating to the folder and then typing:

`ls -l` 
Below is what it shows <br>
                                size date+time of last modification and then file name
- `-rw-r--r-- 1 sysadmin sysadmin 1489 Jan 20 16:22 0310_Dealer_schedule`

- r=read, w=write and then x=execute(not applicable to the file above), the sysadmin sysadmin are the username and the usergroup, then its folowed by the file size, date and time and the size of the file  

-



