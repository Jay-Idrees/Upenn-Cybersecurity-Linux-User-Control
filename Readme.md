
# Linus operating system file structure assessment and user control

Date of publication: 1/22/2021

I am writing this readme to reinforce and document things I leant during the University of Pensylvania Cybersecurity Engineer certification training so I can circle back to it later

## Random useful things
`ctrl-k` will delete the current line in Nano
`cd /home/sysadmin`
`sudo cp /etc/shadow shadow_copy` This command with copy the shadow file from the etc folder to the current directory and then name the copied file shadow_copy
- `sudo` stands for superuser do. It also keeps a log of all the commands used by a particular user

- If you hit tab while partially typing the desired file name then it will display a list of all the filenames in the directory that hace the overlapping name spellings. 

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


 ## Magic of the sudo command in linux

