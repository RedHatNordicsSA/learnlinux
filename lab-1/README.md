# Exploring the operating system
Next, we are going to explore the different parts of the operating system, using our skills from the previous lab.

### Exploring filesystems and directories
If it helps, everything in Linux is files. Disk devices, network devices, directories, links, it's all files. So to explore the operating system is to explore a bunch of files.
Before we can start with that, it helps to know where different files normally lives.

💥 Use the ```tree``` command which we installed in lab-0 to explore the filesystem.
```
tree / -L 1
```

Expected output:
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

Directories are all connected to different storage devices. Too view these there are a couple of different useful tools.
💥 List all block devices on the system together with that filesystems which are mounted.
```
lsblk
lsblk -f
```

Expected output:
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

Here we can see that we have a disk device called xvda which has two partitions on it, one of those partitions is connected to a XFS filesystem (default in Red Hat Enterprise Linux) which is mounted at /. That means that all directories which are on temporary filesystem are stored on that disk.

💥 List filesystem utilization
```
df -h
```

Expected output:
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

Here we can see some of the temporary filesystems as well as the /dev/xvda2 device file which points to the partition where we keep the XFS filesystem.

### Processes and services
To build yourself an idea of what is running on a system is important for both understanding the system and troubleshooting it.

To list running processes system wide, we can use the command ```ps```.

💥 List all currently running programs spawned from your own user
```
ps
```

Example output:
```
[ec2-user@ip-172-31-31-136 etc]$ ps
    PID TTY          TIME CMD
 107382 pts/1    00:00:00 bash
 108369 pts/1    00:00:00 ps
[ec2-user@ip-172-31-31-136 etc]$ 
```

Each process is assigned an unique so called process id, or PID, when it's started.

💥 Next, list all processes systemwide.
```
ps -ef
```

The first processes spawned in Red Hat Enterprise Linux is systemd, which is the system and services program which in Red Hat Enterprise Linux also initializes the operating system at boot. But there are a lot of processes in the list that you probably never heard about. Sometimes, the ```man``` command can help and tell us what is what, in other cases, we can use ```rpm``` or ```dnf``` to identify what RPM owns a file and then get more info.

💥 Explore at least 5 different processes running that you do not know about.

Example:
```
root       71700       1  0 Oct11 ?        00:00:03 /usr/libexec/packagekitd

man packagekitd
rpm -q --whatprovides /usr/libexec/packagekitd
rpm -qi PackageKit
dnf whatprovides /usr/libexec/packagekitd
dnf info PackageKit
```

If you want to list the different services running in the system, we can use the ```systemctl``` command.

💥 List all currently running processes on your server
```
systemctl list-unit-files
```

💥 Select five different running services and run a status command on it.
Example:
```
systemctl status tuned
```

If we want to see a live process list, we can use a tool such as ```top```.
💥 Review which processes consumes the most resources
```
top
```

### Hardware








