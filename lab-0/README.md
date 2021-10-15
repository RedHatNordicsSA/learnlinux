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
reminder="I should go for a hours walk every day"
```

💥 Next, we are going to fetch the value of the variable and print it out on the screen. We do that by running the command ```echo```, which prints things to the screen.
```
echo $reminder
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ echo $reminder
I should go for a hours walk every day
[ec2-user@ip-172-31-31-136 ~]$ 
```
</p>
</details> 

Now, why did we spend time on learning this? Because variables is a common way to configure applications and set behavior in Linux.

Now that you understand the concept of variables, we're going to have a look at the variable which controlls how your prompt looks like.

💥 Using the same command (```echo```) we'll now look at the PS1 environment variable.
```
echo $PS1
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ echo $PS1
[\u@\h \W]\$
[ec2-user@ip-172-31-31-136 ~]$
```
</p>
</details> 

💥 Change the PS1 environment variable to include some additional information like time and where you are. By using the export command, we can expose the change to the current shell session we are in.
```
export PS1="[\u@\h:\t:\w]\$ "
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ export PS1="[\u@\h:\t:\w]\$ "
[ec2-user@ip-172-31-31-136:20:20:49:~]$ 
```
</p>
</details> 

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

💥 Run below command to display the content of the file ```/etc/bashrc```
```
cat /etc/bashrc
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136:20:35:36:~]$ cat /etc/bashrc
# /etc/bashrc

# System wide functions and aliases
# Environment stuff goes in /etc/profile

# It's NOT a good idea to change this file unless you know what you
# are doing. It's much better to create a custom.sh shell script in
# /etc/profile.d/ to make custom changes to your environment, as this
# will prevent the need for merging in future updates.
```
</p>
</details> 

If we are to change environment variables, we're getting some advise here in the start of the /etc/bashrc file, which is to put environment variables somewhere else.
Those other places are:
```
/etc/profile.d/something.sh
or
~/.bashrc
```

👍 ~/.bashrc is shorthand for the current users home directory, in your case /home/ec2-user/.bashrc

So, environment variables can be set in many different places, how do we then easily tell what is set to what?
There is a command for that, luckily.

💥 Use the ```env``` command to display all environment variables in your shell session.
```
env
```

Example output:
<details>
<summary>Show</summary>
<p>
  
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
</p>
</details> 

Let's review some of the more important ones of the environment variables, which are below:
```
LANG=en_US.UTF-8
HOSTNAME=ip-172-31-31-136.eu-central-1.compute.internal
HOME=/home/ec2-user
SHELL=/bin/bash
PATH=/home/ec2-user/.local/bin:/home/ec2-user/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin
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
```
pwd
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136:20:40:34:~]$ pwd
/home/ec2-user
[ec2-user@ip-172-31-31-136:20:55:42:~]$ 
```
</p>
</details> 

💥 Let's see if we can find ~/.bashrc, a file located in our home directory. In your terminal, run the ```list directory``` command:

```
ls
```

Above lists all files located where we are currently.

Expected example output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ ls
[ec2-user@ip-172-31-31-136 ~]$
```
</p>
</details> 

That was perhaps not what we expected. We got nothing. The reason for this is because files which starts with ```.``` in their names, such as .bashrc are automatically hidden. To show hidden files we need to provide an argument to the ```ls``` command. But what argument?
Introducing ```manual pages``` or ```man pages```. In Linux, most commands comes with a so called manual pages. 

💥 Access the manual pages for the ls command. To exit the manual page, press q.
```
man ls
```

Expected example output:
<details>
<summary>Show</summary>
<p>
  
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
</p>
</details> 

❗ To exit the manual page, press q.

Now we know how to find hidden files.

👍 If you want to learn more about a command, viewing the manual page of the command is a good way to start. Remember that as we move forwards in the lab.

💥 Look at all the hidden files in your home directory.
<details>
<summary>I could use some help...</summary>
<p>
  
```
ls -a
```
</p>
</details> 

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136:21:05:51:~]$ ls -a
.  ..  .bash_history  .bash_logout  .bash_profile  .bashrc  .ssh
[ec2-user@ip-172-31-31-136:21:05:54:~]$ 
```
</p>
</details> 

💥 We have found the .bashrc file. Let's see what more we can learn. Add the ```-l``` switch to the ls command. 
```
ls -la
```

Example output:
<details>
<summary>Show</summary>
<p>
  
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
</p>
</details> 

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
```
ls -la /home
```

