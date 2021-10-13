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

Sometimes, the ```man``` command can help and tell us what is what, in other cases, we can use ```dnf``` to identify what RPM owns a file and then get more info.
💥 Explore at least 5 different processes running.

Example:
```
root       71700       1  0 Oct11 ?        00:00:03 /usr/libexec/packagekitd

man packagekitd
dnf whatprovides /usr/libexec/packagekitd
dnf info PackageKit
```








