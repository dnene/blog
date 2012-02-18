---
date: '2009-10-05 19:26:20'
layout: post
slug: configuring-a-secure-ubuntu-linux-virtual-private-server
status: publish
title: Configuring a secure Ubuntu Linux Virtual Private Server
wordpress_id: '852'
categories:
- linux
- security
tags:
- linux
- security
- ubuntu
- vps
---

This post is based on my notes for an initial configuration for an Ubuntu 9.04 Virtual Private Server with a focus on security. At that time I searched for a number of references on security, and while I have not kept the note of all their URLs, most of what I write below is as a result of other documents even though I cannot specifically cite them (in other words, there is little originality except perhaps for attempting to cover the entire gamut of configuration activities into one article).

Keep in mind that these steps are based on my notes which might be a little incomplete especially around the part where acidbase is installed.




	
  * Initial Configuration : Basic configuration when getting started up.

	
  * Setting up basic security : Basic Security configuration

	
  * Setting up rootkit detection : Setting up rootkit detection

	
  * Setting up Bastille : Setting up Bastille

	
  * Setting up the lamp stack : Set up the LAMP stack including mysql and apache

	
  * Setting up Snort and acidbase : Configure Intrusion detection using Snort and Acidbase.

	
  * Setting up File Integrity using AIDE  : Setting up File integrity checks using AIDE







##  Initial Configuration



These steps cover the initial setup of a server



###  Setup the hostname 



Lets say the hostname we want to setup is _vps_.


``` bash
$ echo "vps.mydomain.com" > /etc/hostname
$ hostname -F /etc/hostname
```


Now update the _/etc/hosts_ file to reflect the hostname and the fully qualified domain name
_Replace 12.34.56.78 with the IP address of your host_


```
127.0.0.1       localhost.localdomain  localhost
12.34.56.78     vps.mydomain.com vps
```





###  Updating the ubuntu repositories 


You will need to update your ubuntu repositories to include jaunty-updates and universe repositories. This is so that you may install additional packages as required from these repositories as well. In my case, the earlier version of the file _/etc/apt/sources.list_ was as follows.

However please note, that repository selection and its update strategy may be linked to your company or application strategy. Please make sure these steps are consistent with your policy. If not, kindly adapt consistent with your team / organisations policy. Also instead of us.archive.ubuntu.com, you may find other country specific server names. In that case you may want to continue to use the other server name as already listed in your file.



```
deb http://us.archive.ubuntu.com/ubuntu/ jaunty main restricted
deb-src http://us.archive.ubuntu.com/ubuntu/ jaunty main restricted

deb http://security.ubuntu.com/ubuntu jaunty-security main restricted
deb-src http://security.ubuntu.com/ubuntu jaunty-security main restricted
```


Upon adding jaunty-updates and the universe repositories, the resultant file is as follows.


```
deb http://us.archive.ubuntu.com/ubuntu/ jaunty main restricted universe
deb-src http://us.archive.ubuntu.com/ubuntu/ jaunty main restricted universe

deb http://security.ubuntu.com/ubuntu jaunty-security main restricted universe
deb-src http://security.ubuntu.com/ubuntu jaunty-security main restricted universe

deb http://us.archive.ubuntu.com/ubuntu/ jaunty-updates main restricted universe
deb-src http://us.archive.ubuntu.com/ubuntu/ jaunty-updates main restricted universe
```



Now update the sources. This will scan all the repositories



```
$ sudo apt-get update
```



Finally upgrade ie. replace any existing packages which have a newer upgrade



```
$ sudo apt-get upgrade
```





###  Download the language pack 



To add the necessary for the preferred language of your choice add the appropriate language pack. In my case I add support for english (en)



```
$ sudo apt-get install language-pack-en
```





###  Set the timezone 



Set the timezone of the server. You may choose to set it based on server location, or typical user location or to UTC.


```
$ dpkg-reconfigure tzdata
```


That will start a small app, from which you can select the timezone. I selected _None of the Above_ which offered me a choice of timezones based on UTC offsets and subsequently selected _UTC_.



###  Setting up Mail sending 



