
# Linus operating system file structure assessment and user control

Date of publication: 1/22/2021

I am writing this readme to reinforce and document things I leaned during the University of Pensylvania Cybersecurity Engineer certification training so I can circle back to it later

## File structure

Folders:<br>
- `home` - Contains user info, files: passwd and shadow (shadow contains the hash, passwd doesnt, but both contain usernames that can be hacked. These two files are therefore very important to protect agaist hackers)
- `root` - Its like a user directory that is outside the etc and alows system wide configurations such as preventing all users from accessing facebook.com etc
- `etc` - usernames, imp files shadow and 
- `bin` - It has applications and files for all the commands





## Useful paths

> Getting to the home directory

`cd /` 

> Getting to the user folders

`cd home/`

> Getting to the etc folder

`cd etc` and then `sudo cat shadow` or `sudo less /etc/shadow`
- Important files here are the shadow (most imp as it contains the hashed pw) and the passwd - both containing the usernames
- Below is an extract from a sample shadow file. Notice the hashed pw

`adam:$6$v2/mxc36Kx4HWHuk$41kXVPzWJEZJM3i2.qm0xnZjl2RTQhvIRV2nhYks.vr4c7t0KthtVeneg1ctxDemgdqH1hDKwvbEKAQnYQV/n1:18562:0:99999:7:::`
