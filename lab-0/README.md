# Navigating in Linux

In this lab, you'll learn how to navigate the filesystem in Linux. It's vital in order to find, view, create and edit files.

## Your terminal and the shell

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
[ec2-user@ip-172-31-31-136 ~]$ reminder="I should go for a an hours walk every day"
```

💥 Next, we are going to fetch the value of the variable and print it out on the screen. We do that by running the command ```echo```, which prints things to the screen.

Run below commands:
```
echo $reminder
```

Expected output:
```
[ec2-user@ip-172-31-31-136 ~]$ echo $reminder
I should go for a an hours walk every day
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
cat ~/.bashrc
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

### list and change directory
The two most common tool used in any Linux distribution are ```ls``` and ```cd```. They help you navigate around in the operating system.
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

💥 Access the manual pages for the ls command.

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

Now we know how to find hidden files.



