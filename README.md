<h1>SMB Samba Enumeration Part 2</h1>


<h2>Description</h2>
This project consists of using the Nmap, rpcclient, enum4linux, smbclient, & msfconsole Linux tools to enumerate SMB Samba information. 
<br />
<br />
- Samba is a suite of applications that implements the Server Message Block (SMB) protocol. Many operating systems, including Microsoft Windows, use the SMB protocol for client-server networking. Samba enables Linux / Unix machines to communicate with Windows machines in a network.
<br />
<br />

Disclaimer: The Kali Linux user machine and target machine used in this project was provided to me by INE for educational purposes. The misuse of the information in this project lab can result in criminal charges brought against the individual/individuals in question.
<br />


<h2>Languages and Utilities Used</h2>

- <b>Nmap</b>
- <b>MSFconsole</b>
- <b>enum4linux</b>
- <b>smbclient</b>
- <b>rpcclient</b>


<h2>Environments Used </h2>

- <b>Kali Linux</b>

<h2>Project walk-through:</h2>

<p align="left">
Ping target machine to verify that it is active: <br/>
<br/>
- INE has provided me with Kali Linux GUI & target machine (target machine is one ip address above from mine).  I will run and ip a command to verify my own ip address. 
<br/>
<br/>
Commands: ip a
<br/>
ping -c 5 192.49.250.3
<br/>
<br/>
<img src="https://i.imgur.com/BsNS32w.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<br />
<br />
<br />
<br />
<br />
<br />
Run an Nmap scan to check for open ports: <br/>
<br/>
- It looks like port 445 (SMB) is open. 
<br/>
<br/>
Command: nmap 192.49.250.3
<br/>
<br/>
<img src="https://i.imgur.com/8daTRtN.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<br />
<br />
<br />
<br />
<br />
<br />
Run Nmap scan which will enumerate version information: <br/>
<br/>
- We can see that the nmap command below will check the version for port 445. It looks like port 445 is running Samba. 
<br/>
<br/>
Command: nmap 192.49.250.3 -p 445 -sV
<br/>
<br/>
<img src="https://i.imgur.com/ZkSOk7w.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<br />
<br />
<br />
<br />
<br />
<br />
Run rpcclient command to check if a guest connection is allowed: <br/>
<br/>
- It looks like the connection successfully connected and it even shows us the OS version which is 6.1. 
<br/>
- In the command down below, -U "" means that the user does not have a username (guest/null session) and -N means that we will try to connect without a password. 
<br/>
<br/>
Command: rpcclient -U "" -N 192.49.250.3
<br/>
<br/>
<img src="https://i.imgur.com/Whh2sDy.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<br />
<br />
<br />
<br />
<br />
<br />
Use the enum4linux tool to enumerate information about the SMB server: <br/>
<br/>
- We can use -h with any tool to get "help" and know how a tool can be used (example below). 
<br/>
- We can see Target info, Workgroup/Domain name, Null session check, & OS info. 
<br/>
<br/>
Commands: enum4linux -h
<br/>
enum4linux -o 192.49.250.3
<br/>
<br/>
<img src="https://i.imgur.com/SWD3Ez2.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<img src="https://i.imgur.com/lYw4SnP.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<br />
<br />
<br />
<br />
<br />
<br />
Use the smbclient tool to list shares: <br/>
<br/>
- We can see a list of shares for public, john, aisha, emma, everyone, and IPC$ (null session).
<br/>
<br/>
Command: smbclient -L 192.49.250.3
<br/>
<br/>
<img src="https://i.imgur.com/jSOZk0V.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<br />
<br />
<br />
<br />
<br />
<br />
Use the nmap tool to enumerate what smb protocols & dialects the target machine utilizes: <br/>
<br/>
- We can see the dialects that the smb server is running (SMBv1 which is dangerous, but default).
<br/>
<br/>
Command: nmap 192.49.250.3 -p 445 --script smb-protocols
<br/>
<br/>
<img src="https://i.imgur.com/Qg39KlV.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<br />
<br />
<br />
<br />
<br />
<br />
Use the msfconsole tool to check if the SMB2 protocol is supported by the Samba server: <br/>
<br/>
- We can see that the Samba server does support SMB2.
<br/>
<br/>
Commands: msfconsole
<br/>
use auxiliary/scanner/smb/smb2
<br/>
set rhosts 192.49.250.3
<br/>
exploit
<br/>
<br/>
<img src="https://i.imgur.com/eiBVT1L.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<img src="https://i.imgur.com/99OLlCo.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<br />
<br />
<br />
<br />
<br />
<br />
Use the nmap tool to list all of the users that exists on the Samba server: <br/>
<br/>
- We can see that admin, aisha, elie, emma, john, & shawn exists as users on the Samba server.
<br/>
<br/>
Command: nmap 192.49.250.3 -p 445 --script smb-enum-users
<br/>
<br/>
<img src="https://i.imgur.com/T2wfghz.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<br />
<br />
<br />
<br />
<br />
<br />
Use the enum4linux tool to list all of the users that exists on the Samba server: <br/>
<br/>
- We can see that admin, aisha, elie, emma, john, & shawn exists as users on the Samba server.
<br/>
<br/>
Command: enum4linux -U 192.49.250.3 
<br/>
<br/>
<img src="https://i.imgur.com/6QVHvJJ.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<br />
<br />
<br />
<br />
<br />
<br />
Use the rpcclient tool find the SID of the user admin: <br/>
<br/>
- SIDs are a component of a security database that security authorities can use to identify the user and the permissions that user is entitled to. When users log on to a Windows system, the system generates an access token that includes the user SID, the SID of any groups the user belongs to and the user privilege level.
<br/>
<br/>
Commands: rpcclient -U "" -N 192.49.250.3 
<br/>
lookupnames admin
<br/>
<br/>
<img src="https://i.imgur.com/H5gC07p.png" height="80%" width="80%" alt="SMB Nmap Scripting" class="center"/>
<br />
<br />
<br />
<br />
<br />
<br />
<br />



</p>
