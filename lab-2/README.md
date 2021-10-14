# Security features
Now that you have a little better understanding of Linux, we can start exploring the different security features in Linux.
Some of them you have already discovered, like file permissions and encrypted management channels.

The topic of security is vast and is not something easily understood by people who are starting to learn about Linux, but we will try and cover key topics.

### Limiting the attack vector
The more things you have running on a system, the more things can be exploited by bad actors. A first step is to review what is running on your system and disable what is not needed.

💥 Review what services are running on your system and find something which is not needed.
```
systemctl list-unit-files
```

❗ If you are not sure what you can disable, look below. In this lab, do **not** disable the ```sshd``` service.

<details>
<summary>I could use some help...</summary>
<p>
Here's example of two service which are not needed on the system and can be safely disabled.
  
```      
cloud-init-local.service                   enabled        
cloud-init.service                         enabled     
```
</p>
</details> 

To disable a service, we again use the ```systemctl``` command.

💥 Disable one of the identified services and verify that has been done
<details>
<summary>I could use some help...</summary>
<p>
  
```      
sudo systemctl disable cloud-init --now
systemctl status cloud-init
```
</p>
</details> 

Expected output:
<details>
<summary>Click here to view...</summary>
<p>
  
```      
[ec2-user@ip-172-31-31-136 ~]$ sudo systemctl disable cloud-init.service --now
Removed /etc/systemd/system/cloud-init.target.wants/cloud-init.service.
[ec2-user@ip-172-31-31-136 ~]$ systemctl status cloud-init
● cloud-init.service - Initial cloud-init job (metadata service crawler)
   Loaded: loaded (/usr/lib/systemd/system/cloud-init.service; disabled; vendor preset: disabled)
   Active: inactive (dead) since Thu 2021-10-14 10:07:55 UTC; 49s ago
 Main PID: 947 (code=exited, status=0/SUCCESS)

Oct 11 07:46:13 ip-172-31-31-136.eu-central-1.compute.internal cloud-init[947]: ci-info: +-------+-------------+---------+-----------+-------+
Oct 11 07:46:14 ip-172-31-31-136.eu-central-1.compute.internal useradd[1178]: new group: name=ec2-user, GID=1000
Oct 11 07:46:14 ip-172-31-31-136.eu-central-1.compute.internal useradd[1178]: new user: name=ec2-user, UID=1000, GID=1000, home=/home/ec2-user, shell=/bin/bash
Oct 11 07:46:14 ip-172-31-31-136.eu-central-1.compute.internal useradd[1178]: add 'ec2-user' to group 'adm'
Oct 11 07:46:14 ip-172-31-31-136.eu-central-1.compute.internal useradd[1178]: add 'ec2-user' to group 'systemd-journal'
Oct 11 07:46:14 ip-172-31-31-136.eu-central-1.compute.internal useradd[1178]: add 'ec2-user' to shadow group 'adm'
Oct 11 07:46:14 ip-172-31-31-136.eu-central-1.compute.internal useradd[1178]: add 'ec2-user' to shadow group 'systemd-journal'
Oct 11 07:46:14 ip-172-31-31-136.eu-central-1.compute.internal systemd[1]: Started Initial cloud-init job (metadata service crawler).
Oct 14 10:07:55 ip-172-31-31-136.eu-central-1.compute.internal systemd[1]: cloud-init.service: Succeeded.
Oct 14 10:07:55 ip-172-31-31-136.eu-central-1.compute.internal systemd[1]: Stopped Initial cloud-init job (metadata service crawler).
[ec2-user@ip-172-31-31-136 ~]$ 

```
</p>
</details> 

Limit of what's being run on a system would normally be done at install time. Meaning we adjust installation images or templates to not include what's not needed from the start. Applying hardening retroactively is more challenging as that requires re-testing and more knowledge to ensure that the environment doesn't break as we harden it.

## Enabling SELinux
The normal access control system in Linux is based on permissions on files, which you have experienced. It's a so called discretionary access system (DAC). The owner of a file has the discretion to decide what the access should be. For example, a user can set file permissions so that any user in the system can edit it.
This has a challenge that if the user can been compromised, it can do bad thing. For example, applications runs as a specific user. Sometimes applications runs as priviledged users, even root sometimes. What happens if that application is then compromised in a system with only discretionary access systems? The hacker can then do whatever that user can do. If it's a priviledged user, your system is now fully comprimised.

