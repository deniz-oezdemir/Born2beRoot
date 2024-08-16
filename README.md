# Born2beRoot

## Downloading Debian
Download [debian](https://www.debian.org/) ISO (International Organization for Standardization) image
> The subject recommends debian if you are new to SysAdmin

| Feature                   | Rocky Linux                                 | Debian                                      |
|---------------------------|--------------------------------------------|---------------------------------------------|
| **Origins**               | Community-supported RHEL-compatible OS     | Volunteer-driven, community-based OS        |
| **Release Cycle**         | Fixed, periodic releases with LTS options   | "When it's ready" release cycle             |
| **Package Management**    | RPM (Red Hat Package Manager)               | DPkg and APT (Debian Package Management)    |
| **Community and Support** | Community-driven support, forums, docs     | Large and active community, extensive docs  |
| **Philosophy**            | Stability, RHEL compatibility               | Free software principles, community-driven  |

Rocky Linux Pros:
* Predictable Release Cycle
* Enterprise Features

Debian Pros:
* Massive Package Repository
* Commitment to Free Software
* Diverse Architectures


## Creating virtual machine in VirtualBox
* Create New
* Choose Name
* Choose folder in sgoinfre - to have enough disk space
* Select image
* Skip Unattended Installation - as we want to learn manual configuration
* Choose 2048MB RAM, 1 CPU, Create virtual hard disk with 30.00GB
> A virtual machine (VM) is a software emulation of a physical computer. It allows you to run multiple operating systems on a single physical machine.
> In our case VirtualBox acts as a host hypervisor as described in the following scheme:
![alt text](https://github.com/deniz-oezdemir/42-Born2beRoot/blob/main/virtual_machines-h.png)
> Advantages of VM:
> * Resource Optimization: enables the efficient use of physical hardware by allowing multiple virtual servers to run on a single physical server
> * Isolation and Security: isolation of OS enhances security by preventing one VM from impacting others
> * Testing and Development: Developers can create and deploy VMs to replicate different environments, allowing for thorough testing without affecting the host system

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
> Logical Volume Manager (LVM) is a system for managing logical volumes or filesystems that reside on a hard drive or storage array. It provides a layer of abstraction between the operating system and the physical storage devices. LVM allows you to create, resize, move, and manage storage volumes dynamically, without the need to shut down the system.

## Configuring the package manager
* Scan extra installation media? No
* Select country
* Select deb.debian.org
* HTTP proxy information: Empty and Continue
* Configuring popularity-contest
* Participate in the package usage survey? No
* Software selection
* Disable all options of software and Continue

> Two package management tools in Debian-based systems:

> `aptitude` is a high-level package management tool that is more than just a package installer; it is also a package manager with features for searching, browsing, and managing packages.It can handle complex dependency resolutions and recommends solutions to conflicts. While it's powerful, `aptitude` is not as commonly used as `apt` in recent Debian-based systems.

> `apt` (Advanced Package Tool) is a low-level package manager. `apt` is designed to be user-friendly and efficient for common package management tasks, such as installing, removing, updating, and upgrading packages. It is widely used and *recommended for most day-to-day package management* operations.

> AppArmor is a Linux security module that enhances system security by confining the capabilities of applications through the enforcement of profiles, which define and restrict their access to resources on a Linux-based system.


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

> Using `sudo` (Superuser Do):
>
> Elevated Privileges: The sudo command allows a permitted user to execute a command with superuser privileges or as another user, as specified by the security policy. This enables users to perform administrative tasks that require elevated access.
>
> Security: By requiring users to authenticate before executing privileged commands, sudo enhances security. It helps prevent unauthorized access to critical system functions and ensures accountability for administrative actions.
>
> Example of sudo Operation:
>
> Suppose you want to update the package information on a Linux system using the apt package manager, a task that typically requires administrative privileges: `sudo apt update`.

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

> To connect via ssh from the local machine to the virtual machine use the command `ssh denizozd@localhost -p 2222`; it will ask for the password you are trying to log in. Once the password is introduced it will show denizozd@denizozd42:~$ in green which means that the connections has been successfull.

> SSH, or Secure Shell, is a cryptographic network protocol used for secure communication over an unsecured network. It provides a secure channel between two devices, typically a client and a server, allowing for secure data exchange, remote command execution, and other network services.

## Installing Uncomplicated Firewall (UFW)
* UFW =  [firewall](https://en.wikipedia.org/wiki/Firewall_(computing)) which uses the command line for setting up [iptables](https://en.wikipedia.org/wiki/Iptables)
* `sudo apt install ufw` - UFW installation
* `sudo ufw enable` - enable UFW service
* `sudo ufw allow 4242` - allow Port 4242 for firewall
* `sudo ufw status` - check firewall rules and status
> UFW is a user-friendly front-end for managing iptables, the default firewall management tool for many Linux distributions.

## Setting up sudo password policy
* `touch /etc/sudoers.d/sudo_config` - create sudo_config file for sudo password config
* `mkdir /var/log/sudo` - create folder for sudo logs
* `vim /etc/sudoers.d/sudo_config` - edit the sudo_config file with rules:
```
Defaults  passwd_tries=3 //max number of password tries
Defaults  badpass_message="Error :-/" //error password message
Defaults  logfile="/var/log/sudo/sudo_config" //log file
Defaults  log_input, log_output
Defaults  iolog_dir="/var/log/sudo" //log file diretory
Defaults  requiretty //TTY mode enabled: user must have real terminal to run commands with sudo
Defaults  secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin" //restricts the directories in which sudo will search for executables
```
> Pros:
> * Increased Security: Enforce strong password complexity to resist brute-force attacks.
>
> * Regular Password Changes: Set passwords to expire every 30 days, reducing the risk of long-term compromise.
>
> * User Awareness: Provide a 7-day warning period, encouraging users to proactively change passwords.
>
> * Password History: Ensure new passwords differ by at least 7 characters, preventing reuse.
>
> Cons:
> * User Convenience: Frequent changes may lead to user frustration and weaker passwords.
>
> * Potential for Lockouts: Increased risk of forgotten passwords with complex requirements.
>
> * Resource Utilization: Assess potential computational overhead from additional password checks.
>
> * Security Trade-offs: Balance usability and security considerations, especially with the maxrepeat setting.

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

- uname -a: This command is used to display system information. The -a option stands for "all," and it prints all available information about the system.
- $(...): Command substitution. The result of uname -a is stored in the variable arch.

# Count the number of physical CPUs
cpuf=$(grep "physical id" /proc/cpuinfo | wc -l)

- wc -l: Counts the number of lines.
- grep see below

# Count the number of virtual CPUs
cpuv=$(grep "processor" /proc/cpuinfo | wc -l)

# Get total, used, and percentage of RAM
ram_total=$(free --mega | awk '$1 == "Mem:" {print $2}')
ram_use=$(free --mega | awk '$1 == "Mem:" {print $3}')
ram_percent=$(free --mega | awk '$1 == "Mem:" {printf("%.2f"), $3/$2*100}')

- free --mega: Displays memory usage in megabytes.
- awk see below

# Get total, used, and percentage of disk space
disk_total=$(df -m | grep "/dev/" | grep -v "/boot" | awk '{disk_t += $2} END {printf ("%.1fGb\n"), disk_t/1024}')
disk_use=$(df -m | grep "/dev/" | grep -v "/boot" | awk '{disk_u += $3} END {print disk_u}')
disk_percent=$(df -m | grep "/dev/" | grep -v "/boot" | awk '{disk_u += $3} {disk_t+= $2} END {printf("%d"), disk_u/disk_t*100}')

- df -m: Displays disk space usage in megabytes.

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

> `grep` is a command-line utility used for searching text within files. It stands for "Global Regular Expression Print." Here's a brief explanation of how grep works:
>
> Basic Syntax: `grep [options] pattern [file...]`
>
> Parameters:
> * options: Various flags and options that modify the behavior of grep.
> * pattern: The regular expression or plain text string to search for.
> * file: The file or files in which to search. If not specified, grep reads from standard input.
>
> Working Principle: grep reads lines from the specified file(s) or standard input. It searches for the specified pattern in each line. If a line contains the pattern, it is printed to the standard output.
>
> Example: `grep "example" file.txt ` This command searches for the string "example" in the file file.txt and prints lines containing that string.

> `awk` - A Powerful Text Processing Tool
>
> Basic Syntax: `awk 'pattern { action }' file`
> * pattern: A condition that, if true, triggers the associated action.
> * action: A set of commands to be executed when the pattern is true.
> * file: The input file(s) to process. If not specified, awk reads from standard input.
>
> Working Principle:
> * awk reads the input file line by line.
> * For each line, it evaluates the specified patterns.
> * If a pattern is true, the associated action block is executed.
>
> Fields and Records:
> * awk divides each line into fields based on a delimiter (default is whitespace).
> * Fields are accessed using $1, $2, etc.
> * The whole line is represented by $0.
> * Records are the lines in the file.
>
> Example:
> `awk '{ print $1 }' file.txt` This command prints the first field of each line in the file file.txt.

## Setting Crontab
> `crontab` is a background process manager that can execute specified processes
* `sudo crontab -u root -e` - to configure crontab
* `*/10 * * * * sh /ruta del script` - add to above script to execute every 10 minutes

## Creating Signature
* Shut down VM
* locate path of VM on disk
* run `shasum Born2beRoot.vdi`

## Useful commands
### Checking
* `sudo ufw status` - check ufw status
* `sudo service ufw status` - check ufw status
* `sudo service ssh status` - check SSH status

### Users, groups and passwords
* `ssh denizozd@localhost -p 2222` - enter remotely
* `uname -v` - check OS
* `getent group sudo` or `user` - check user in these 2 groups
* `sudo adduser username` - create new user
* `sudo chage -l username` - check the other password rules
* `sudo vim /etc/login.defs` - check some of the documents
* `sudo vim /etc/pam.d/common-password` - other rules
* `sudo addgroup groupname` - create a new group
* `sudo adduser username groupname` - add the user to the new group

### Host and partitions
* `hostame` - check hostname
* `hostnamectl set-hostname username` - change hostname
* `lsblk` - check partitions

### Sudo
* `sudo -V` - check if sudo is installed
* `sudo adduser username sudo` - add user to sudo
* `getent group sudo` - check if its correct
* `sudo visudo` - check the rules
* `cd /var/log/sudo` - check that log exists
* `cat sudo_config` - check commands executed with sudo

### Firewall
* `dpkg -s ufw`  - check UFW is correctly installed
* `sudo ufw allow 6060` - allow port 6060
* `sudo ufw status` - check the port
* `vim /etc/ssh/sshd_config` - check in this file that only 4242 is configured
* `sudo ufw delete allow 6060` - delete the ports

### Monitoring
* `sudo vim /usr/local/bin/monitoring.sh`  - check script
* `sudo crontab -u root -e` - check cron tabs
* `sudo /etc/init.d/cron stop` - make script stop
* `sudo /etc/init.d/cron start` - make script start
* `dpkg -l | grep lighttpd` - checking installation of lighttpd


## Sources
[Born2beroot-Tutorial (with Screenshots)](https://github.com/gemartin99/Born2beroot-Tutorial/blob/main/README_EN.md#1--download-the-virtual-machine-iso-)

[Another Born2beroot Tutorial](https://github.com/lbordonal/01-Born2beroot/wiki#mandatory-part)

[Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#headers)