Example output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136:21:15:08:~]$ ls -la /home
total 0
drwxr-xr-x.  3 root     root      22 Oct 11 07:46 .
dr-xr-xr-x. 18 root     root     236 May  4 17:30 ..
drwx------.  3 ec2-user ec2-user  95 Oct 11 18:32 ec2-user
```
</p>
</details> 

Here we can see the first position being set to ```d```, that means we are looking at a directory called ec2-user.
The first three positions are set to ```rwx```, which means that the user who owns this directory has read, write and execute rights to the directory.
In order to enter into a directory, you need both read and execute rights.

### Changing permissions on files

To change permissions for files and directories, first you need to have write access to them.

💥 Create a new file called secrets in your home directory which we can manipulate.

```
echo "I still do not like broccoli" >secret
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136:21:30:55:~]$ echo "I still do not like broccoli" >secret
[ec2-user@ip-172-31-31-136:21:42:21:~]$
```
</p>
</details> 

Above uses the echo command, which prints text to the terminal to create a file. We do this by using a built in function in our shell, which redirects the text we print from the standard output device, to a file called secret. This is useful, as almost any command's output can be redirected in a file in this way.

💥 Let's have a look at the file, by using the ```cat``` command again.

```
cat secret
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136:21:46:33:~]$ cat secret
I still do not like broccoli
[ec2-user@ip-172-31-31-136:21:46:34:~]$ 
```
</p>
</details> 

Next, let's explore moving and copying files. Let's start by moving our secret file.
💥 Rename your file to .secret to hide the file from open view.

```
mv secret .secret
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ mv secret .secret
[ec2-user@ip-172-31-31-136 ~]$ 
```
</p>
</details> 

💥 Verify that the file is hidden by running ```ls```.

💥 Now, let's trying copying.

```
cp .secret .secret.backup
```

Verify that the file was copied by running ```ls```.

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

```
chmod ug-r .secret
```

💥 Try to see the content of the file, using ```cat```.

```
cat .secret
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136:21:58:12:~]$ cat .secret
cat: .secret: Permission denied
[ec2-user@ip-172-31-31-136:21:58:15:~]$ 
```
</p>
</details> 

💥 Now let's change the permissions so that user and group can read the file again.
```
chmod ug+r .secret
```

Another way to keep something secret, is to put it in a directory which has restricted access.
💥 Create a directory using the ```mkdir``` command.
```
mkdir secretdir
```

💥 Next, move the secret file into this directory, using the ```mv``` (move) command.
```
mv .secret secretdir/
```

💥 Let's check the permissions on our new directory.
```
ls -l
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ ls -l
total 4
drwxrwxr-x. 2 ec2-user ec2-user  6 Oct 13 17:43 secretdir
```
</p>
</details>

The secretdir was created with permissions for anyone on this system to go into the directory. You need read (r) and execute (x) access for that.

💥 Remove the read and execute access for the directory and then try to enter into it using the change directory command ``cd``` and list files in it.
```
chmod a-rx secretdir
cd secretdir
ls secretdir
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ chmod a-rx secretdir
[ec2-user@ip-172-31-31-136 ~]$ cd secretdir
-bash: cd: secretdir: Permission denied
[ec2-user@ip-172-31-31-136 ~]$ ls secretdir
ls: cannot open directory 'secretdir': Permission denied
[ec2-user@ip-172-31-31-136 ~]$ 
```
</p>
</details>

Did you consider something. What set the permissions in the first place? When you created the file or directory.
The answers is that there is a default setting called UMASK, which controlls what permissions should default to when files and directories are created.

💥 Check what the umask of your system is set to by running the umask command:
```
umask
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ umask
0002
[ec2-user@ip-172-31-31-136 ~]$ 
```
</p>
</details>

0002, means the default permissions for creating a file will be ```rw-r--r--``` and ```drwxrwxr-x.``` for directories.

⭐ If you want to dive further into how umask works, have a look here: https://man7.org/linux/man-pages/man2/umask.2.html

❗ Setting correct permissions on files is a security fundamental in Linux, having very permissive access rights on files can be a serious security issue. But before you run off to remove read, write and execute permissions for anyone who is not an owner of a file on your Linux system, wait. If you do that, you may break your system. Why is that? Because a lot of programs rely on being able to read and execute files and directories they don't own. For example, to be able to understand if a system has a specific setting or not which the program needs to adopt.

To conclude, to be able to set permissions on a need-to-know basis, we need to know a lot about both the system and the programs running on it.

💥 Let's have a look at an example of a critical, but world readable file.

```
ls /etc/selinux/config
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136:22:03:41:~]$ ls -la /etc/selinux/config 
-rw-r--r--. 1 root root 548 May  4 17:28 /etc/selinux/config
[ec2-user@ip-172-31-31-136:22:03:53:~]$ 
```
</p>
</details>

