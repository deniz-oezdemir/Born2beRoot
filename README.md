# 42-Born2beRoot

**Table of contents**


## Downloading Debian
Download [debian](https://www.debian.org/) ISO (International Organization for Standardization) image

## Creating virtual machine in VirtualBox
* Create New
* Choose Name
* Choose folder in sgoinfre - to have enough disk space
* Select image
* Skip Unattended Installation - as we want to learn manual configuration
* Choose 2048MB RAM, 1 CPU, Create virtual hard disk with 30.00GB
> A virtual machine (VM) is a software emulation of a physical computer. It allows you to run multiple operating systems on a single physical machine.
In our case VirtualBox acts as a host hypervisor as described in the following scheme:
![alt text](https://github.com/deniz-oezdemir/42-Born2beRoot/blob/main/virtual_machines-h.png)

## Starting VM
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

## Configuring the package manager
* Scan extra installation media? No
* Select country
* Select deb.debian.org
* HTTP proxy information: Empty and Continue
* Configuring popularity-contest
* Participate in the package usage survey? No
* Software selection
* Disable all options of software and Continue

## Selecting software
* Disable all options of software and Continue
* Install the GRUB boot loader
* Install the GRUB boot loader to your primary drive: Yes
* Device for boot loader installation: select /dev/sda

* Install the GRUB boot loader the GRUB boot loader to your primary drive: Yes
* Device for boot loader installation: select /dev/sda

## Setting up sudo, user and group
* `su` - change to admin user
* `apt install sudo` - sudo installation
* `sudo reboot` - reboot
* `sudo adduser <user>` - add user
* `sudo adduser denizozd`
* `sudo addgroup <group_name>` - create group
* `sudo addgroup user42`
* `getent group user42` - view users inside group user42
* `sudo adduser <user> <group>` - add user to group
* `sudo adduser denizozd user42`
* `sudo adduser denizozd sudo`

## Installing and configuring Secure Shell (SSH)
* `sudo apt update` - refresh repositories
* `sudo apt install openssh-server` - ssh server installation
* `sudo service ssh status` - verify ssh status "Active"
* `sudo apt install vim` - install vim as its not preinstalled
* `sudo vim /etc/ssh/sshd_config` - set parameters
* Rewrite `#Port 22` to `Port 4242` - enable Port 4242
* Rewrite `#PermitRootLogin prohibit-password` to `PermitRootLogin no` - disable root access to ssh
* `sudo vim /etc/ssh/ssh_config` - set parameters
* Rewrite `#	Port 22` to `Port 4242` - enable Port 4242
* `sudo service ssh restart` - restart ssh services
* `sudo service ssh status` - verify ssh status (ports 4242 mentioned at bottom)

## Connecting SSH
* Shutdown VM
* Oracle VirtualBox:
* Settings > Network > Advanced > Port Forwarding > New Rule
* Host Port = 2222
* Guest Port = 4242
* Start VM
* Switch to terminal on local machine
* To connect via ssh from the local machine to the virtual machine use the command 'ssh denizozd@localhost -p 2222'; it will ask for the password you are trying to log in. Once the password is introduced it will show denizozd@denizozd42:~$ in green which means that the connections has been successfull.

## Installing Uncomplicated Firewall (UFW)
* UFW =  [firewall](https://en.wikipedia.org/wiki/Firewall_(computing)) which uses the command line for setting up [iptables](https://en.wikipedia.org/wiki/Iptables)
* `sudo apt install ufw` - UFW installation
* `sudo ufw enable` - enable UFW service
* `sudo ufw allow 4242` - allow Port 4242 for firewall
* `sudo ufw status` - check firewall rules and status

## Setting up sudo password policy
* `touch /etc/sudoers.d/sudo_config` - create sudo_config file for sudo password config
* `mkdir /var/log/sudo` - create folder for sudo logs
* `vim /etc/sudoers.d/sudo_config` - edit the sudo_config file with rules:
```
Defaults  passwd_tries=3 //max number of password tries
Defaults  badpass_message="Error :-/" //error password message
Defaults  logfile="/var/log/sudo/sudo_config" //log file
Defaults  log_input, log_output
Defaults  iolog_dir="/var/log/sudo/logfile" //log file diretory
Defaults  requiretty //TTY mode enabled: user must have real terminal to run commands with sudo
Defaults  secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin" //restricts the directories in which sudo will search for executables
```

## Setting up strong password policy
`vim /etc/login.defs` - change parameters of login.defs file
```
PASS_MAX_DAYS 30 - set password expiration to 30 days
PASS_MIN_DAYS 2 - set minimum days to change password again
PASS_WARN_AGE 7 - set 7 days to warn password expiration date
```

`sudo apt install libpam-pwquality` - installation of libpam-pwquality
`vim /etc/pam.d/common-password` - modify common password policies
`minlen=10 lcredit=-1 ucredit=-1 dcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root` - insert after retry=3
```
minlen=10 //minimum number of characters
lcredit=-1 //minimum one lowercase character
ucredit=-1 //minimum one uppercase character
dcredit=-1 //minimum one number
maxrepeat=3 //maximum number of same character repeated
reject_username //reject username in password
difok=7 //minimum 7 characters different from the last password
enforce_for_root //add the rule to root user too
```

## Scripting monitoring.sh
* `su` //change to sudo
* write own script based on the following script from sources:

```
# Get system architecture
arch=$(uname -a)

# Count the number of physical CPUs
cpuf=$(grep "physical id" /proc/cpuinfo | wc -l)

# Count the number of virtual CPUs
cpuv=$(grep "processor" /proc/cpuinfo | wc -l)

# Get total, used, and percentage of RAM
ram_total=$(free --mega | awk '$1 == "Mem:" {print $2}')
ram_use=$(free --mega | awk '$1 == "Mem:" {print $3}')
ram_percent=$(free --mega | awk '$1 == "Mem:" {printf("%.2f"), $3/$2*100}')

# Get total, used, and percentage of disk space
disk_total=$(df -m | grep "/dev/" | grep -v "/boot" | awk '{disk_t += $2} END {printf ("%.1fGb\n"), disk_t/1024}')
disk_use=$(df -m | grep "/dev/" | grep -v "/boot" | awk '{disk_u += $3} END {print disk_u}')
disk_percent=$(df -m | grep "/dev/" | grep -v "/boot" | awk '{disk_u += $3} {disk_t+= $2} END {printf("%d"), disk_u/disk_t*100}')

# Get CPU load percentage
cpul=$(vmstat 1 2 | tail -1 | awk '{printf $15}')
cpu_op=$(expr 100 - $cpul)
cpu_fin=$(printf "%.1f" $cpu_op)

# Get last system boot time
lb=$(who -b | awk '$1 == "system" {print $3 " " $4}')

# Check if LVM (Logical Volume Manager) is in use
lvmu=$(if [ $(lsblk | grep "lvm" | wc -l) -gt 0 ]; then echo yes; else echo no; fi)

# Count established TCP connections
tcpc=$(ss -ta | grep ESTAB | wc -l)

# Count the number of logged-in users
ulog=$(users | wc -w)

# Get network information (IP address and MAC address)
ip=$(hostname -I)
mac=$(ip link | grep "link/ether" | awk '{print $2}')

# Count the number of sudo commands
cmnd=$(journalctl _COMM=sudo | grep COMMAND | wc -l)

# Display system information using the wall command
wall "	Architecture: $arch
	CPU physical: $cpuf
	vCPU: $cpuv
	Memory Usage: $ram_use/${ram_total}MB ($ram_percent%)
	Disk Usage: $disk_use/${disk_total} ($disk_percent%)
	CPU load: $cpu_fin%
	Last boot: $lb
	LVM use: $lvmu
	Connections TCP: $tcpc ESTABLISHED
	User log: $ulog
	Network: IP $ip ($mac)
	Sudo: $cmnd cmd"
```

## Setting Crontab
* is a background process manager that can execute specified processes
* `sudo crontab -u root -e` - to configure crontab
* `*/10 * * * * sh /ruta del script` - add for above script to execute every 10 minutes

## Creating Signature
* Shut down VM
* locate path of VM on disk
* run shasum Born2beRoot.vdi

## Evaluating this project
write summary of information necessary, commands, grpahs, etc.

## Sources
[Born2beroot-Tutorial (with Screenshots)](https://github.com/gemartin99/Born2beroot-Tutorial/blob/main/README_EN.md#1--download-the-virtual-machine-iso-)

[Another Born2beroot Tutorial](https://github.com/lbordonal/01-Born2beroot/wiki#mandatory-part)

[Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#headers)

