# 42-Born2beRoot

**Table of contents**


## Installing Debian on a virtual machine (VM)
Download [debian](https://www.debian.org/) ISO (International Organization for Standardization) image

VirtualBox:
Create New
Choose Name
Choose folder in sgoinfre - to have enough disk space
Select image
Skip Unattended Installation - as we want to learn manual configuration

2048MB RAM
1 CPU
Create virtual hard disk with 30.00GB

Start VM:
Install
Select language, country, keyboard
Set hostname to denizozd42 - as required by subject
Leave domain naim empty - as not required by subject 
Set root password (MyFirstVM@42)
Set username to denizozd and password (AnotherPW@42)
If you do not want to do the Bonus select 'Guided - use entire disk and set up encrypted LVM'
Separate /home, /var, and /tmp partitions
Cancel - as erasing of data is not required
Set  partition password (MyFirstVM@42)
Finish partitioning and write changes to disk

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

## 6. SSH Connection / 4.3 Installing & configuring UFW 🔥🧱

## Evaluating this project
write summary of information necessary, commands, grpahs, etc.

## Sources
[Born2beroot-Tutorial (with Screenshots)](https://github.com/gemartin99/Born2beroot-Tutorial/blob/main/README_EN.md#1--download-the-virtual-machine-iso-)

[Another Born2beroot Tutorial](https://github.com/lbordonal/01-Born2beroot/wiki#mandatory-part)

[Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#headers)

