# Exploring the operating system
Next, we are going to explore the different parts of the operating system, using our skills from the previous lab.

### Exploring filesystems and directories
If it helps, everything in Linux is files. Disk devices, network devices, directories, links, it's all files. So to explore the operating system is to explore a bunch of files.
Before we can start with that, it helps to know where different types of files normally lives.

💥 Explore the ```tree``` command.
```
man tree
```

💥 Use the ```tree``` command which we installed in lab-0 to explore the first couple of directories of the filesystem.
<details>
<summary>I could use some help...</summary>
<p>
  
```
tree / -L 1
tree / -L 2
tree / -L 3
```
</p>
</details> 


Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ tree / -L 1
/
├── bin -> usr/bin
├── boot
├── data
├── dev
├── etc
├── home
├── lib -> usr/lib
├── lib64 -> usr/lib64
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin -> usr/sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```
</p>
</details> 


Now, let's review what we got here.
```
/                       The root directory.
├── bin -> usr/bin      Here we got common programs used by all users.
├── boot                Here we have parts used to boot Red Hat Enterprise Linux
├── dev                 Here we have device files, like disk devices.
├── etc                 Here a majority of configuration is kept
├── home                Default, home directories get's created here
├── lib -> usr/lib      Libraries
├── lib64 -> usr/lib64  Libraries
├── media               CD/DVD roms shows up here
├── mnt                 Temporarily mounted filesystems
├── opt                 Add-on software, rarely used
├── proc                Virtual filesystem providing process and kernel information as files.
├── root                The admin users home directory
├── run                 Information about the running system since last boot
├── sbin -> usr/sbin    Here we got common programs which requires admin or priviledged access.
├── srv                 Site-specific data served by this system, rarely used.
├── sys                 Contains information about devices, drivers, and some kernel features.
├── tmp                 Temporary files are stored here
├── usr                 A majority of the systems applications lives here
└── var                 Variable files: files whose content is expected to continually change, like logs.
```

⭐ For more information about the directory structure, see: https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard

💥 To navigate around between directories, we use the ```cd``` (change directory) command. Navigate to the different directories and have a look at at least 10 different files.
Example:
```
cd /etc
ls
less login.defs
```

Directories are all connected to different storage devices. Too view these there are a couple of different useful tools. Let's start with ```lsblk```.

💥 Explore the ```lsblk``` command
```
man lsblk
```

💥 List all block devices on the system together with that filesystems which are mounted.
<details>
<summary>I could use some help...</summary>
<p>
  
```
lsblk
lsblk -f
```
</p>
</details> 

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 etc]$ lsblk
NAME    MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  10G  0 disk 
├─xvda1 202:1    0   1M  0 part 
└─xvda2 202:2    0  10G  0 part /
[ec2-user@ip-172-31-31-136 etc]$ lsblk -f
NAME    FSTYPE LABEL UUID                                 MOUNTPOINT
xvda                                                      
├─xvda1                                                   
└─xvda2 xfs          d35fe619-1d06-4ace-9fe3-169baad3e421 /
[ec2-user@ip-172-31-31-136 etc]$  
```
</p>
</details>

Here we can see that we have a disk device called xvda which has two partitions on it, one of those partitions is connected to a XFS filesystem (default in Red Hat Enterprise Linux) which is mounted at /. That means that all directories which are on temporary filesystem are stored on that disk.

💥 Explore the ```df``` command
```
man df
```

💥 List filesystem utilization in a way you can read it easily
<details>
<summary>I could use some help...</summary>
<p>
  
```
df -h
```
</p>
</details> 

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 etc]$ df -h
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        375M     0  375M   0% /dev
tmpfs           404M     0  404M   0% /dev/shm
tmpfs           404M   41M  363M  11% /run
tmpfs           404M     0  404M   0% /sys/fs/cgroup
/dev/xvda2       10G  2.4G  7.7G  24% /
tmpfs            81M     0   81M   0% /run/user/1000
[ec2-user@ip-172-31-31-136 etc]$ 
```
</p>
</details> 

Here we can see some of the temporary filesystems as well as the /dev/xvda2 device file which points to the partition where we keep the XFS filesystem.

### Processes and services
To build yourself an idea of what is running on a system is important for both understanding the system and troubleshooting it.

To list running processes, we can use the command ```ps```.

💥 Explore the ps command
```
man ps
```

💥 List all currently running programs spawned from your own user
<details>
<summary>I could use some help...</summary>
<p>
  
```
ps
```
</p>
</details> 


Example output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 etc]$ ps
    PID TTY          TIME CMD
 107382 pts/1    00:00:00 bash
 108369 pts/1    00:00:00 ps
[ec2-user@ip-172-31-31-136 etc]$ 
```
</p>
</details> 