I do not need this VPS to act as a mail server. However I do need to have capabilities to send email from this machine. Many unix tools routinely assume the existence of sendmail or equivalent MTA. However that is an overkill in this context. So we shall not be installing sendmail or postfix or exim or any other equivalent. Instead we shall configure this server to be only able to send out mail using an SMTP account on another mail server. For this we shall install a tool called mailx. _Note: If you have mailx already installed through another ubuntu package called mailutils, you may either continue with the same (in which case you will need to configure the remainder of the mail stack correspondingly eg. sendmail) or remove mailutils and add heirloom-mailx_


```
$ sudo apt-get install heirloom-mailx
```


We shall also configure a global configuration for sending out mail. In my case its all right to always send mail using only one account irrespective of the process or user who is sending it.


```
$ sudo vi /etc/nail.rc
```



Note that in the above configuration, we shall be placing the mail account password in clear text. Make sure it is a mail account you do not use for any other purposes and that its password is not the same as used for any other purposes. Now enter the following as contents of the _/etc/nail.rc_ file. Obviously change the relevant fields to appropriate values. Note that this file is configured for sending mail via gmail. You may need to configure it differently based on your own SMTP configurations.



```
set smtp-use-starttls
set from=my_user_id@gmail.com
set smtp=smtp.gmail.com:587
set smtp-auth-user=my_user_id@gmail.com
set auth-login=my_user_id@gmail.com
set smtp-auth-password=my_password
```




You can try testing whether this got set up successfully. Enter the following (replace youremailid@youremaildomain.com by the email id where you would like the mail to be sent to)



```
$ mail youremailid@youremaildomain.com
Subject: This is a test mail
Hello
.
```






## Basic Security


In this section we shall make some basic configuration changes with a view to enhance the system security.



###  Mounting the shared memory as read only 



Open and edit the file /etc/fstab to add an entry to mount shared memory in read only mode. The reason we do it is because many exploits use shared memory to attack other running services.

If you have a good reason to make shared memory writeable skip this step.


```
$ vi /etc/fstab
```


Now add the following line at the end of the file


```
tmpfs           /dev/shm        tmpfs   defaults,ro     0       0
```





###  Tightening the passwords 



One of the easiest exploits is to attempt a brute force login using dictionary based attacks. In order to ensure strong ie. non-guessable passwords we shall update the password checking policy so that it allows only strong passwords. A simple way to ensure that is to ensure a reasonable minimum length and to ensure multiple character classes.

First lets install a new pam authentication module pam_cracklib. To install the same run the following


```
$ sudo apt-get install libpam-cracklib
```


Answer 'Y' to the prompt it asks for regarding continuing.

_Note: if you did not add the universe repository to your sources.list file, you will not be able to install libpam-cracklib. In that case you will need to skip this step._

This should've resulted in the file _etc/pam.d/common-password_ having an entry for pam_cracklib.so and pam_unix.so. Update the pam_cracklib.so entry to add one more requirement ie. _minclass=4_.

In my case, the resultant two lines in _/etc/pam.d/common-password_ are as follows. Note that I added the minclass=4 clause manually.


```
password        requisite                       pam_cracklib.so retry=3 minlen=8 difok=3 minclass=4
password        [success=1 default=ignore]      pam_unix.so obscure use_authtok try_first_pass sha512
```



There. You now have a strong password scheme which will conduct a whole range of password checks in addition to ensuring that the password has a minimum length of 8 and each new password has at least one each of the four character classes. The four character classes are lower_case, upper_case, digit and special_characters (the last one being any non alpha-numeric character)



###  Creating the first user 


Note: if you have already created at least one more non root user this step is not required. We are primarily creating the new user so that we shall eventually allow sudo and remote ssh login privileges to the user and disable remote ssh privileges for the root user.

Setup the first new user. One of the reasons you should create a new user is so that it will afford you the ability to allow him to perform root actions through sudo, and thus subsequently allow you to disable root access over ssh. By default when one creates a new user, another group gets created with the same name as well. In this case we shall create a new group "_dev_" and then create a new user associated with that group "_someuser_". Use the groupname and the username as you would like to setup when executing the commands below. In the commands below we create a new home directory for the user, associate the _/bin/bash_ shell with his account instead of the default _/bin/sh_, (I just prefer bash to the plain sh) and finally set the password for him.