SELinux is a mandatory access system (MAC) which aims to solve above challenge. SELinux allows you to define what things a specific application should be allowed to do. This is then enforced by the Linux kernel. Even if your web server runs as root, it's still only allowed to do things which a web server needs to do. Which does not include things like creating users or installing software.

💥 Check if SELinux is enabled on your system.
```
getenforce
```

If the command returns ```Enforcing```, that means SELinux is protecting your system.

In order to learn a bit about how SELinux is protecting the system, we'll demonstrate what happens when a web application get's hacked, with and without SELinux.
In order to do this, we will a web server and run a php application which provides a shell to the system, this emulates a successful compromise to the system.

💥 Disable the web console, as we'll be using the same port is it.
```
sudo systemctl stop cockpit.socket
```

💥 Install Apache web server
```
sudo dnf install httpd
```

💥 Enable the web server
```
sudo systemctl enable httpd --now
```

💥 Download the php application
```
cd
curl http://hacka.net/files/phpshell-2.6-demo.tar.gz >phpshell.tar.gz
```

💥 Unzip the zip archive
```
tar xvf phpshell.tar.gz
```

💥 Move the directory to the web servers cgi directory
```
mv phpshell-2.6 /var/www/cgi-bin/
```

💥 Access the application via http://yoursystem/cgi-bin/phpshell-2.6/phpshell.php
```
Login using user: testuser
Password: redhat123
```

We are now going to pretend that we are an attacker who gained shell access via our web application and who is now starting to explore the system.

💥 List all processes in the system
```
ps -ef
```

Expected output:
```
$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 Oct11 ?        00:00:37 /usr/lib/systemd/systemd --s
ec2-user  112572  112562  0 08:11 ?        00:00:00 (sd-pam)
root      114264       1  0 10:19 ?        00:00:00 php-fpm: master process (/et
root      114265       1  0 10:19 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache    114266  114264  0 10:19 ?        00:00:00 php-fpm: pool www
apache    114267  114264  0 10:19 ?        00:00:00 php-fpm: pool www
apache    114268  114264  0 10:19 ?        00:00:00 php-fpm: pool www
apache    114269  114264  0 10:19 ?        00:00:00 php-fpm: pool www
apache    114270  114264  0 10:19 ?        00:00:00 php-fpm: pool www
apache    114271  114265  0 10:19 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache    114272  114265  0 10:19 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache    114273  114265  0 10:19 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache    114274  114265  0 10:19 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache    114489  114265  0 10:20 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache    114584  114264  0 10:22 ?        00:00:00 php-fpm: pool www
apache    114588  114264  0 10:22 ?        00:00:00 php-fpm: pool www
apache    115499  114584  0 10:45 ?        00:00:00 sh -c export ROWS=24 export 
apache    115500  115499  0 10:45 ?        00:00:00 ps -ef
$
```

Please note that what we are seeing here are not all the processes in the system. This is due to SELinux enforcing more granular access controls on the Apache webserver. Apache does not need to see all processes in the system, so it's not allowed. This, even though the apache user, can see them.

💥 List all installed RPMs on the system
```
rpm -qa
```

Expected output:
```
$ rpm -qa
error: cannot open Packages index using db5 - Permission denied (13)
error: cannot open Packages database in /var/lib/rpm
error: cannot open Packages index using db5 - Permission denied (13)
error: cannot open Packages database in /var/lib/rpm
```

💥 Have a look at the kernel ring buffer
```
dmesg
```

Expected output:
```
$ dmesg
dmesg: read kernel buffer failed: Permission denied
```

💥 Now we are going to disable SELinux on the system. **Never** do this on an actual system. If anyone questions this, it likely means that they do not understand what they are talking about.
```
sudo setenforce 0
```

💥 Now let's run the same commands (```ps -ef```, ```rpm -qa```, ```dmesg``` again, on our phpshell applications.

Expected result:
* You see all processes
* You can list all RPMs
* You can access the kernel's ring buffer

💥 Enable SELinux again and verify that it's set to ```Enforcing```
```
sudo setenforce 1
getenforce
```

Now that you know why to never run Linux without SELinux enabled, you are good to go to the next section.

### Firewalling
Firwalld

### Audit
Auditd/AIDE
