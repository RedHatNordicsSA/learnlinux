# Security features
Now that you have a little better understanding of Linux, we can start exploring the different security features in Linux.
Some of them you have already discovered, like file permissions and encrypted management channels.

The topic of security is vast and is not something easily understood by people who are starting to learn about Linux, but we will try and cover key topics.

### Keeping your system updated
The most difficult challenge is not how to apply security hardening on systems, it's how to keep your systems updated.
To update our system in Red Hat Enterprise Linux, we simply run the below command:
```
sudo dnf update
```

Keeping your system updated though, is mainly about three things.
* Testing
* Planning
* Automation

#### Testing
In a perfect world, no software vendor would have bugs, in reality, all software is in different broken states, because software is written by humans.
So we need to have test environments, where we with less impact on the business can test out updates. A good way to start and approach testing is to start and focus on key features. 

How do you in one simple test prove that most of the functionality in your Linux system works? Perhaps by connecting to the system via SSH and then running ```ls```. That proves that large parts of the functionality we depend on, such as networking, storage and the operating system, is indeed working.

If you do not test, then you are rolling a dice and that will make it difficult to create suffient trust in the organisation to be allowed to update systems on a regular basis (I don't mean once per year now...).

👍 The best way to reduce the need of testing, is by running solutions which has forward compatibility. Red Hat Enterprise Linux is the only Linux operating system which provides solid forward compatibility. Simplified, this is done by backporting bug fixes, security updates and new features into software versions that was selected for a new major release of the operating system. This significantly reduces the need to do testing. We at Red Hat have customers who in diverse environments with thousands of different applications patches systems hundreds of thousands of times in a year, without business critical business outage. That is valuable and would very difficult without forward compatibility.

⭐ Read more about the Red Hat Enterprise Linux ABI (forward compatibility) here: https://access.redhat.com/articles/rhel8-abi-compatibility

#### Planning
In a perfect world, it's always possible to take systems offline to patch them, in reality, it's very common that requirements on availability and application architecture is a complete mismatch. When we talk about availability, there are two terms, which often are misused. High availability, which means that there is some degree of redundancy and full diversity, which means that every single function runs in multiple places and there is no single point of failure. Full diversity is very uncommon to see outside of Telco and some Government organizations. The reason for that is that everything needs to be replicated, applications, load balancers, security functions, servers, server rooms, data centers in different cities, different internet providers which provides connections via different black fiber, and so on.

👍 As reality commonly is that we only have some degree of reundancy, we need to plan for when we have to take things down. A common solution is distributed application architecture, such as microservices. If you have five instances of a web front end and you can take down one, upgrade it and then join it back to the group of live systems, we can decrease the impact updating has on our system.

👍 Another best practice is to talk to and collaborate with the people who affects your life cycle. If you depend on X to run your things, ensure you know what the future life cycle of X looks like.

#### Automation
In a perfect world, all applications are delivered with enough automation that they manages themselfs, in reality, few does. It's common that a piece of software comes with a manual, instead of a script and automatics. This means that doign things like testing and updating systems and software requires humans to follow documentation and enter information into terminals and click on buttons, which does not scale. It means that we cannot update as often, as humans with skills and experience are limited resources in most companies.

👍 In order to get things into an automated state, most importantly, we need an automation strategy. Automation will not reach a sufficient state if the company is relying on good will and individual or team initiatives. To read about the key components of an automation strategy worth it's paper, see here: https://www.linkedin.com/pulse/what-serious-automation-strategy-looks-like-magnus-glantz/

👍 If you are impatient and want to start with automation on Linux today, why not try out the most popular automation framework today, Ansible. Learn more here: https://www.ansible.com/overview/how-ansible-works

### Limiting the attack vector
The more things you have running on a system, the more things can be exploited by bad actors. A first step is to review what is running on your system and disable what is not needed.

💥 Review what services are running on your system and find something which is not needed.
```
systemctl list-unit-files
rpm -qa
netstat -tulpn
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
In order to do this, we will install a web server and run a php application which provides a shell to the system, this emulates a successful compromise of a web application on this system.

💥 Disable the web console, as we'll be using the same port it uses.
```
sudo systemctl stop cockpit.socket
```

💥 Install Apache web server
```
sudo dnf install httpd php
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
sudo mv phpshell-2.6 /var/www/cgi-bin/
```

💥 Adjust the SELinux context for what we copied in
```
sudo restorecon -Rv /var/www/cgi-bin
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
<details>
<summary>Show</summary>
<p>
  
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
</p>
</details> 

Please note that what we are seeing here are not all the processes in the system. This is due to SELinux enforcing more granular access controls on the Apache webserver. Apache does not need to see all processes in the system, so it's not allowed. This, even though the apache user, can see them.

💥 List all installed RPMs on the system
```
rpm -qa
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
$ rpm -qa
error: cannot open Packages index using db5 - Permission denied (13)
error: cannot open Packages database in /var/lib/rpm
error: cannot open Packages index using db5 - Permission denied (13)
error: cannot open Packages database in /var/lib/rpm
```
</p>
</details> 

💥 Have a look at the kernel ring buffer
```
dmesg
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
$ dmesg
dmesg: read kernel buffer failed: Permission denied
```
</p>
</details> 

💥 Now we are going to disable SELinux on the system. **Never** do this on an actual system. If anyone questions this, it likely means that they do not understand what they are talking about.
```
sudo setenforce 0
```

💥 Now let's run the same commands (```ps -ef```, ```rpm -qa```, ```dmesg```) again, on our phpshell application.

Expected result:
* You see all processes
* You can list all RPMs
* You can access the kernel's ring buffer

💥 Enable SELinux again and verify that it's set to ```Enforcing```
```
sudo setenforce 1
getenforce
```

💥 Disable the web server
```
sudo systemctl disable httpd --now
```

💥 Enable the web console
```
sudo systemctl start cockpit.socket
```

💥 Either run below command or go to https://yoursystem and access the web console, once there, go to the SELinux tab and further review things SELinux prevented access to.
```
sudo ausearch -m AVC -i 
```

Now that you know why to never run Linux without SELinux enabled, you are good to go to the next section.

### Firewalling
Good security is built like an onion, in layers. Just because you have firewalls in your network, doesn't mean that you shouldn't do filtering on the host level as well. Network firewalls often only filter some of the traffic and often not traffic which happens in the local networks. 

Red Hat Enterprise Linux comes with very capable network filtering features. It also provides a daemon which helps manage your local firewall rules, called firewalld. Let's explore it.

💥 First, install firewalld.
```
sudo dnf install firewalld
```

💥 Enable and start running it
```
sudo systemctl enable firewalld --now
```

💥 Next step is to verify that it's running. We'll use firewalld's cli to do this.
```
sudo firewall-cmd --state
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ sudo firewall-cmd --state
running
[ec2-user@ip-172-31-31-136 ~]$
```
</p>
</details> 

💥 Try to access the web console of your system and see what happens.

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
This site can’t be reached
https://ec2-18-195-46-93.eu-central-1.compute.amazonaws.com/ is unreachable.
ERR_ADDRESS_UNREACHABLE
```
</p>
</details>  

The reason why we haven't also been kicked out of the system is because already established connections and the ```sshd``` service is allowed by default.

💥 Review the current rules of the system.
```
sudo firewall-cmd --list-all 
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: eth0
  sources: 
  services: cockpit dhcpv6-client ssh
  ports: 
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
[ec2-user@ip-172-31-31-136 ~]$ 
```
</p>
</details> 

Let's add https. Here's we use the services concept which firewalld has, where we can enable a service and no have to think about what ports and protocols are in use.

💥 List available service in firewalld
```
firewall-cmd --get-services
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ firewall-cmd --get-services
RH-Satellite-6 amanda-client amanda-k5-client amqp amqps apcupsd audit bacula bacula-client bb bgp bitcoin bitcoin-rpc bitcoin-testnet bitcoin-testnet-rpc bittorrent-lsd ceph ceph-mon cfengine cockpit collectd condor-collector ctdb dhcp dhcpv6 dhcpv6-client distcc dns dns-over-tls docker-registry docker-swarm dropbox-lansync elasticsearch etcd-client etcd-server finger freeipa-4 freeipa-ldap freeipa-ldaps freeipa-replication freeipa-trust ftp galera ganglia-client ganglia-master git grafana gre high-availability http https imap imaps ipp ipp-client ipsec irc ircs iscsi-target isns jenkins kadmin kdeconnect kerberos kibana klogin kpasswd kprop kshell kube-apiserver ldap ldaps libvirt libvirt-tls lightning-network llmnr managesieve matrix mdns memcache minidlna mongodb mosh mountd mqtt mqtt-tls ms-wbt mssql murmur mysql nfs nfs3 nmea-0183 nrpe ntp nut openvpn ovirt-imageio ovirt-storageconsole ovirt-vmconsole plex pmcd pmproxy pmwebapi pmwebapis pop3 pop3s postgresql privoxy prometheus proxy-dhcp ptp pulseaudio puppetmaster quassel radius rdp redis redis-sentinel rpc-bind rquotad rsh rsyncd rtsp salt-master samba samba-client samba-dc sane sip sips slp smtp smtp-submission smtps snmp snmptrap spideroak-lansync spotify-sync squid ssdp ssh steam-streaming svdrp svn syncthing syncthing-gui synergy syslog syslog-tls telnet tentacle tftp tftp-client tile38 tinc tor-socks transmission-client upnp-client vdsm vnc-server wbem-http wbem-https wsman wsmans xdmcp xmpp-bosh xmpp-client xmpp-local xmpp-server zabbix-agent zabbix-server
[ec2-user@ip-172-31-31-136 ~]$ 
```
</p>
</details> 

Here we can see https listed, good.

💥 Add an opening in the firewall for https
```
sudo firewall-cmd --zone=public --add-service=https
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ sudo firewall-cmd --zone=public --add-service=https
success
[ec2-user@ip-172-31-31-136 ~]$ 
```
</p>
</details> 

💥 Now refresh the web console web page and verify that you again have access.

Once we have verified we have access, we can now add the firewall rule as a permanent rule which does not go away after reboot.
```
sudo firewall-cmd --zone=public --add-service=https --permanent
```

Expected output:
<details>
<summary>Show</summary>
<p>
  
```
[ec2-user@ip-172-31-31-136 ~]$ sudo firewall-cmd --zone=public --add-service=https --permanent
success
[ec2-user@ip-172-31-31-136 ~]$ 
```
</p>
</details> 

### Cryptographic hardening
At any time, there are a large number of applications runnning on a system. A main concern is what type of cryptographic algoritms are used in those applications.
The field of cryptography is not static, new attacks are constantly invented. The cryptographic algorithms from a number of years ago may no longer be keeping your data safe. The challenge is to know what algorithms are safe and how to make applications used those safe algorithms.

The solution to this challenge in Red Hat Enterprise Linux is the system-wide cryptographic policy.
Once a system-wide policy is set up, applications in RHEL follow it and refuse to use algorithms and protocols that do not meet the policy, unless you explicitly request the application to do so.

It's now time to stop following this guide blindly 😅 and get some more information from a more complete source. For security that is the Red Hat Enterprise Linux Security Hardening guide.

❗ You will be asked to try out some examples outlined in the documentation, but **DO NOT** reboot your system as that will cause you to loose access to it **for ever**.

💥 Have a a look at below chapters and try out the examples described in them. You can find the documentation here: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/security_hardening/index#using-the-system-wide-cryptographic-policies_security-hardening

💥 Go through the below chapters:
```
5.1. System-wide cryptographic policies
5.2. Switching the system-wide cryptographic policy to mode compatible with earlier releases
5.3. Switching the system to FIPS mode
```

### Auditing
You have already learned a bit about auditing log files. In this section we'll introduce two more tools which allows you to audit what's happening in the system.
Using no additional audit tools, we are often in the hands on what an application or the operating system deems suitable to log. In more security sensitive environments, we often want to log more. After a system has been attacked, it's critical that we learn what functions and data was compromised, for this, there are two main tools, ```auditd``` and ```AIDE```.


#### Auditd
To learn about how to setup and use the main auditing tool in Linux, we're again going to use the official Security Hardening guide for Red Hat Enterprise Linux.

💥 Have a look in the official Red Hat Enterprise Linux Security Hardening guide to learn about how to install and configure Auditd. It's found here: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/security_hardening/index#linux-audit_auditing-the-system

💥 Go through and try out the examples shown in the below chapters:
```
13.1. Linux Audit
13.2. Audit system architecture
13.3. Configuring auditd for a secure environment
13.3. Configuring auditd for a secure environment
13.5. Understanding Audit log files
13.6. Using auditctl for defining and executing Audit rules
13.7. Defining persistent Audit rules
13.8. Using pre-configured rules files
```

#### AIDE
AIDE or Advanced Intrusion Detection Environment is a tool which can help you detect intrusions. It does this by generating a database of the files on your system so that it can detect if any of them was tampered with or changed in any way.

💥 Again, use the Red Hat Enterprise Linux Security Hardening guide to learn how to use AIDE. The chapter on AIDE is found here: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/security_hardening/index#checking-integrity-with-aide_security-hardening

💥 Go through and try out the examples shown in the below chapters:
```
10.1. Installing AIDE
10.2. Performing integrity checks with AIDE
10.3. Updating an AIDE database
```

Congratulations, you've are now better equipped to understand security in Linux. 😃

Next chapter is about troubleshooting.
[Go to the next lab, lab 3](../lab-3/README.md) 