```
$ groupadd dev
$ mkdir /home/someuser
$ useradd -d /home/someuser -s /bin/bash -g dev someuser
$ chown someuser.dev /home/someuser
$ passwd someuser
```



We shall also create the .ssh directory for the user which we shall be using later


```
$ mkdir /home/someuser/.ssh
$ chmod 700 /home/someuser/.ssh
$ touch /home/someuser/.ssh/authorized_keys
$ chmod 600 /home/someuser/.ssh/authorized_keys
$ chown -R someuser.dev /home/someuser
```



Now we shall create the keypair for the user to log in to the host remotely. Note that if you are going to do this for multiple users, then you might want to have each user run the next step locally and then copy over his public key onto the server before continuing to the ssh tightening operations described later.

The user should do the following on his _local workstation from which he is most frequently likely to connect to the server (not the server that we are hardening)_.

_Note: the part after -C in ssh-keygen is just a comment to identify the keys - enter something to identify the user and his machine._
Also make sure not to keep the passphrase blank though ssh-keygen will allow a blank passphrase. The reason is that if the user's local machine is compromised the attacker can then get an easy access to the server being hardened.

_change someuser and some.host.com below based on the user id and vps name correspondingly_


```
$ mkdir ~/.ssh
$ ssh-keygen -t dsa -b 1024 -C "some user on his desktop"
$ scp ~/.ssh/id_dsa.pub someuser@some.host.com:/home/someuser/.ssh/someuser.pub
```



Now the user should himself ssh to the remote server and on the remote server move his public key into the _authorized_keys_ file. So execute the following command after being connected to the VPS



```
$ cd .ssh
$ cat someuser.pub >> authorized_keys
$ rm someuser.pub
```



At this stage the user can disconnect from the VPS and attempt to reconnect using ssh. If all works well, he should get connected to the vps in a manner where it does not prompt him for a password but instead he does get prompted for the passphrase to his private key (assuming he did set one).

This stage of updating the authorized_key file can also be performed by an administrative user / root once we later reconfigure ssh to only allow public key based logins.



###  Enabling the user to perform sudo operations 


We shall enable any group who belongs to the group '_admin_' to be able to conduct root operations through using sudo. 

First create a group '_admin_'. subsequently associate the user with that group as well. Note : For best security ensure you allow associate only a very small number of users with the 'admin' group since that will effectively allow them control over the whole machine (assuming you setup the privileges as I subsequently describe below).


```
$ groupadd admin
$ adduser someuser admin
```



Now we shall enable any user who belongs to the admin group to perform root actions by using sudo. To edit the sudo policy file do the following


```
$ sudo visudo
```



At the end of the file which is now opened up - add the following line


```
%admin ALL=(ALL) ALL
```



Note this grants all superuser privileges to the users who belongs to admin group when conducting operations using sudo. You can use the sudo policy configurations to set up far more fine grained set of privileges, but thats beyond the scope of this document.

To test whether the configuration worked successfully, you can login as _someuser_ and execute the following command.



```
$ sudo cat /etc/shadow
```





###  Tightening up ssh 


To create the group and associate the users with them perform the following command (use the appropriate username instead of someuser for each user who you would like to allow SSH access).


```
sudo addgroup sshlogin
sudo adduser someuser sshlogin
```



Now that we have at least one user id which can conduct root operations using _sudo_, its time to disable root login from ssh. Go open the file _/etc/ssh/sshd_config_. Make the following changes

This one disables root login for ssh.
_PermitRootLogin yes_ to _PermitRootLogin no_

Adding the following line allows only users of a particular group to login. _Note: if your user count is small, you could use _AllowUsers_ instead_. We shall be separately creating the group and associating the users with the group.
_AllowGroups sshlogin_

This change reduces the login grace time (time before a user needs to login after making the ssh login request). This is to allow better protection to sshd against DOS or brute force attacks.
_LoginGraceTime 20_ to _LoginGraceTime 20_

