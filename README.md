# linux_security

## NOTHING IS 100% SECURE

![image](https://user-images.githubusercontent.com/107435692/234869988-b1bde71d-70ca-466e-93dd-8e1ba7dc7f7d.png)

## DON'T TRUST ANY IN YOUR ENVIRONMENT ALWAYS USE ZERO TRUST POLICY

![image](https://user-images.githubusercontent.com/107435692/234870116-10e96643-21a5-4f5b-aee8-b3c708d37567.png)

## DEFENCE IN DEPTH

![image](https://user-images.githubusercontent.com/107435692/234870179-2bf42db4-44fe-42ab-97b2-92144786cd1b.png)

## NEVER GIVE ROOT ACCESS TO SSH THIS IS A PRINCIPLE OF LEAST PREVILAGE 

![image](https://user-images.githubusercontent.com/107435692/234871454-617ae941-a7c1-4d56-b3b3-bcf73b7d0e3e.png)

## ALWAYS MONITOR USING "IPS & IDS" MONITORING LOGGING USE "WIRESHARK"

![image](https://user-images.githubusercontent.com/107435692/234872164-eb81ba90-e98c-4c4d-b0d9-b9de7a823c9a.png)

## LEARNING IS CONTINOUS PROCESS 

![image](https://user-images.githubusercontent.com/107435692/234872675-d3590d83-066c-4bdb-8ae3-53a19432b9cf.png)

## IMPORTANT POINT IN SECURITY

![image](https://user-images.githubusercontent.com/107435692/234873019-7e1a3684-17be-4d2f-86ae-519adad92409.png)

## Logging
```
journal -u NetworkManager | tail -10 | less
nmcli general logging
nmcli general logging level DEBUG domains ALL
```
## REDUCE THE ATTACK SURFACE
```
systemctl list-unit-files
systemctl list-unit-files --type service | grep enabled | awk '{print $1 $2}'
systemctl --type service
Delete the unnecessary services
Ex: There are 2 two services running for Network 1. NetworkManager 2. Networking
systemctl stop networking                              > It is start after service This change is temporary
systemctl --now disable networking                     > Permanent Even after reboot
systemctl --now disable cockpit.socker                 > Disable permanent Even after reboot 
```

## CHECK OPEN PORTS ON THE SPECIFIC IP ADDRESS 
```
nmap 10.0.0.45 -P
```

## CHECK THE PORTS WHICH ARE LISTINING ON
```
ss -tulnw
```

## CHECK THE PORTS WHICH ARE LISTINING ON & PROCESS ID AND USER WHO IS WORKING ON THAT
```
ss -plunt
```

## DISABLE THE SERVICE AND ALL CONFIGURATION
```
apt remove openssh-server -y
apt pruge openssh-server    > Remove all the configuration files
dnf remove cockpit -y
```

## CHECK THE SYSTEM DEGRADED OR NOT
```
systemctl status                  > **running is show everything is fine it is not degraded if it is red it is degraded**
systemctl --failed                > **TO KNOW WHAT SERVICE IS FAILED**
```
## LETS DEGRADED THE SYSTEM
```
sudo echo "Port 22@AFHAKHA" >> /etc/ssh/sshd_config
sudo systemctl restart ssh
**THIS WILL DEGRADE THE SYSTEM IN THE NEXT STEP WE WILL TROUBLESHOOT THE ISSUE**
```
## DEBUGGING THE FAILED SERVICE WHICH DEGRADED THE LINUX SYSTEM
```
systemctl status
systemctl --failed
systemctl status <failed-service>                  > This Only gives last 5 logs 
journalctl -u <failed-service> | tail -10 | less   > This gives last 10 logs
jourcalctl -u <failed-service>  

**CHECK THE LOGS WHICH IT POINTING OUT FILE PATH AND ERROR BASED ON THAT GO AND DEBUG THE ISSUE GOING TO THAT FILE**

systemctl restart <failed-service>
systemctl status                                   > **CHECK THE STATUS IS GREEN AND RUNNING**
```
