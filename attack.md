Welcome to Ubuntu 20.04.5 LTS (GNU/Linux 5.15.0-1032-gcp x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

 System information disabled due to load higher than 2.0

 * Strictly confined Kubernetes makes edge and IoT secure. Learn how MicroK8s
   just raised the bar for easy, resilient and secure K8s cluster deployment.

   https://ubuntu.com/engage/secure-kubernetes-at-the-edge

41 updates can be applied immediately.
7 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

New release '22.04.2 LTS' available.
Run 'do-release-upgrade' to upgrade to it.


*** System restart required ***
Last login: Tue May 23 22:13:46 2023 from 35.235.241.18
```
pruthvis2340@backend-api:~$ systemctl --failed
  UNIT                          LOAD   ACTIVE SUB    DESCRIPTION                           
● google-osconfig-agent.service loaded failed failed Google OSConfig Agent                 
● networkd-dispatcher.service   loaded failed failed Dispatcher daemon for systemd-networkd

LOAD   = Reflects whether the unit definition was properly loaded.
ACTIVE = The high-level unit activation state, i.e. generalization of SUB.
SUB    = The low-level unit activation state, values depend on unit type.

2 loaded units listed.
```
```
pruthvis2340@backend-api:~$ systemctl status networkd
Unit networkd.service could not be found.
pruthvis2340@backend-api:~$ systemctl status networkd-dispatcher.service 

pruthvis2340@backend-api:~$ ^C
pruthvis2340@backend-api:~$ systemctl status google-os
google-osconfig-agent.service  google-oslogin-cache.service   google-oslogin-cache.timer 
```
```
pruthvis2340@backend-api:~$ systemctl status google-osconfig-agent.service 
● google-osconfig-agent.service - Google OSConfig Agent
     Loaded: loaded (/lib/systemd/system/google-osconfig-agent.service; enabled; vendor preset: enabled)
     Active: failed (Result: signal) since Mon 2023-05-08 08:04:24 UTC; 2 weeks 1 days ago
   Main PID: 212998 (code=killed, signal=KILL)

May 08 08:04:23 backend-api systemd[1]: google-osconfig-agent.service: Main process exited, code=killed, status=9/KILL
May 08 08:04:23 backend-api systemd[1]: google-osconfig-agent.service: Failed with result 'signal'.
May 08 08:04:24 backend-api systemd[1]: google-osconfig-agent.service: Scheduled restart job, restart counter is at 4.
May 08 08:04:24 backend-api systemd[1]: Stopped Google OSConfig Agent.
May 08 08:04:24 backend-api systemd[1]: google-osconfig-agent.service: Start request repeated too quickly.
May 08 08:04:24 backend-api systemd[1]: google-osconfig-agent.service: Failed with result 'signal'.
May 08 08:04:24 backend-api systemd[1]: Failed to start Google OSConfig Agent.
```
```
pruthvis2340@backend-api:~$ systemctl restart google-osconfig-agent.service 
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to restart 'google-osconfig-agent.service'.
Authenticating as: Ubuntu (ubuntu)
Password: 
polkit-agent-helper-1: pam_authenticate failed: Authentication failure
==== AUTHENTICATION FAILED ===
Failed to restart google-osconfig-agent.service: Access denied
See system logs and 'systemctl status google-osconfig-agent.service' for details.
```
```
pruthvis2340@backend-api:~$ systemctl restart google-osconfig-agent.service 
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to restart 'google-osconfig-agent.service'.
Authenticating as: Ubuntu (ubuntu)
Password: 
polkit-agent-helper-1: pam_authenticate failed: Authentication failure
==== AUTHENTICATION FAILED ===
Failed to restart google-osconfig-agent.service: Access denied
See system logs and 'systemctl status google-osconfig-agent.service' for details.
pruthvis2340@backend-api:~$ sudo su
```
```
root@backend-api:/home/pruthvis2340# systemctl restart google-osconfig-agent.service
root@backend-api:/home/pruthvis2340# passwd
New password: 
Retype new password: 
passwd: password updated successfully
root@backend-api:/home/pruthvis2340# systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2023-05-21 08:05:06 UTC; 2 days ago
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 635099 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
    Process: 651367 ExecReload=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
    Process: 651369 ExecReload=/bin/kill -HUP $MAINPID (code=exited, status=0/SUCCESS)
   Main PID: 635100 (sshd)
      Tasks: 1 (limit: 1148)
     Memory: 10.9M
     CGroup: /system.slice/ssh.service
             └─635100 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups

May 23 22:34:39 backend-api sshd[703222]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=23.97.195.150  user=root
May 23 22:34:41 backend-api sshd[703220]: Received disconnect from 85.234.137.154 port 43394:11: Bye Bye [preauth]
May 23 22:34:41 backend-api sshd[703220]: Disconnected from authenticating user root 85.234.137.154 port 43394 [preauth]
May 23 22:34:41 backend-api sshd[703222]: Failed password for root from 23.97.195.150 port 38444 ssh2
May 23 22:34:43 backend-api sshd[703222]: Received disconnect from 23.97.195.150 port 38444:11: Bye Bye [preauth]
May 23 22:34:43 backend-api sshd[703222]: Disconnected from authenticating user root 23.97.195.150 port 38444 [preauth]
May 23 22:34:46 backend-api sshd[703224]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=20.193.246.165  user=root
May 23 22:34:48 backend-api sshd[703224]: Failed password for root from 20.193.246.165 port 39782 ssh2
May 23 22:34:48 backend-api sshd[703224]: Received disconnect from 20.193.246.165 port 39782:11: Bye Bye [preauth]
May 23 22:34:48 backend-api sshd[703224]: Disconnected from authenticating user root 20.193.246.165 port 39782 [preauth]
May 23 22:35:30 backend-api sshd[703232]: Invalid user oratest from 185.239.71.86 port 47312
May 23 22:35:30 backend-api sshd[703232]: pam_unix(sshd:auth): check pass; user unknown
May 23 22:35:30 backend-api sshd[703232]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=185.239.71.86
May 23 22:35:33 backend-api sshd[703232]: Failed password for invalid user oratest from 185.239.71.86 port 47312 ssh2
May 23 22:35:33 backend-api sshd[703234]: Invalid user guillermo from 159.192.143.249 port 37546
May 23 22:35:33 backend-api sshd[703234]: pam_unix(sshd:auth): check pass; user unknown
May 23 22:35:33 backend-api sshd[703234]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=159.192.143.249
May 23 22:35:34 backend-api sshd[703232]: Received disconnect from 185.239.71.86 port 47312:11: Bye Bye [preauth]
May 23 22:35:34 backend-api sshd[703232]: Disconnected from invalid user oratest 185.239.71.86 port 47312 [preauth]
May 23 22:35:36 backend-api sshd[703234]: Failed password for invalid user guillermo from 159.192.143.249 port 37546 ssh2
```
```
root@backend-api:/home/pruthvis2340# nano /etc/ssh/sshd_config
root@backend-api:/home/pruthvis2340# systemctl restart ssh
root@backend-api:/home/pruthvis2340# systemctl statis google-osconfig-agent.service
Unknown operation statis.
root@backend-api:/home/pruthvis2340# systemctl status google-osconfig-agent.service
● google-osconfig-agent.service - Google OSConfig Agent
     Loaded: loaded (/lib/systemd/system/google-osconfig-agent.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2023-05-23 22:34:10 UTC; 5min ago
   Main PID: 703203 (google_osconfig)
      Tasks: 10 (limit: 1148)
     Memory: 23.3M
     CGroup: /system.slice/google-osconfig-agent.service
             └─703203 /usr/bin/google_osconfig_agent

May 23 22:34:10 backend-api systemd[1]: Started Google OSConfig Agent.
May 23 22:34:13 backend-api OSConfigAgent[703203]: 2023-05-23T22:34:13.2217Z OSConfigAgent Info: OSConfig Agent (version 20210608.1-0ubuntu1~20.04.1) started.
root@backend-api:/home/pruthvis2340# systemctl --failed
  UNIT                        LOAD   ACTIVE SUB    DESCRIPTION                           
● networkd-dispatcher.service loaded failed failed Dispatcher daemon for systemd-networkd

LOAD   = Reflects whether the unit definition was properly loaded.
ACTIVE = The high-level unit activation state, i.e. generalization of SUB.
SUB    = The low-level unit activation state, values depend on unit type.

1 loaded units listed.
```
```
root@backend-api:/home/pruthvis2340# systemctl restart networkd-dispathcher
Failed to restart networkd-dispathcher.service: Unit networkd-dispathcher.service not found.
root@backend-api:/home/pruthvis2340# systemctl restart networkd-dispatcher
root@backend-api:/home/pruthvis2340# systemctl status networkd-dispatcher
● networkd-dispatcher.service - Dispatcher daemon for systemd-networkd
     Loaded: loaded (/lib/systemd/system/networkd-dispatcher.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2023-05-23 22:46:11 UTC; 44s ago
   Main PID: 703799 (networkd-dispat)
      Tasks: 1 (limit: 1148)
     Memory: 25.2M
     CGroup: /system.slice/networkd-dispatcher.service
             └─703799 /usr/bin/python3 /usr/bin/networkd-dispatcher --run-startup-triggers

May 23 22:45:11 backend-api systemd[1]: Starting Dispatcher daemon for systemd-networkd...
May 23 22:45:16 backend-api networkd-dispatcher[703799]: No valid path found for iwconfig
May 23 22:45:17 backend-api networkd-dispatcher[703799]: No valid path found for iw
May 23 22:46:11 backend-api systemd[1]: Started Dispatcher daemon for systemd-networkd.
root@backend-api:/home/pruthvis2340# nano /etc/ssh/sshd_config
root@backend-api:/home/pruthvis2340# systemctl restart sshd
root@backend-api:/home/pruthvis2340# 
```