This changes the port number that _sshd_ listens on from the default 22. Changing the default port takes away the ability of an attacker to easily guess the port on which to attempt to penetrate the system via ssh.
_Port 22_ to _Port 9999_ (or any other suitable number) 

This change disables X11Forwarding over the SSH connection. It will result in you not being able to run X11 GUI applications remotely. If you need that flexibility, do not make this change
_X11Forwarding yes_ to _X11Forwarding no_

The next change disables password authentication (thus allowing only users with their public key being stored in the corresponding authorized_keys folder to connect using ssh). Thus passwords, which need to be transferred to the server in clear text no longer are a valid authentication mechanism. The only available choice is public key based authentication.
_PasswordAuthentication yes_ to _PasswordAuthentication No_

The next change disables password authentication (thus allowing only users with their public key being stored in the corresponding authorized_keys folder to connect using ssh). Thus passwords, which need to be transferred to the server in clear text no longer are a valid authentication mechanism. The only available choice is public key based authentication.
_PasswordAuthentication yes_ to _PasswordAuthentication No_
_UsePAM yes_ to _UsePAM no_

The following change of uncommenting the banner allows a welcome message (not really a welcome one :) ) to be displayed to SSH logins. It does not improve any aspect of the physical security but is more from a legal notice perspective.
_#Banner /etc/issue.net_ to _Banner /etc/issue.net_

Now go update the contents of the file _/etc/issue.net_ to something similar to following


```
***************************************************************************
                                                        NOTICE TO USERS


This computer system is the private property of its owner, whether
individual, corporate or government.  It is for authorized use only.
Users (authorized or unauthorized) have no explicit or implicit
expectation of privacy.

Any or all uses of this system and all files on this system may be
intercepted, monitored, recorded, copied, audited, inspected, and
disclosed to your employer, to authorized site, government, and law
enforcement personnel, as well as authorized officials of government
agencies, both domestic and foreign.

By using this system, the user consents to such interception, monitoring,
recording, copying, auditing, inspection, and disclosure at the
discretion of such personnel or officials.  Unauthorized or improper use
of this system may result in civil and criminal penalties and
administrative or disciplinary action, as appropriate. By continuing to
use this system you indicate your awareness of and consent to these terms
and conditions of use. LOG OFF IMMEDIATELY if you do not agree to the
conditions stated in this warning.
****************************************************************************
```



Restart the ssh daemon and you will no longer be able to connect as root over ssh directly.


```
$ sudo /etc/init.d/ssh restart
```





###  Disabling su 



As per the policy being followed, only users belonging to the '_admin_' group will have any special privileges. For all other users, we shall attempt to lock down the system to the extent feasible. As an early step we shall disable the '_su_' (switch user) command for all but those users belonging to the admin group. _Note : since this disables sudo for a brief duration (between the commands), first switch over as a root before executing these commands_


```
$ sudo su -
$ chown root.admin /bin/su /usr/bin/sudo
$ chmod 04750 /bin/su
$ chmod 04550 /usr/bin/sudo
```






##  Install rootkit detection 



Install chkrootkit



```
$ sudo apt-get install chkrootkit
```



Now create this file _/etc/cron.daily/chkrootkit.sh_ and enter the following contents (replace sever.domain.com with the servername and emailid where you would like the report to be sent to).
Since the mail is rather verbose, and may lead to useful alerts getting ignored, I've removed the lines with the common messages. You may change or not use the grep commands as per your preference.



```
#!/bin/bash
/usr/sbin/chkrootkit | \
grep -v "not found" | \
grep -v "nothing found" | \
grep -v "not infected" | \
mail -s "Daily chkrootkit from server.domain.com" mailid@destination.com
```



Allow execute permissions on the file, and test once by running .. you should get a mail with the report.



```
$ sudo chmod 755 /etc/cron.daily/chkrootkit.sh
$ sudo /etc/cron.daily/chkrootkit.sh
```






##  Installing Bastille 



Bastille is a comprehensive package for security configuration.

** Bastille Bug? and workaround **

