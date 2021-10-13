# Command line fundamentals

In this lab, you'll learn the basics about the command line interface.

### The terminal

When you have connected to your system, you will see a prompt which looks something like this:
```
[ec2-user@ip-172-31-31-136 ~]$
```
This represents your connection to the Linux command line based user interface on your system, also commonly called a ```shell```.
How your prompt looks like differs from system to system based on how the shell is configured. 
There are many different types of shells, with some different capabilities which are of less interest to us at this point.
The most commonly used shell in Linux is the Bourn Again SHell or BASH, which is what you are using now.

### Setting and viewing variables

The look of our shell is controlled by a so called environment variable called PS1. 
Wait, what? Yes, environment variables. Variable? A variable is a thing which can store information, like this:

```
testvariable=5
linuxrules="a variable can also store text strings"
```

💥 Let's test and set a variable in our shell and then use it. We are going to set a variable and then fetch the content of it. Run below command to set your variable.
```
[ec2-user@ip-172-31-31-136 ~]$ reminder="I should go for a hours walk every day"
```

💥 Next, we are going to fetch the value of the variable and print it out on the screen. We do that by running the command ```echo```, which prints things to the screen.

Run below commands:
```
echo $reminder
```

Expected output:
```
[ec2-user@ip-172-31-31-136 ~]$ echo $reminder
I should go for a hours walk every day
[ec2-user@ip-172-31-31-136 ~]$ 
```

Now, why did we spend time on learning this? Because variables is a common way to configure applications and set behavior in Linux.

Now that you understand the concept of variables, we're going to have a look at the variable which controlls how your prompt looks like.

💥 Using the same command (```echo```) we'll now look at the PS1 environment variable.

Run below commands:
```
echo $PS1
```

Expected output:
```
[ec2-user@ip-172-31-31-136 ~]$ echo $PS1
[\u@\h \W]\$
[ec2-user@ip-172-31-31-136 ~]$
```

💥 Change the PS1 environment variable to include some additional information like time and where you are. By using the export command, we can expose the change to the current shell session we are in.

Run below commands:
```
export PS1="[\u@\h:\t:\w]\$ "
```

Expected output:
```
[ec2-user@ip-172-31-31-136 ~]$ export PS1="[\u@\h:\t:\w]\$ "
[ec2-user@ip-172-31-31-136:20:20:49:~]$ 
```

❗ Please note that the change to the PS1 variable which we did above is not permanent, the next time we connect, the change will not be there.

### Environment variables, looking at content of files and manual pages
Environment variables are variables which are set in a user's shell session. In the BASH shell, which you are using, they are normally set in one out of three files.

```
/etc/bashrc
/etc/profile
/home/user/.bashrc
/etc/profile.d/something
```

To have a look at these files, we can use the ```cat```or concatenate command, which prints the contents of files.

💥 Run below command to display the content of the file ```/etc/bashrc``` and /home/user/.bashrc. 
```
cat /etc/bashrc
```
👍 Please note that instead of writing /home/user/.bashrc we are typing ~/.bashrc instead. ```~``` is shorthand for home directory and allows us to not hardcode the name of the user in this example.

Expected output:
```
[ec2-user@ip-172-31-31-136:20:35:36:~]$ cat /etc/bashrc
# /etc/bashrc

# System wide functions and aliases
# Environment stuff goes in /etc/profile

# It's NOT a good idea to change this file unless you know what you
# are doing. It's much better to create a custom.sh shell script in
# /etc/profile.d/ to make custom changes to your environment, as this
# will prevent the need for merging in future updates.
...
```

If we are to change environment variables, we're getting some advise here in the start of the /etc/bashrc file, which is to put environment variables somewhere else.
Those other places are:
```
/etc/profile.d/something.sh
or
~/.bashrc
```

So, environment variables can be set in many different places, how do we then easily tell what is set to what?
There is a command for that, luckily.

💥 Use the ```env``` command to display all environment variables in your shell session.

Run below commands:
```
env
```

Example output:
```
[ec2-user@ip-172-31-31-136:22:07:07:~]$ env
...
LANG=en_US.UTF-8
HISTCONTROL=ignoredups
HOSTNAME=ip-172-31-31-136.eu-central-1.compute.internal
XDG_SESSION_ID=5
USER=ec2-user
PWD=/home/ec2-user
HOME=/home/ec2-user
SHELL=/bin/bash
XMODIFIERS=@im=ibus
SELINUX_USE_CURRENT_RANGE=
SHLVL=1
LOGNAME=ec2-user
DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus
XDG_RUNTIME_DIR=/run/user/1000
PATH=/home/ec2-user/.local/bin:/home/ec2-user/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin
PS1=[\u@\h:\t:\w]$
[ec2-user@ip-172-31-31-136:22:14:20:~]$
```

