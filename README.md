# 42-Born2beRoot

**Table of contents**


## Installing Debian on a virtual machine (VM)
Download [debian](https://www.debian.org/) ISO (International Organization for Standardization) image

VirtualBox:
* Create New
* Choose Name
* Choose folder in sgoinfre - to have enough disk space
* Select image
* Skip Unattended Installation - as we want to learn manual configuration

* Choose 2048MB RAM, 1 CPU, Create virtual hard disk with 30.00GB

Start VM:
* Install
* Select language, country, keyboard
* Set hostname to denizozd42 - as required by subject
* Leave domain naim empty - as not required by subject 
* Set root password (MyFirstVM@42)
* Set username to denizozd and password (AnotherPW@42)
* If you do not want to do the Bonus select 'Guided - use entire disk and set up encrypted LVM'
* Separate /home, /var, and /tmp partitions
* Cancel - as erasing of data is not required
* Set  partition password (MyFirstVM@42)
* Finish partitioning and write changes to disk

Configure the package manager:
Scan extra installation media? No
Select country
Select deb.debian.org
HTTP proxy information: Empty and Continue
Configuring popularity-contest
Participate in the package usage survey? No
Software selection
Disable all options of software and Continue

Software selection:
Disable all options of software and Continue
Install the GRUB boot loader
Install the GRUB boot loader to your primary drive: Yes
Device for boor loader installation: select /dev/sda

Install the GRUB boot loader the GRUB boot loader to your primary drive: Yes
Device for boor loader installation: select /dev/sda

## Setup sudo, user and group
su - change to admin user
apt install sudo - sudo installation
sudo reboot - reboot
sudo adduser <user> - add user
sudo adduser lbordona
sudo addgroup <group_name> - create group
sudo addgroup user42
getent group user42 - view users inside group user42
sudo adduser <user> <group> - add user to group
sudo adduser denizozd user42
sudo adduser denizozd sudo

## Install and configure Secure Shell (SSH)
sudo apt update - refresh repositories
sudo apt install openssh-server - ssh server installation
sudo service ssh status - verify ssh status "Active"
sudo apt install vim - install vim as its not preinstalled
sudo vim /etc/ssh/sshd_config - set parameters
Rewrite "#Port 22" to "Port 4242" - enable Port 4242
Rewrite "#PermitRootLogin prohibit-password" to "PermitRootLogin no" - disable root access to ssh
sudo vim /etc/ssh/ssh_config - set parameters
Rewrite "#	Port 22" to "Port 4242" - enable Port 4242
sudo service ssh restart - restart ssh services
sudo service ssh status - verify ssh status (ports 4242 mentioned at bottom)

## Connecting SSH
Shutdown VM
Oracle VirtualBox:
Settings > Network > Advanced > Port Forwarding > New Rule
Host Port = 2222
Guest Port = 4242
Start VM
Switch to terminal on local machine
To connect via ssh from the machine to the virtual machine use the command 'ssh denizozd@localhost -p 2222'; it will ask for the password you are trying to log in. Once the password is introduced it will show denizozd@denizozd42:~$ in green which means that the connections has been successfull.

## Installing Uncomplicated Firewall (UFW)
UFW =  [firewall](https://en.wikipedia.org/wiki/Firewall_(computing)) which uses the command line for setting up [iptables](https://en.wikipedia.org/wiki/Iptables)
sudo apt update - refresh repositories
sudo apt install ufw - UFW installation
sudo ufw enable - enable UFW service
sudo ufw allow 4242 - allow Port 4242 for firewall
sudo ufw status - check firewall rules and status

## Setting up sudo password policy
touch /etc/sudoers.d/sudo_config - create sudo_config file for sudo password config
mkdir /var/log/sudo - create folder for sudo logs
vim /etc/sudoers.d/sudo_config - edit the sudo_config file with rules:

Defaults  passwd_tries=3 //max number of password tries
Defaults  badpass_message="Error :-/" //error password message
Defaults  logfile="/var/log/sudo/sudo_config" //log file
Defaults  log_input, log_output
Defaults  iolog_dir="/var/log/sudo/logfile" //log file diretory
Defaults  requiretty //TTY mode enabled: user must have real terminal to run commands with sudo
Defaults  secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin" //restricts the directories in which sudo will search for executables

## Setting up strong password policy
vim /etc/login.defs - change parameters of login.defs file
PASS_MAX_DAYS 30 - set password expiration to 30 days
PASS_MIN_DAYS 2 - set minimum days to change password again
PASS_WARN_AGE 7 - set 7 days to warn password expiration date
sudo apt install libpam-pwquality - installation of libpam-pwquality
vim /etc/pam.d/common-password - modify common password policies
minlen=10 lcredit=-1 ucredit=-1 dcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root - insert after retry=3
minlen=10 //minimum number of characters
lcredit=-1 //minimum one lowercase character
ucredit=-1 //minimum one uppercase character
dcredit=-1 //minimum one number
maxrepeat=3 //maximum number of same character repeated
reject_username //reject username in password
difok=7 //minimum 7 characters different from the last password
enforce_for_root //add the rule to root user too

## Scripting monitoring.sh
su //change to sudo


## Evaluating this project
write summary of information necessary, commands, grpahs, etc.

## Sources
[Born2beroot-Tutorial (with Screenshots)](https://github.com/gemartin99/Born2beroot-Tutorial/blob/main/README_EN.md#1--download-the-virtual-machine-iso-)

[Another Born2beroot Tutorial](https://github.com/lbordonal/01-Born2beroot/wiki#mandatory-part)

[Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#headers)