_You may need to perform this workaround if your _/etc/debian_version_ contains 4.0 and the current ubuntu bastille package has not resolved the issue. Ideally one should just need to install bastille from the ubuntu repositories_

On Ubuntu 9.0.4 (jaunty) as of the day this document was written, I received the following error
~& ERROR:   'DB5.0' is not a supported operating system.

I've had a difficulty to install Bastille on Ubuntu 9.0.4. This is due to the fact that Bastille does not support the debian version number in _/etc/debian_version_. As per Bastille bug reports this got fixed in 3.0.8, but did not work for me in Bastille 3.0.9. Hence the following is a work around to install bastille.

This workaround involves downloading lynx to access internet, downloading the debian package directly, and then installing the debian package. When you reach the web page it will show you a list of mirrors. Download the .deb package from one of the mirrors



```
$ sudo apt-get install lynx libcurses-perl
$ lynx http://packages.debian.org/squeeze/all/bastille/download
# Download the package using lynx
$ sudo dpkg --install bastille_3.0.9-12_all.deb
```



Install psad which we shall need later and perl-gtk which we shall need to configure bastille with the following command


```
$ sudo apt-get install psad
```



You will notice an error message :

To resolve the same execute the following :



```
$ echo -e 'kern.info\t|/var/lib/psad/psadfifo' | sudo tee -a /etc/syslog.conf
$ sudo /etc/init.d/sysklogd restart
```



Now run bastille in an interactive mode



```
$ sudo bastille -c
```



The resultant dialog with bastille is too long and I am just showing the final configuration file which is produced as a result of bastille configuration. _I've used the port 22 as a proxy for the SSH port which I assume you've changed to some other value - use the other value instead of 22_