This file, which controls if SELinux (an important security feature we'll cover more) should be turned on or off and how, is readable to everyone on the system.

💥 Let's imagine we are a bad actor and try to overwrite this file with nothing.

```
echo "" >/etc/selinux/config
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136:22:03:53:~]$ echo "" >/etc/selinux/config
-bash: /etc/selinux/config: Permission denied
[ec2-user@ip-172-31-31-136:22:07:07:~]$ 
```
</p>
</details>

### Removing files
To remove files, we use the command ```rm```. It's a very powerful command and should be used with great care. Especially if you are an admin user, you may accidentally remove important things which causes the operating system to stop working.

💥 Let's create some test files which we then can remove. Below commands will create a number of files in your home directory.
```
for i in {1..1000}; do touch file-$i; done
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ for i in {1..1000}; do touch file-$i; done
[ec2-user@ip-172-31-31-136 ~]$
```
</p>
</details>

Verify that the files were created by using ```ls```.

💥 Next, we'll remove the write permissions on a number of the files.
```
chmod a-w file-7*
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ chmod a-w file-7*
[ec2-user@ip-172-31-31-136 ~]$
```
</p>
</details>

Now, if you imagine that we need to delete these files, deleting them one by one would take a bit too long. The shell has some built in functions that allows us to easily run a command on a larger number of things.

💥 Remove one file
```
rm file-1
```

💥 Remove all test files
```
rm file-*
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ rm file-*
rm: remove write-protected regular empty file 'file-7'? y
rm: remove write-protected regular empty file 'file-70'? y
rm: remove write-protected regular empty file 'file-700'? ^C
```
</p>
</details>

<details>
<summary>A tip for how to fix things</summary>
<p>

Use ```ctrl+c``` to break out of the remove loop.
</p>
</details>

When a file does not have write permissions, ```rm``` will ask the user if it should really be removed. Here we have two options, to set write permissions for the files or to just ask ```rm``` to remove the files anyways.

💥 Use the manpage for ```rm``` to figure out how to remove all the files without having to enter in y, one hundred times.
```
man rm
```

💥 Now delete all the files without getting prompted...
<details>
<summary>I could use some help...</summary>
<p>
       
```
rm file-* -f
```
</p>
</details> 

### Priviledged tasks
Next, we're going to add a new user and group. Tasks like this requires admin priviledges.
The default admin user in Linux is called _root_ and has user ID 0, to perform priviledged tasks, we will need to either become a priviledged user or to use a tool such as ```sudo```, which allows us access to doing priviledged tasks. Sudo helps us to have a least priviledge approach to what we are doing, using a non-priviledged user when we can and only use priviledged access when we need to. This also helps us to reduce the number of system-fatal mistakes which we do as we learn and gain experience.

💥 Add a new user called _test_.
```
sudo useradd test
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ sudo useradd test
[ec2-user@ip-172-31-31-136 ~]$
```
</p>
</details>

Note that command which adds the user is called ```useradd```, we are just prefixing it with sudo to be given access to run the command.

💥 Set the password for the user test, to something you remember.
```
sudo passwd test
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ sudo passwd test
Changing password for user test.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
```
</p>
</details>

The ```sudo``` command can be put infront of any command, to run that specific command with admin priviledges.
Who can use the command and how the commmand can be used is controlled in a configuration file located in /etc/sudoers.

💥 Let's have a look at that file, using the ```cat``` command.

```
cat /etc/sudoers
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ cat /etc/sudoers
cat: /etc/sudoers: Permission denied
[ec2-user@ip-172-31-31-136 ~]$
```
</p>
</details>

💥 We were not allowed to do that. Let's have a closer look on the file to figure out why, using the ```ls``` command.
```
ls -l /etc/sudoers
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ ls -l /etc/sudoers
-r--r-----. 1 root root 4361 May  4 17:30 /etc/sudoers
[ec2-user@ip-172-31-31-136 ~]$
```
</p>
</details>

Answer, permissions only allows the user and the group who owns the file to read it, the owning user and group is root.

💥 Let's use sudo to be able to look at the file. We're going to use a new tool ```less``` to do that.
```
sudo less /etc/sudoers
```

The ```less``` commands allows you to explore larger files by scrolling up and down in the file.
Use the arrow keys or PgUp/Down to scroll. If you want to search for a keyword, type ```/thekeyword```.
To exit ```less```, press ```q```.

In /etc/sudoers file we can see which users are allowed to do what with sudo.
Good to keep in mind is that when sudo is used, actions are logged. If we have a look at system logs, we can see when we accessed the sudoers file.

If we are to edit 

💥 Review last five lines of system logs using the journalctl command.
```
journalctl -n 5
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ journalctl -n 5
-- Logs begin at Mon 2021-10-11 07:45:09 UTC, end at Wed 2021-10-13 12:48:12 UTC. --
Oct 13 12:48:12 ip-172-31-31-136.eu-central-1.compute.internal sudo[106040]: ec2-user : TTY=pts/1 ; PWD=/home/ec2-user ; USER=root ; COMMAND=/bin/cat /etc/sudoers
```
</p>
</details>

💥 Now that we have created a new local user, become the test user, using ```su```, see where you are and then list all files in the users home directory.
```
su - test
pwd
ls -la
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
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
</p>
</details>

Note that when we created the user, a home directory (/home/test) was also created for that user.

💥 Exit the session for the test user by running exit
```
exit
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[test@ip-172-31-31-136 ~]$ exit
logout
[ec2-user@ip-172-31-31-136 ~]$
```
</p>
</details>

### Manipulating text and output and using pipe
Next, there are a number of useful tools in Linux for doing text manipulation, allowing your to format, edit and search text.
These tools are:

```
grep   Searches text in files
cut    Removes sections from each line in a file
sed    Allows you to manipulates text, such as printing, inserting, deleting and replacing content.
tail   Print the end of a file
head   Print the start of a file
diff   See what differences there are between files
```

These tool are all extremely capable and can be used to a lot of things, at this point we'll just briefly introduce the tools together with a common usecase.

💥 Use ```grep``` to find users which can access the BASH shell on our system
```
grep bash /etc/passwd
```

Example output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 etc]$ grep bash /etc/passwd
root:x:0:0:root:/root:/bin/bash
ec2-user:x:1000:1000:Cloud User:/home/ec2-user:/bin/bash
test:x:1001:1001::/home/test:/bin/bash
[ec2-user@ip-172-31-31-136 etc]$
```
</p>
</details>

💥 Use ```cut``` to just print out username from the /etc/passwd file
```
cut -d: -f1 /etc/passwd
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ cut -d: -f1 /etc/passwd
root
bin
daemon
adm
lp
sync
shutdown
halt
mail
operator
games
ftp
nobody
dbus
systemd-coredump
systemd-resolve
tss
hacluster
unbound
polkitd
rpc
sssd
rpcuser
chrony
sshd
ec2-user
cockpit-ws
cockpit-wsinstance
setroubleshoot
pcp
test
apache
nginx
[ec2-user@ip-172-31-31-136 ~]$
```
</p>
</details>

💥 Use ```sed``` to replace the username root with 'the administrator'
```
sed 's/root/the administrator/g' /etc/passwd
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ sed 's/root/the administrator/g' /etc/passwd
the administrator:x:0:0:the administrator:/the administrator:/bin/bash
```
</p>
</details>

Next, we are going to add some of these things together. You can send the output of one program to another program by using the symbol ```|``` in the shell. Also called a pipe.

💥 Just print the first line of the /etc/passwd file using head
```
head -1 /etc/passwd
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ head -1 /etc/passwd
root:x:0:0:root:/root:/bin/bash
[ec2-user@ip-172-31-31-136 ~]$
```
</p>
</details>

💥 Just print the last line of the /etc/passwd file using tail
```
tail -1 /etc/passwd
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ tail -1 /etc/passwd
test:x:1001:1001::/home/test:/bin/bash
[ec2-user@ip-172-31-31-136 ~]$ 
```
</p>
</details>

💥 Print the first user which can access a shell and replace the text root with some text.
```
grep bash /etc/passwd|head -1|cut -d: -f1|sed 's/root/the administrator account is configured with a shell/g' 
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ grep bash /etc/passwd|cut -d: -f1|sed 's/root/the administrator account is configured with a shell/g' 
the administrator account is configured with a shell
[ec2-user@ip-172-31-31-136 ~]$ 
```
</p>
</details>

💥 Search for when someone used the sudo command on the system during the past one day
```
journalctl --since "1 day ago"|grep COMMAND
```

💥 Create two files and compare them
```
echo "This is the same" >file1
echo "This is different" >>file1
echo "This is the same" >file2
diff -y file1 file2
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ diff -y file1 file2
This is the same						This is the same
This is different					      <
[ec2-user@ip-172-31-31-136 ~]$
```
</p>
</details>

The ```diff``` command is very powerful, as it allowed us to compare any files with each other, a key trick when we do troubleshooting.


### Editing files
Next thing which we need to know to be able to navigate the Linux operating system is how to edit files.
The most common text editor in Linux is called ```vi```, which we'll have a quick look at.

💥 Create a new file and start editing it using ```vi```
```
vi testscript
```

Expected output:
```
                                                             
~
~
~
~
"testscript" [New File]
```

💥 To start inserting text, press ```i``` which stands for insert. To remove text, use backspace. Insert an echo command.
```
i
```

Expected output:
```
echo "Hello world"
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

If you didn't know, :wq stands for write quit. You you just want to save something and continue editing, just type in :w. If you just want to quick without saving type :q!

💥 Next, let's run the script we created. 
```
./testscript
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ ./testscript
-bash: ./testscript: Permission denied
[ec2-user@ip-172-31-31-136 ~]$ 
```
</p>
</details>

This is what we should have expected, it's because by creating a file, the execution permission is not added automatically.

💥 Add executive rights to our user for the file and rerun the command.
```
chmod u+x testscript
./testscript
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ chmod u+x testscript
[ec2-user@ip-172-31-31-136 ~]$ ./testscript 
Hello world
[ec2-user@ip-172-31-31-136 ~]$
```
</p>
</details>

### Installing software
For the next lab, we will need some additional software to be in place. Software in Red Hat Enteprise Linux is put in packages called RPMs.
RPMs are put in repositories where they are fetched to systems over HTTP, HTTPS or FTP. What repositories you enable on a system is configured in /etc/yum.repos.d where configuration files are dropped. When you then run the ```dnf``` command to install software, the system knows where to get the software.

To view information about installed software, we can also use the ```rpm``` command.

💥 Explore the rpm command
```
man rpm
```

💥 List all install RPMs on the system and sort the output into alfabetic order by piping output to ```sort```
<details>
<summary>I could use some help...</summary>
<p>
  
```
rpm -qa|sort
```
</p>
</details> 

💥 View more information about a specific RPM
<details>
<summary>I could use some help...</summary>
<p>
  
```
rpm -qi zlib
```
</p>
</details>

💥 View the changelog of an RPM
<details>
<summary>I could use some help...</summary>
<p>
  
```
rpm -q zlib --changelog
```
</p>
</details>

💥 View what files an RPM deliver to the system
<details>
<summary>I could use some help...</summary>
<p>
  
 ```
 rpm -ql zlib
 ```
</p>
</details>


💥 Let's have a look at which repositories are configured on our system. Run ```ls``` to list any files inside of the /etc/yum.repos.d directory.
```
ls /etc/yum.repos.d
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ ls /etc/yum.repos.d
redhat-rhui-client-config-ha.repo  redhat-rhui-ha.repo
```
</p>
</details>

💥 Next, let's look into one of those files using ```less```
```
less /etc/yum.repos.d/redhat-rhui-ha.repo
```

In the file, you can see multiple repositories configured for the system. When we now will install software, yum searches through the available repositories until it finds (or not finds) the software we want to install.

💥 Search the available repositories for a software called tree
```
sudo dnf search tree
```

💥 Install tree, a piece of software which allows us to display directory structures more easily.
```
sudo dnf install tree
```

❗ You need to answer yes (y) before the software actually installs.

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ sudo dnf install tree
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.

Last metadata expiration check: 0:00:17 ago on Wed 13 Oct 2021 05:59:55 PM UTC.
Dependencies resolved.
==============================================================================================================================================================================================================================================
 Package                                           Architecture                                        Version                                                     Repository                                                            Size
==============================================================================================================================================================================================================================================
Installing:
 tree                                              x86_64                                              1.7.0-15.el8                                                rhel-8-baseos-rhui-rpms                                               59 k

Transaction Summary
==============================================================================================================================================================================================================================================
Install  1 Package

Total download size: 59 k
Installed size: 109 k
Is this ok [y/N]: y
Downloading Packages:
tree-1.7.0-15.el8.x86_64.rpm                                                                                                                                                                                  750 kB/s |  59 kB     00:00    
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                                                                         498 kB/s |  59 kB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                                                                                      1/1 
  Installing       : tree-1.7.0-15.el8.x86_64                                                                                                                                                                                             1/1 
  Running scriptlet: tree-1.7.0-15.el8.x86_64                                                                                                                                                                                             1/1 
  Verifying        : tree-1.7.0-15.el8.x86_64                                                                                                                                                                                             1/1 
Installed products updated.

Installed:
  tree-1.7.0-15.el8.x86_64                                                                                                                                                                                                                    

Complete!
[ec2-user@ip-172-31-31-136 ~]$ 
```
</p>
</details>


Now you know enough to start exploring Linux, congratulations! 😃

[Go to the next lab, lab 1](../lab-1/README.md)