Let's review some of the more important ones of the environment variables, which are below:
```
LANG=en_US.UTF-8
HOSTNAME=ip-172-31-31-136.eu-central-1.compute.internal
HOME=/home/ec2-user
SHELL=/bin/bash
PATH=/home/ec2-user/.local/bin:/home/ec2-user/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin
[ec2-user@ip-172-31-31-136:22:14:20:~]$
```
So, what do we got here?
```
LANG sets the language and locale for your system.
HOSTNAME sets what the hostname of the system is.
HOME sets your home directory
SHELL sets what your shell is
PATH sets in what places the system should look for commands in. Meaning, all the commands you run in this lab, are programs located in one of the defined directories set in PATH. Note that directories are separated with the : character.
```

### listing files and permissions
The most common command used in Linux is ```ls```. It helps you show files.
Let's try it out to further explore the files which sets environment variables. First we need to find out where we are on the Linux filesystem.

💥 Use ```pwd``` to Print Working Directory, meaning printing out where you are currently.

Run below commands:
```
pwd
```

Expected output:
```
[ec2-user@ip-172-31-31-136:20:40:34:~]$ pwd
/home/ec2-user
[ec2-user@ip-172-31-31-136:20:55:42:~]$ 
```

💥 Let's see if we can find ~/.bashrc, a file located in our home directory. In your terminal, run the ```list directory``` command:

```
ls
```

Above lists all files located where we are currently.

Expected example output:
```
[ec2-user@ip-172-31-31-136 ~]$ ls
[ec2-user@ip-172-31-31-136 ~]$
```

That was perhaps not what we expected. We got nothing. The reason for this is because files which starts with ```.``` in their names, such as .bashrc are automatically hidden. To show hidden files we need to provide an argument to the ```ls``` command. But what argument?
Introducing ```manual pages``` or ```man pages```. In Linux, most commands comes with a so called manual pages. 

💥 Access the manual pages for the ls command. To exit the manual page, press q.

Run below commands:
```
man ls
```

Expected example output:
```
LS(1)                                                                                                         User Commands                                                                                                        LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List information about the FILEs (the current directory by default).  Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

       Mandatory arguments to long options are mandatory for short options too.

       -a, --all
              do not ignore entries starting with .
```

❗ To exit the manual page, press q.

Now we know how to find hidden files.

💥 Look at all the hidden files in your home directory.

Run below commands:
```
ls -a
```

Expected output:
```
[ec2-user@ip-172-31-31-136:21:05:51:~]$ ls -a
.  ..  .bash_history  .bash_logout  .bash_profile  .bashrc  .ssh
[ec2-user@ip-172-31-31-136:21:05:54:~]$ 
```

💥 We have found the .bashrc file. Let's see what more we can learn. Add the ```-l``` switch to the ls command. 
```
ls -la
```

Example output:
```
[ec2-user@ip-172-31-31-136:21:11:12:~]$ ls -la
total 16
drwx------. 3 ec2-user ec2-user  95 Oct 11 18:32 .
drwxr-xr-x. 3 root     root      22 Oct 11 07:46 ..
-rw-------. 1 ec2-user ec2-user  15 Oct 11 18:32 .bash_history
-rw-r--r--. 1 ec2-user ec2-user  18 Apr 21 14:04 .bash_logout
-rw-r--r--. 1 ec2-user ec2-user 141 Apr 21 14:04 .bash_profile
-rw-r--r--. 1 ec2-user ec2-user 376 Apr 21 14:04 .bashrc
drwx------. 2 ec2-user ec2-user  29 Oct 11 07:46 .ssh
[ec2-user@ip-172-31-31-136:21:15:08:~]$ 
```

Let's take some time to break down what we are looking at here. First off, we can can see what user and what group owns what file.
```
-rw-r--r--. 1 ec2-user ec2-user 376 Apr 21 14:04 .bashrc
```