```
# Q:  Would you like to enforce password aging? [Y]
AccountSecurity.passwdage="Y"
# Q:  Should Bastille disable clear-text r-protocols that use IP-based authentication? [Y]
AccountSecurity.protectrhost="Y"
# Q:  Should we disallow root login on tty's 1-6? [N]
AccountSecurity.rootttylogins="N"
# Q:  Would you like to deactivate the Apache web server? [Y]
Apache.apacheoff="N"
# Q:  Would you like to disable CTRL-ALT-DELETE rebooting? [N]
BootSecurity.secureinittab="N"
# Q:  Should we restrict console access to a small group of user accounts? [N]
ConfigureMiscPAM.consolelogin="Y"
# Q:  Which accounts should be able to login at console? [root]
ConfigureMiscPAM.consolelogin_accounts="root"
# Q:  Would you like to put limits on system resource usage? [N]
ConfigureMiscPAM.limitsconf="N"
# Q:  Would you like to set more restrictive permissions on the administration utilities? [N]
FilePermissions.generalperms_1_1="Y"
# Q:  Would you like to disable SUID status for mount/umount?
FilePermissions.suidmount="Y"
# Q:  Would you like to disable SUID status for ping? [Y]
FilePermissions.suidping="Y"
# Q:  Do you need the advanced networking options?
Firewall.ip_advnetwork="N"
# Q:  Interfaces for DHCP queries: [ ]
Firewall.ip_b_dhcpiface=" "
# Q:  DNS Servers: [0.0.0.0/0]
Firewall.ip_b_dns="0.0.0.0/0"
# Q:  ICMP allowed types: [destination-unreachable echo-reply time-exceeded]
Firewall.ip_b_icmpallowed="destination-unreachable echo-reply time-exceeded"
# Q:  ICMP services to audit: [ ]
Firewall.ip_b_icmpaudit=" "
# Q:  ICMP types to disallow outbound: [destination-unreachable time-exceeded]
Firewall.ip_b_icmpout="destination-unreachable time-exceeded"
# Q:  NTP servers to query: [ ]
Firewall.ip_b_ntpsrv=" "
# Q:  Force passive mode? [N]
Firewall.ip_b_passiveftp="Y"
# Q:  Public interfaces: [eth+ ppp+ slip+]
Firewall.ip_b_publiciface="eth+"
# Q:  TCP service names or port numbers to allow on public interfaces: [ ]
Firewall.ip_b_publictcp="22 80 443"
# Q:  UDP service names or port numbers to allow on public interfaces: [ ]
Firewall.ip_b_publicudp=" "
# Q:  Reject method: [DENY]
Firewall.ip_b_rejectmethod="DENY"
# Q:  Enable source address verification? [Y]
Firewall.ip_b_srcaddr="Y"
# Q:  TCP services to audit: [telnet ftp imap pop3 finger sunrpc exec login linuxconf ssh]
Firewall.ip_b_tcpaudit="telnet ftp imap pop3 finger sunrpc exec login linuxconf ssh"
# Q:  TCP services to block: [2049 2065:2090 6000:6020 7100]
Firewall.ip_b_tcpblock="2049 2065:2090 6000:6020 7100"
# Q:  UDP services to audit: [31337]
Firewall.ip_b_udpaudit="31337"
# Q:  UDP services to block: [2049 6770]
Firewall.ip_b_udpblock="2049 6770"
# Q:  Should Bastille run the firewall and enable it at boot time? [N]
Firewall.ip_enable_firewall="N"
# Q:  Would you like to run the packet filtering script? [N]
Firewall.ip_intro="Y"
# Q:  Would you like to set up process accounting? [N]
Logging.pacct="N"
# Q:  Would you like to deactivate NFS and Samba? [Y]
MiscellaneousDaemons.remotefs="Y"
# Q:  Alert on all new packets?
PSAD.psad_alert_all="Y"
# Q:  psad check interval: [15]
PSAD.psad_check_interval="15"
# Q:  Would you like to setup psad?
PSAD.psad_config="Y"
# Q:  Danger Levels: [5 50 1000 5000 10000]
PSAD.psad_danger_levels="5 50 1000 5000 10000"
# Q:  Email addresses: [root@localhost]
PSAD.psad_email_alert_addresses="mailid@destination.com"
# Q:  Email alert danger level: [1]
PSAD.psad_email_alert_danger_level="1"
# Q:  Should Bastille enable psad at boot time? [N]
PSAD.psad_enable_at_boot="N"
# Q:  Enable automatic blocking of scanning IPs?
PSAD.psad_enable_auto_ids="N"
# Q:  Enable scan persistence?
PSAD.psad_enable_persistence="N"
# Q:  Port range scan threshold: [1]
PSAD.psad_port_range_scan_threshold="1"
# Q:  Scan timeout: [3600]
PSAD.psad_scan_timeout="3600"
# Q:  Show all scan signatures?
PSAD.psad_show_all_signatures="N"
# Q:  Would you like to disable printing? [N]
Printing.printing="Y"
# Q:  Would you like to disable printing? [N]
Printing.printing_cups="N"
# Q:  Would you like to display "Authorized Use" messages at log-in time? [Y]
SecureInetd.banners="Y"
# Q:  Should Bastille ensure inetd's FTP service does not run on this system? [y]
SecureInetd.deactivate_ftp="Y"
# Q:  Should Bastille ensure the telnet service does not run on this system? [y]
SecureInetd.deactivate_telnet="Y"
# Q:  Who is responsible for granting authorization to use this machine?
SecureInetd.owner="Company Name"
# Q:  Would you like to set a default-deny on TCP Wrappers and xinetd? [N]
SecureInetd.tcpd_default_deny="N"
# Q:  Do you want to stop sendmail from running in daemon mode? [Y]
Sendmail.sendmaildaemon="Y"
# Q:  Would you like to install TMPDIR/TMP scripts? [N]
TMPDIR.tmpdir="N"
```







##  Installing the Lamp Stack 



Let us use the not so well known _tasksel_ command to download all the packages necessary for a lamp stack. Note that _tasksel_ is simply a convenience tool around _apt-get_ and allows one to install a whole bunch of packages based on a class of necessity - in this case a _LAMP (Linux,Apache,MySQL,PHP)_ stack.



```
$ sudo tasksel install lamp-server
```



During the installation you will be required to create the password for the mysql _root_ user. In interest of tight security do make sure to create a really strong password.

This actually sets up the basic LAMP stack and that should get you operational.