Each process is assigned an unique so called process id, or PID, when it's started.

💥 Next, list all processes systemwide.
<details>
<summary>I could use some help...</summary>
<p>
  
```
ps -ef
```
</p>
</details> 

The first processes spawned in Red Hat Enterprise Linux is systemd, which is the system and services program which in Red Hat Enterprise Linux also initializes the operating system at boot. But there are a lot of processes in the list that you probably never heard about. Sometimes, the ```man``` command can help and tell us what is what, in other cases, we can use ```rpm``` or ```dnf``` to identify what RPM owns a file and then get more info.

💥 Explore at least 5 different processes running that you do not know about.

Example:
```
root       71700       1  0 Oct11 ?        00:00:03 /usr/libexec/packagekitd

man packagekitd
rpm -q --whatprovides /usr/libexec/packagekitd
rpm -qi PackageKit
```

If you want to list the different services running in the system, we can use the ```systemctl``` command.

💥 Explore the systemctl command
```
man systemctl
```

💥 List all currently running processes on your server
<details>
<summary>I could use some help...</summary>
<p>
  
```
systemctl list-unit-files
```
</p>
</details> 

💥 Select five different running services and run a status command on it.
Example:
```
systemctl status tuned
```

If we want to see a live process list, we can use a tool such as ```top```.

💥 Explore the top command
```
man top
```

💥 Review which processes consumes the most resources
<details>
<summary>I could use some help...</summary>
<p>
  
```
top
```
</p>
</details> 

### Hardware
To view the hardware the operating system runs on, there are a lot of different tools. These tools fetches information from the /proc filesystem, which means that you can also go there to get the unformated information.

#### CPU

💥 Explore the lscpu command
```
man lscpu
```


💥 Have a look at the systems CPU
<details>
<summary>I could use some help...</summary>
<p>
  
```
lscpu
```
</p>
</details> 

#### Memory
💥 Explore the lsmem command
```
man lsmem
```

💥 Have a look at the systems memory
<details>
<summary>I could use some help...</summary>
<p>
  
```
lsmem
```
</p>
</details> 

#### Disks
💥 Explore the parted command
```
man parted
```

💥 Exmine attached disks
<details>
<summary>I could use some help...</summary>
<p>
  
```
sudo parted -l
```
</p>
</details> 

#### Network devices
💥 Explore the nmcli command
```
man nmcli
```

💥 Check out your network devices
<details>
<summary>I could use some help...</summary>
<p>
  
```
nmcli device show
```
</p>
</details> 

#### General hardware
💥 Explore the dmidecode command
```
man dmidecode
```

💥 Have a look at various hardware devices described in the systems SMBIOS/DMI
<details>
<summary>I could use some help...</summary>
<p>
  
```
sudo dmidecode
```
</p>
</details> 


💥 Explore the dmesg command
```
man dmesg
```

💥 Examine messages from the kernels ring buffer, where you find a lot of initialization information about hardware
<details>
<summary>I could use some help...</summary>
<p>
  
```
dmesg
```
</p>
</details> 

### Logs
Most program generates logs. In Red Hat Enterprise Linux, systemd is responsible to store them.
We can access the logs by using the ```journalctl``` command.

💥 Explore the journalctl command
```
man journalctl
```


💥 Review the full system logs. Quit by pressing ```q```
<details>
<summary>I could use some help...</summary>
<p>
  
```
journalctl
```
</p>
</details> 



💥 Review logs for the boot process
<details>
<summary>I could use some help...</summary>
<p>
  
```
journalctl -b
```
</p>
</details> 



💥 Review logs for a specific service
<details>
<summary>I could use some help...</summary>
<p>
  
```
journalctl -u tuned
```
</p>
</details> 



💥 Follow the live log stream
<details>
<summary>I could use some help...</summary>
<p>
  
```
journalctl -f
```
</p>
</details> 



💥 Fetch logs since a specific time range
<details>
<summary>I could use some help...</summary>
<p>
  
```
journalctl --since "1 hour ago"
```
</p>
</details> 



💥 Fetch all logs for your user, using the $UID environment variable which containers the user id of our user.
<details>
<summary>I could use some help...</summary>
<p>
  
```
journalctl _UID=$UID
```
</p>
</details> 