Next we can also see some other things, for example, at what date the file was created and to the far left, what permissions are set for this file.
Permissions are displayed as follows:
* The very first position indicates special permissions, such as if what we are looking at is a directory, a link or has special access to the system.
* The first three positions, in our example starting at the first r, indicates permissions for the user who owns the file.
* The second three positions, in our example starting with r, indicates permissions for the group which owns the file
* The third three positions, in our example starting with r, indicates permissions for others, meaning everyone who does _not_ own the file.
* 
Permissions which can be set are r and as read, w as in write and x as in execute. If you see -, that means a permission is not set.
So, in our example:
```
-rw-r--r--. 1 ec2-user ec2-user 376 Apr 21 14:04 .bashrc
```
Means that the user which owns this file, ec2-user, can read and write to this file.
Means that the group which owns this file, also called ec2-user, can read this file.
Means that everyone who does not own this file, can read this file.

💥 Let's look at something else, at our home directory.

Run below commands:
```
ls -la /home
```

Example output:
```
[ec2-user@ip-172-31-31-136:21:15:08:~]$ ls -la /home
total 0
drwxr-xr-x.  3 root     root      22 Oct 11 07:46 .
dr-xr-xr-x. 18 root     root     236 May  4 17:30 ..
drwx------.  3 ec2-user ec2-user  95 Oct 11 18:32 ec2-user
```

Here we can see the first position being set to ```d```, that means we are looking at a directory called ec2-user.
The first three positions are set to ```rwx```, which means that the user who owns this directory has read, write and execute rights to the directory.
In order to enter into a directory, you need both read and execute rights.

### Changing permissions on files

To change permissions for files and directories, first you need to have write access to them.

💥 Create a new file called secrets in your home directory which we can manipulate.

Run below commands:
```
echo "I still do not like broccoli" >secrets
```

Expected output:
```
[ec2-user@ip-172-31-31-136:21:30:55:~]$ echo "I still do not like broccoli" >secret
[ec2-user@ip-172-31-31-136:21:42:21:~]$
```

Above uses the echo command, which prints text to the terminal to create a file. We do this by using a built in function in our shell, which redirects the text we print from the standard output device, to a file called secrets. This is useful, as almost any command's output can be redirected in a file in this way.

💥 Let's have a look at the file, by using the ```cat``` command again.

Run below commands:
```
cat secret
```

Expected output:
```
[ec2-user@ip-172-31-31-136:21:46:33:~]$ cat secret
I still do not like broccoli
[ec2-user@ip-172-31-31-136:21:46:34:~]$ 
```

To change permissions on files, we use the ```chmod``` command. Use ```man chmod``` to find out more about how it works.
The different people we can give permissions to, if you remember, is: user, group, other. In the command we refer to them as such:
```
u      user
g      group
o      other
```
In short, we pass who or whom we are setting permissions for first and then what permissions to set for them.

For example:
```
chmod u+r
```
Means, give the user who owns the file read rights. We can also combined and assign right to several entities at the same time, as such:

```
chmod ug+rw
```

Above, we assign read and write access to both the user and the group which owns a file.

💥 Now, let's remove the read access to our secret file.

Run below commands:
```
chmod ug-r secret
```

💥 Try to see the content of the file, using ```cat```.

Run below commands:
```
cat secret
```

Expected output:
```
[ec2-user@ip-172-31-31-136:21:58:12:~]$ cat secret
cat: secret: Permission denied
[ec2-user@ip-172-31-31-136:21:58:15:~]$ 
```

💥 Now let's change the permissions so that user and group can read the file again.
```
chmod ug+r secret
```

Did you consider something. What set the permissions in the first place? When you created the file.
The answers is that there is a default setting called UMASK, which controlls what permissions should default to when files and directories are created.

💥 Check what the umask of your system is set to by running the umask command:
```
umask
```

Expected output:
```
[ec2-user@ip-172-31-31-136 ~]$ umask
0002
[ec2-user@ip-172-31-31-136 ~]$ 
```

0002, means the default permissions for creating a file will be ```rw-r--r--```.

⭐ If you want to dive further into how umask works, have a look here: https://man7.org/linux/man-pages/man2/umask.2.html

❗ Setting correct permissions on files is a security fundamental in Linux, having very permissive access rights on files can be a serious security issue. But before you run off to remove read, write and execute permissions for anyone who is not an owner of a file on your Linux system, wait. If you do that, you may break your system. Why is that? Because a lot of programs rely on being able to read files they don't own. For example, to be able to understand if a system has a specific setting or not which the program needs to adopt.

