**3389 port allows us for RDP conncetion.**

### Port 3306
- [ ] Check for Default passwords: root: blank password
- [ ] Bruteforce 
- [ ] Check if it is connceting to your machine and if yes then 
	- [ ] Start your server locally and then create a user and database [Admirer HTB]


### Port 21
- [ ] Check for anonymous login
	- [ ] Secrets in directory
	- [ ] Directory is accessible by web so you can put shell and get reverse connection.
- [ ] Search the version in **searchsploit**
- [ ] Try the credentials if you have them.
- [ ] Nmap scan
	```bash
    nmap –script ftp-anon,ftp-bounce,ftp-libopie,ftp-proftpd-backdoor,ftp-vsftpd-backdoor,ftp-vuln-cve2010-4221,tftp-enum -p 21 10.0.0.1
	```



### Port 22
- [ ] Run nmap ssh NSE scripts.
- [ ] Check for default passwords
- [ ] Try credentials if you have any.
- [ ] [Predictable Private key](https://github.com/g0tmi1k/debian-ssh).
- [ ] [Public key with weak crypto](https://github.com/Ganapati/RsaCtfTool).
- [ ] **(Last option)**Check if they have weak credential try bruteforce iva Hydra `hydra -L users.txt -P pass.txt -t 10 <IP address> ssh -s 22`

### Port 25
- [ ] User enumeration.
```bash
nc 10.11.1.217 25
VRFY root
252 2.0.0 root
VRFY idontexist
550 5.1.1 <idontexist>: Recipient address rejected: User unknown in local recipient table
```
- [ ] User enum with nmap
	```bash
	sudo nmap --script smtp-enum-users 10.10.10.51 -p25
	```
- [ ] [[smpt-vrfy-user.py]]
- [ ] Nmap scan
	```bash
    sudo nmap -–script smtp-commands,smtp-enum-users,smtp-vuln-cve2010-4344,smtp-vuln-cve2011-1720,smtp-vuln-cve2011-1764 -p 25 10.0.0.1
	```
- [ ] If you have =emails then try phishing them=
	```bash
	# Multiple
	swaks --to $(cat emails | tr '\n' ',' | less) --from test@sneakymailer.htb --header "Subject: test" --body "please click here http://10.10.14.42/" --server 10.10.10.197
	# Single
	swaks --to sales@humongousretail.com --from it@humongousretail.com --header "Subject: test" --body "please click here http://10.10.14.14/" --server 10.10.10.197
	```
	- Start the netcat -nvlp at port 80.
- [ ] Use inbuild Mailer if you have credentials `Evolution`.


### Port 110 (POP3)
- [ ] Telnet the service
	```bash
	telnet [IP] 110
	USER [username]
	PASS [password]
	# To list messages
	list
	# Tp retrive messages
	RETR [message number]
	# Quit
	quit
	```


### Port 143 (IMAP)

Login:
```bash
nc 10.10.10.143 143
login paul password
LIST "" "*"
SELECT inbox
SELECT "INBOX.Sent Items"

# Fetch an email

FETCH 1 BODY.PEEK[]
# similaraly fetch 2nd email
FETCH 2 BODY.PEEK[]

```
### Port 53 (DNS)
- [ ] Get the information from zonetransfer
```bash
# Guessing Domain.com ex: firendzone.red
dig axfr @[IP] DOMAIN.COM
	
	# or we can use this also
	
dnsrecon -d TARGET -D /usr/share/wordlists/dnsmap.txt -t std --xml output.xml
```


### Port 88 (Kerberos)
- [ ] Enumerate kerberos Users
	```bash
	nmap -sV --script krb5-enum-users --script-args krb5-enum-users.realm='domain.local',userdb='/usr/share/wordlist/userlist.txt' <target ip> -p88
	```






### Port 111 (NFS-2049/RPC)
- [ ] [[NFS]]
- [ ] Enumerate RPC first.
	`nmap -sV -p 111 --script=rpcinfo $RHOST`
- [ ] If nfs related services
	`nmap -p 111 --script nfs* $RHOST`
- [ ] Check if you can add users else remember this is for soemthing else [NFS](https://casvancooten.com/posts/2020/05/oscp-cheat-sheet-and-command-reference/#rpc--nfs-111tcp)
- [ ] Command line check
	```bash
	rpcinfo -p x.x.x.x
	```


### Port 80, 443, 8000, 8080, 8443 .. (HTTPs)

- [ ] Check the web interface
- [ ] Run Dirb/DirSearch/Gobuster if machine taking load then use **wfuzz**
- [ ] Run nikto `nikto -h $RHOST -o nikto-oit.txt`
- [ ] Run SSLScan for vulns in SSL: `sslscan $RHOST`
- [ ] Search for every service or verion you identify on **searchsploit**
- [ ] Look for source code, backup files.
- [ ] **(Last Option)** All-in-one run [AutoRecon](https://github.com/Tib3rius/AutoRecon) which enumerates everything almost.
- [ ] Check for shellshock if you see linux bash script execution: For capture the request see [[shellshock]]
	```bash
	sudo nmap --script http-shellshock --script-args uri=/cgi-bin/user.sh,cmd=ls $IP
	```

### Port 137/TCP , 137/UDP
-   NetBIOS Names : Port 137/UDP or 137/TCP
    
-   Session Service : Port 139/TCP
    
-   Connectionless Communication : Port 138/UDP

```bash
nmblookup -A <IP>
nbtscan <IP>/30
sudo nmap -sU -sV -T4 --script nbstat.nse -p137 -Pn -n <IP>
```

Scan for netbios nameservers:
`nbstat -r 192.168.43.1/24`

### Port 135 (MSRPC), 139 (NetBios-SSN) & 445 (Microsoft-DS) (SMB)


- [ ] Port 445 
	```bash
	rpcclient -U "" -N 10.10.10.161
	> enumdomusers
	> enumdomgroups
	> # Get User info
	> qeryuser 0x1f4
	```
- [ ] Check for eternal blue
- [ ] Check the OS version
	```bash
	nmap -v -p 139, 445 --script=smb-os-discovery 10.11.1.227
	```
- [ ] Obtain information
	```bash
	nmap --script "safe or smb-enum-*" -p 445 <IP>
	```
- [ ] GUI connection from Linux
	```bash
	xdg-open sbm://IP/
	```
- [ ] Open in Browser the URL.
	```bash
	smb://IP/general
	```
- [ ] Check if **SMB** is accessible from web and you have permission to write
- [ ] Check the SMB shares [[SMB]]
- [ ] Null Sessions
`smbclient -H hostname` it will auth without entring anything

### Port 143, 993 (IMAP) 
*sneakymailer*
- [ ] 

### Port 161/UDP (SNMP)
- [ ] Verfiy if port is open or not
	```bash
	sudo nmap -sU -sV -sC --open -p 161 $RHOST
	```
	
- [ ] Extrtact information using community string
	```bash
	# Pre-requisits
	sudo apt-get install snmp-mibs-downloader
	download-mibs
	
	# Find community string
	onesixtyone -c /usr/share/seclists/Discovery/SNMP/common-snmp-community-strings.txt $RHOST
	
	# SNMPCHECK
	snmp-check $RHOST 
	
	
	
	# SNMPWALK
	snmpwalk -v1 -c public 10.10.10.241 . > scans/snmpwalk-full
	snmpwalk -v1 -c public $RHOST
	
	# Nmap process finder
	nmap -sU -p 161 --script snmp-process 192.10.232.3
	```

- [ ] [Resource]( https://book.hacktricks.xyz/pentesting/pentesting-snmp#mib)

### Port 500/UDP (IKE)
UDP 500 is used for Internet Key Exchange which is used to establish IPSEC VPN. 

```bash
ike-scan -M 10.10.10.116

ike-scan -M --ikev2 10.10.10.116

```



### Port 389 & 636 (LDAPS)
- [ ] [Link to pentest LDAP](https://www.tarlogic.com/blog/how-to-attack-kerberos/)

```sh
ldapsearch -H ldap://10.10.10.161:389 -X -b DC=htb,DC=local |wc -l
```

### Port 4555 (RSIP)

Seen this port being used by Apache James Remote Configuration.

There is an [exploit for version 2.3.2](https://www.exploit-db.com/exploits/35513)


### Port 6379 (Redis)
*Default Username: Redis*
First thing to try to drop **webshell,ssh keys, crontab, check hacktricks redis rce**
- SSH


- Connect with password leaked
	`rdcli -h 'machine-ip' -a [...]`


Redis is a nosql database that stores everything in RAM as key value pairs
By default, A text orientaition interface is reachable on port 6379 without any authentication.
 All you need to know right now is that the interface is very forgiving and will try to parse every provided input (until a timeout or the 'QUIT' command). It may only quietly complain via messages like "-ERR unknown command".
 
 Get the version number:
 ```bash
 info
 ```
 
 
- [Redis Master Slave Replication](https://medium.com/@knownsec404team/rce-exploits-of-redis-based-on-master-slave-replication-ef7a664ce1d0)
 
 
 
 https://medium.com/@knownsec404team/rce-exploits-of-redis-based-on-master-slave-replication-ef7a664ce1d0
 https://book.hacktricks.xyz/pentesting/6379-pentesting-redis
 https://github.com/iw00tr00t/Redis-Server-Exploit
 https://medium.com/@Victor.Z.Zhu/redis-unauthorized-access-vulnerability-simulation-victor-zhu-ac7a71b2e419
 

----

<center><u><h2>Final Notes and Links</h2></u></center>

[More service checklist](https://hausec.com/pentesting-cheatsheet/#_Toc475368978)
[More service checklist](https://mrw0r57.github.io/2020-05-27-enumeration-is-the-key-to-success-6-1/)