###  Securing Apache 





####  Change the user and group apache runs as 



You may be aware that installation of apache resulted in a new userid and group being created (both called _www-data_). I just find it a little bit more comforting to change the well known usernames to something that are less predictable. In the steps below I change _www-data_ to _apache_ but you may want to use something a little less predictable than _apache_.



```
$ sudo usermod -l apache www-data
$ sudo groupmod -n apache www-data
```



We shall need to change the same in the apache configuration file. Edit the file _/etc/apache2/envvars_ and change the two lines as follows



```
export APACHE_RUN_USER=apache
export APACHE_RUN_GROUP=apache
```





###  Install and configure mod_security 



Earlier when we configured, bastille and through it the iptables firewall, we allowed incoming public traffic on the SSH, HTTP and HTTPS ports. We've already covered the tightening of SSH security. However we still do not have a good way to control HTTP and HTTPS traffic. The solution to that is installing the apache module mod_security. It allows us abilities to inspect and if necessary reject or redirect HTTP/S traffic. The full description of mod_security is beyond the scope of this document, but a good example of how it can be used further to implement application level security as well can be had from the document [Securing web services with mod_security](http://www.modsecurity.org/documentation/Securing_Web_Services_with_ModSecurity_2.0.pdf). Another useful overview page is [Secure your Apache2 with mod-security](http://www.debuntu.org/2006/08/13/86-secure-your-apache2-with-mod-security).

We shall start off with installing mod_security.



```
$ sudo apt-get install libapache-mod-security
```



You should notice a new file in _/etc/apache2/conf.d_ called _security_. These are the global mod_security settings.

Change the following variables in the file to make server identification difficult. Note that while these settings do not enhance the security directly, they do make it harder for an intruder to easily identify the specific server and configuration, thus making it harder for him to attempt configuration specific exploits.



```
ServerTokens Prod
ServerSignature Off
TraceEnable Off
```



Also uncomment the following four lines. Note that this may require you to configure other web applications which are not configured appropriately.




```
                AllowOverride None
                Order Deny,Allow
                Deny from all
```




While not related to mod_security, to minimise server information leakage, you may consider changing these two variables to the shown values in _/etc/php5/apache2/php.ini_



```
expose_php = Off
display_errors = Off
```



As mentioned above, there is a whole range of additional controls you can enforce using mod_security. Depending upon your specific requirements feel free to leverage mod_security much more.



###  Disable apache modules you do not need 



In general it is better to turn off unrequired modules unless you really need them. The default configuration installs a number of modules which may not be required. The modules that have been enabled are in the _/etc/apache2/mods-enabled_ directory which are all symbolic links to the modules that have been installed in the _/etc/apache2/mods-available_ directory. Note that turning off some modules may affect some web applications you install, in which case you may choose to subsequently turn them on only if necessary. New modules can be enabled with _sudo a2enmod module_name_ command and enabled modules can be disabled by _sudo a2dismod module_name command_.

Depending upon my requirements, I disabled the following modules



```
$ sudo a2dismod autoindex       # used to build directory indexes like the ls command
$ sudo a2dismod cgi             # used to run cgi scripts
$ sudo a2dismod env             # used to set environment variables for CGI & SSI scripts
```





####  Chrooting apache 



This is an advanced topic that I shall skip. Note that if you do setup chrooting, there's a lot of additional considerations which will need to be observed, considerations especially around shared libraries and configuration files which will make the overall process of configuration extremely difficult.




## Setting up snort and acidbase



_Author's Note: Careful - Very likely, there were a few things that I might have missed or incorrectly noted especially in the context of acidbase and following these instructions may not get acidbase running properly. You may have to do some steps outside these notes to get it all working properly. Unfortunately I am unable to rerun the process on a fresh server so am unable to immediately note the possible lacunae in the notes_

Now that you have a mysql server going, create a new database for snort. In the following section I use _snortdb_ as the database, _snort_ as the user and _snortpwd_ as the password for the database. However I do encourage you to replace the same with some non easily guessable names and passwords.

Here's the commands to create a new snort database and user. In the following example '_$_' is the unix prompt while '_>_' is the mysql prompt.