Logs can also be found stored in files in /var/log.



💥 List logs files
```
ls /var/log
```

💥 Search general log file /var/log/messages for the message "error"
<details>
<summary>I could use some help...</summary>
<p>
  
```
sudo grep -i error /var/log/messages
```
</p>
</details> 

### Common services
As you found when exploring services and processes, there are a lot of moving parts in Linux. Here's some of the more important ones.

#### sshd
The secure shell daemon is the default way you connect to Linux. It provides you with the terminal connection to your shell over an encryped channel.

💥 Explore sshd and systemctl
```
man sshd
man systemctl
```

💥 Review the status of the sshd daemon using systemctl
<details>
<summary>I could use some help...</summary>
<p>
  
```
systemctl status sshd
```
</p>
</details> 

💥 Review logs for the sshd daemo.
<details>
<summary>I could use some help...</summary>
<p>
  
```
journalctl -u sshd
```
</p>
</details> 

* The configuration for the daemon and the ssh client is located in /etc/ssh.
* Configuration for the service: /etc/ssh/sshd_config
* Configuration for the client: /etc/ssh/ssh_config

### chronyd
Chronyd is a service which syncronizes the clock on your system. Without time synchronization on your system you cannot tell when something happened, more importantly a lot of service depends on time to be synchronized across your systems.

💥 Explore chronyd and chronyc
```
man chronyd
man chronyc
```

💥 Review the status of chronyd daemon
<details>
<summary>I could use some help...</summary>
<p>
  
```
systemctl status chronyd
```
</p>
</details> 

💥 Review logs for chronyd daemon
<details>
<summary>I could use some help...</summary>
<p>
  
```
journalctl -u chronyd
```
</p>
</details> 

💥 Check time synchronization on the system
<details>
<summary>I could use some help...</summary>
<p>
  
```
chronyc tracking
```
</p>
</details> 


#### NetworkManager
The program which controls the systems network configuration is called Network Manager. 

💥 Explore NetworkManager and nmcli
```
man NetworkManager
man nmcli
```

💥 Review the status of NetworkManager
<details>
<summary>I could use some help...</summary>
<p>
  
```
systemctl status NetworkManager
```
</p>
</details> 

💥 Review logs for NetworkManager
<details>
<summary>I could use some help...</summary>
<p>
  
```
journalctl -u NetworkManager
```
</p>
</details> 

💥 Review network configuration
<details>
<summary>I could use some help...</summary>
<p>
  
```
nmcli
nmcli device show
```
</p>
</details> 

#### sssd
SSSD provides a set of daemons to manage access to remote directories and authentication mechanisms.
Use ```sssd``` to integrate to any central identity and authentication system, such as Active Directory, ldap or radius, etc.

💥 Explore sssd
```
man sssd
```

💥 Review the status of sssd
<details>
<summary>I could use some help...</summary>
<p>
  
```
systemctl status sssd
```
</p>
</details> 

💥 Review logs for sssd
<details>
<summary>I could use some help...</summary>
<p>
  
```
journalctl -u sssd
```
</p>
</details> 

Configuration is stored in /etc/sssd.

#### Web console
The web console, sometimes called cockpit, after it's open source project, is a lightweight web interface for your Red Hat Enterprise Linux server.
For beginners to Linux, it's highly recommended. For veteran Linux users, it's highly recommended. You get it, it's good stuff.

💥 Install Web console
```
sudo dnf install cockpit
```

💥 Configure Web console to use port 443 instead of the default port 9090. You'll understand a bit better what we are doing after the chapter on security. For now, just understand that we are managing a new set of permissions, enforced by the security feature SELinux. If you want, just cut and paste all the commands into the prompt.
```
sudo mkdir /etc/systemd/system/cockpit.socket.d
sudo su -
sudo cat << 'EOF' >/etc/systemd/system/cockpit.socket.d/listen.conf
[Socket]
ListenStream=
ListenStream=443
EOF
exit
sudo restorecon -R /etc/systemd/system/cockpit.socket.d
sudo semanage port -m -t websm_port_t -p tcp 443
```

💥 Enable the web console service
```
sudo systemctl daemon-reload
sudo systemctl restart cockpit.socket
```

💥 Voila, go ahead and access the web console using your browser, at https://yoursystem
Take some time to explore the different menus and see what information is available.


We're done with this section. Now you know enough to get started with the more advanced topics, which is security. 😃

[Go to the next lab, lab 2](../lab-2/README.md) 






