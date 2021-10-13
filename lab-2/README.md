# Security features
Now that you have a little better understanding of Linux, we can start exploring the different security features in Linux.
Some of them you have already discovered, like file permissions and encrypted management channels.

The topic of security is vast and is not something easily understood by people who are starting to learn about Linux, but we will try and cover key topics.

### Limiting the attack vector
The more things you have running on a system, the more things can be exploited by bad actors. A first step is to review what is running on your system and disable what is not needed.

💥 Review what software is installed and what services runs on your system and find something which is not required.
```
systemctl list-unit-files
rpm -qa|sort
```

## Enabling SELinux
The normal access control system in Linux is based on permissions on files, which you have experienced. It's a so called discretionary access system (DAC). The owner of a file has the discretion to decide what the access should be. For example, a user can set file permissions so that any user in the system can edit it.
This has a challenge that if the user can been compromised, it can do bad thing. For example, applications runs as a specific user. Sometimes applications runs as priviledged users, even root sometimes. What happens if that application is then compromised in a system with only discretionary access systems? The hacker can then do whatever that user can do. If it's a priviledged user, your system is now fully comprimised.

SELinux is a mandatory access system (MAC) which aims to solve above challenge. SELinux allows you to define what things a specific application should be allowed to do. This is then enforced by the Linux kernel. Even if your web server runs as root, it's still only allowed to do things which a web server needs to do. Which does not include things like creating users or installing software.

💥 Check if SELinux is enabled on your system.
```
getenforce
```

If the command returns ```Enforcing```, that means SELinux is protecting your system.

### Firewalling
Firwalld

### Audit
Auditd/AIDE