```
$ mysql -u root -p      # You will be prompted for the password for the mysql root account. Enter it
> create database snortdb;
> grant select, insert, update, delete, create, drop, index, alter, create temporary tables, lock tables on snortdb.* to 'snort'@'localhost' identified by 'snortpwd';
> flush privileges;
> quit;
```



Now we shall install snort.



```
$ sudo apt-get install snort-mysql
```



The installation process will require you to enter the network that you wish to protect. Since in this case we are only protecting a single machine, enter the IP_Address/32 eg. 12.34.56.78/32

The process will prompt you whether you wish to configure the database. Select "Yes" .. the script will proceed but terminate abnormally with the error "subprocess post-installation script returned error exit status 6"

Now execute the following commands to setup the tables in the database. At the end you shall open the snort configuration file to enter the database configuration parameters



```
$ cd /usr/share/doc/snort-mysql
# Enter the database password next when prompted for one
$ sudo zcat create_mysql.gz | mysql -u snort -p snortdb
$ sudo vi /etc/snort/snort.conf
```



Uncomment the line that starts with _output database: log, mysql_ and add the configuration information at the end as shown



```
output database: log, mysql, user=snort password=snortpwd dbname=snortdb host=localhost
```



Now remove the file as shown below to indicate that the database configuration has been done and start snort. The subsequent command confirms that snort daemon is indeed up and running



```
$ sudo rm -rf /etc/snort/db-pending-config
$ sudo /etc/init.d/snort start
$ sudo /etc/init.d/snort status
```



Now, lets install acid which is a web based application to be able to view snort alerts



```
$ sudo apt-get install acidbase
```



Select No when it prompts you for configuring the database. By default it uses the database and userid '_snort_' and I prefer to keep these different from a predictable default, in which case acidbase database initialisation will fail.

Edit the file _/etc/dbconfig-common/acidbase.conf_.

Set the following values



```
dbc_dbuser='snort'
dbc_dbpass='snortpwd'
dbc_dbname='snortdb'
```



The default configuration allows acid data to be accessed only from the machine where it is involved. Since this shall be a VPS which we shall be accessing remotely, it is time to relax the constraint. If you have a good idea of the network IPs that you will be accessing the acid web application from, update the same in the _allow from_ directive in _/etc/acidbase/apache.conf_, else relax it fully to _allow from all_.

Also protect the '_/acidbase_' url by http basic authentication. The resultant section in the file _/etc/acidbase/apache.conf_ looks like :



```
< DirectoryMatch /usr/share/acidbase/>
  Options +FollowSymLinks
  AuthType Basic
  AuthName "Go Away World!
  AuthUserFile /etc/acidbase/mypassword
  Require valid-user
  AllowOverride None
  order deny,allow
  deny from all
  allow from all
  
        php_flag magic_quotes_gpc Off
        php_flag track_vars On
        php_value include_path .:/usr/share/php
  
```




You will need to create a user id / password pair for the app or point AuthUserFile above to another file where you already have the ones set up for your server.

To do so you shall need to run the following command :



```
$ htpasswd -c /etc/acidbase/mypassword my_user_id
```



If you need additional user ids to be installed use the command as shown above again with different user ids - but make sure not to use the _-c_ switch since thats only used the first time to create the password file.

Now start the web application by restarting apache



```
$ sudo /etc/init.d/apache2 restart
```



Goto the web page _http:_vps_fully_qualified_domain_name/acidbase/base_db_setup.php_, click on '_Create Base AG//' and you are on your way.




## File integrity checking using AIDE



Install AIDE



```
$ sudo apt-get install aide
```



Update the aide configuration file _etc/default/aide_ to update your email id where you would prefer the integrity check reports sent (Look for the variable MAILTO)
Now initialise the aide configuration



```
$ sudo aideinit
$ cd /var/lib/aide
$ mv aide.db.new aide.db
```



Now run the process (which will be running once daily)



```
$ /etc/cron.daily/aide
```



This process takes a long time (about 5 mins for me) and will at the end email you a report if any files changed compared to the ones in the default database.