To conclude, to be able to set permissions on a need-to-know basis, we need to know a lot about both the system and the programs running on it.

💥 Let's have a look at an example of a critical, but world readable file.

Run below commands:
```
cat /etc/selinux/config
```

Expected output:
```
[ec2-user@ip-172-31-31-136:22:03:41:~]$ ls -la /etc/selinux/config 
-rw-r--r--. 1 root root 548 May  4 17:28 /etc/selinux/config
[ec2-user@ip-172-31-31-136:22:03:53:~]$ 

```

This file, which controls if SELinux (an important security feature we'll cover more) should be turned on or off and how, is readable to everyone on the system.

💥 Let's imagine we are a bad actor and try to overwrite this file with nothing.

Run below commands:
```
echo "" >/etc/selinux/config
```

Expected output:
```
[ec2-user@ip-172-31-31-136:22:03:53:~]$ echo "" >/etc/selinux/config
-bash: /etc/selinux/config: Permission denied
[ec2-user@ip-172-31-31-136:22:07:07:~]$ 
```

### Priviledged tasks
Next, we're going to add a new user and group. Tasks like this requires admin priviledges.
The default admin user in Linux is called _root_ and has user ID 0, to perform priviledged tasks, we will need to either become a priviledged user or to use a tool such as ```sudo```, which allows us access to doing priviledged tasks.

💥 Add a new user called _test_.
```
sudo useradd test
```

Expected output:
```
[ec2-user@ip-172-31-31-136 ~]$ sudo useradd test
[ec2-user@ip-172-31-31-136 ~]$
```

Note that command which adds the user is called ```useradd```, we are just prefixing it with sudo to be given access to run the command.

💥 Set the password for the user test, to something you remember.
```
sudo passwd test
```

Expected output:
```
[ec2-user@ip-172-31-31-136 ~]$ sudo passwd test
Changing password for user test.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
```

The ```sudo``` command can be put infront of any command, to run that specific command with admin priviledges.
Who can use the command and how the commmand can be used is controlled in a configuration file located in /etc/sudoers.

💥 Let's have a look at that file, using the ```cat``` command.

```
cat /etc/sudoers
```

Expected output:
```
[ec2-user@ip-172-31-31-136 ~]$ cat /etc/sudoers
cat: /etc/sudoers: Permission denied
[ec2-user@ip-172-31-31-136 ~]$
```

💥 We were not allowed to do that. Let's have a closer look on the file to figure out why, using the ```ls``` command.
```
ls -l /etc/sudoers
```

Expected output:
```
[ec2-user@ip-172-31-31-136 ~]$ ls -l /etc/sudoers
-r--r-----. 1 root root 4361 May  4 17:30 /etc/sudoers
[ec2-user@ip-172-31-31-136 ~]$
```

Answer, permissions only allows the user and the group who owns the file to read it, the owning user and group is root.

💥 Let's use sudo to be able to look at the file.
```
sudo cat /etc/sudoers
```

Expected output:
```
...
## The COMMANDS section may have other options added to it.
##
## Allow root to run any commands anywhere 
root	ALL=(ALL) 	ALL

## Allows members of the 'sys' group to run networking, software, 
## service management apps and more.
# %sys ALL = NETWORKING, SOFTWARE, SERVICES, STORAGE, DELEGATING, PROCESSES, LOCATE, DRIVERS

## Allows people in group wheel to run all commands
%wheel	ALL=(ALL)	ALL

## Same thing without a password
# %wheel	ALL=(ALL)	NOPASSWD: ALL

## Allows members of the users group to mount and unmount the 
## cdrom as root
# %users  ALL=/sbin/mount /mnt/cdrom, /sbin/umount /mnt/cdrom

## Allows members of the users group to shutdown this system
# %users  localhost=/sbin/shutdown -h now

## Read drop-in files from /etc/sudoers.d (the # here does not mean a comment)
#includedir /etc/sudoers.d
ec2-user	ALL=(ALL)	NOPASSWD: ALL
```

In this file we can see which users are allowed to do what with sudo.
Good to keep in mind is that when sudo is used, actions are logged. If we have a look at system logs, we can see when we accessed the sudoers file.

💥 Review last five lines of system logs using the journalctl command.
```
journalctl -n 5
```

Expected output should include:
```
[ec2-user@ip-172-31-31-136 ~]$ journalctl -n 5
-- Logs begin at Mon 2021-10-11 07:45:09 UTC, end at Wed 2021-10-13 12:48:12 UTC. --
Oct 13 12:48:12 ip-172-31-31-136.eu-central-1.compute.internal sudo[106040]: ec2-user : TTY=pts/1 ; PWD=/home/ec2-user ; USER=root ; COMMAND=/bin/cat /etc/sudoers
```

💥 Now that we have created a new local user, become the test user, using ```su```, see where you are and then list all files in the users home directory.
```
su - test
pwd
ls -la
```

Expected output:
```
[ec2-user@ip-172-31-31-136 ~]$ su - test
Password: 
Last failed login: Wed Oct 13 07:00:19 UTC 2021 from 199.19.225.248 on ssh:notty
[test@ip-172-31-31-136 ~]$ pwd
/home/test
[test@ip-172-31-31-136 ~]$ ls -la
total 12
drwx------. 2 test test  62 Oct 13 12:00 .
drwxr-xr-x. 4 root root  34 Oct 13 12:00 ..
-rw-r--r--. 1 test test  18 Apr 21 14:04 .bash_logout
-rw-r--r--. 1 test test 141 Apr 21 14:04 .bash_profile
-rw-r--r--. 1 test test 376 Apr 21 14:04 .bashrc
```

Note that when we created the user, a home directory (/home/test) was also created for that user.

💥 Exit the session for the test user by running exit
```
exit
```

Expected output:
```
[test@ip-172-31-31-136 ~]$ exit
logout
[ec2-user@ip-172-31-31-136 ~]$
```

### Manipulating text and output and using pipe
Next, there are a number of useful tools in Linux for doing text manipulation, allowing your to format, edit and search text.
These tools are:

```
grep   Searches text in files
cut    Removes sections from each line in a file
sed    Allows you to manipulates text, such as printing, inserting, deleting and replacing content.
```

These tool are all extremely capable and can be used to a lot of things, at this point we'll just briefly introduce the tools together with a common usecase.

💥 Use ```grep``` to find users which can access the BASH shell on our system
```
grep bash /etc/passwd
```

Example output:
```
[ec2-user@ip-172-31-31-136 etc]$ grep bash /etc/passwd
root:x:0:0:root:/root:/bin/bash
ec2-user:x:1000:1000:Cloud User:/home/ec2-user:/bin/bash
test:x:1001:1001::/home/test:/bin/bash
[ec2-user@ip-172-31-31-136 etc]$
```

💥 Use ```cut``` to just print out username from the /etc/passwd file
```
cut -d: -f1 /etc/passwd
```

💥 Use ```sed``` to replace the username root with 'the administrator'
```
sed 's/root/the administrator/g' /etc/passwd
```

Expected output:
```
[ec2-user@ip-172-31-31-136 ~]$ sed 's/root/the administrator/g' /etc/passwd
the administrator:x:0:0:the administrator:/the administrator:/bin/bash
...
```

Next, we are going to add some of these things together. You can send the output of one program to another program by using the symbol ```|``` in the shell. Also called a pipe.
💥 Print users which can access a shell and replace root with the administrator.
```
grep bash /etc/passwd|cut -d: -f1|sed 's/root/the administrator/g' 
```

Expected output:
```
[ec2-user@ip-172-31-31-136 ~]$ grep bash /etc/passwd|cut -d: -f1|sed 's/root/the administrator/g' 
the administrator
ec2-user
test
[ec2-user@ip-172-31-31-136 ~]$ 
```

### Editing files
Next thing which we need to know to be able to navigate the Linux operating system is how to edit files.
The most common text editor in Linux is called ```vi```, which we'll have a quick look at.

💥 Create a new file and start editing it using ```vi```
```
vi testfile
```

Expected output:
```
                                                             
~
~
~
~
"testfile" [New File]
```

💥 To start inserting text, press ```i``` which stands for insert. To remove text, use backspace.
```
i
```

Expected output:
```
Some text you have input
~
~
~
~
--- INSERT ---
```

💥 Now it's time file and close the text editor by pressing ```ESC``` and then ```:wq``` and then ```ENTER```
```
ESC
:wq
```

❗ After you press ESC and type in :wq, make sure ```:wq``` it should appear in the bottom on the screen, before you press enter. As such:
```
Some text some text
~
~
~
~
:wq
```

If you didn't know, :wq stands for write quit. You you just want to save something and continue editing, just type in :w.
Now you know enough to start exploring Linux, congratulations! 😃


