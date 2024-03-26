## What is SMB?
SMB - Server Message Block Protocol - is a client-server communication protocol used for sharing access to files, printers, serial ports and other resources on a network.

![](https://i.imgur.com/XMnru12.png)


**EternalBlue manual exploit:** [link](https://root4loot.com/post/eternalblue_manual_exploit/)

## Enumeration
```bash
# enumerate all with no pass and username
enum4linux -A [IP] 
# enumerate with specific password and username
enum4linux -u administrator  -p password -U target-ip

crackmapexec smb 10.10.10.175
```

## Show Shares
```bash
# Shows share
smbmap -H  [IP] -u anonymous

nmblookup -A [IP]

smbclient --no-pass -L //sauna.egotistical-bank.local

# If  NT_STATUS_CONNECTION_DISCONNECTED error
smbclient -L //[IP] --option='cloent min protocol=NT1' -U="administrator"
# Username and password
smbclient //10.10.10.100/Users -U active.htb\\SVC_TGS%GPPstillStandingStrong2k18

# Username and password
smbmap -H [IP] -d domain -u [username] -p [password]
```

SMB no pass
`smbclient -L //[IP] -N`

**Check For Null sessions**

In windows NT2000/XP default config for SMB allows for nullsessions to be created. In windows 2003/XP SP2 onwards it is disabled.

***We could also change the password of others if user has the right permissions [article](https://malicious.link/post/2017/reset-ad-user-password-with-linux/)***

```bash
rpcclient -U "" [IP]
# sevrer info such as OS version, service type
srvinfo
# Users infomations
enumdomusers
# Show passwordpolicy
getdompwinfo
# Domain info
querydomaininfo
# Find net share
netshareenum

# Change password of other user

rpcclient -U "blackfield.local\support" 10.10.10.192

> setuserinfo2 audit2020 23 'password1'

```

```bash
nbtscan x.x.x.x
```

Discover Windows/Samba servers on subnet, find Windows MAC addresses, netbios name and discover client workgroup/ domain
```bash
# Discover subnet,MAC,netbios name,etc
python /usr/share/doc/python-impacket-doc/examples/samrdump.py x.x.x.x
```

## Change Password of SMB
- [ ] Have username and previous password?

```bash
smbpasswd -r 10.10.10.193 -U <user_name>
```

## Eploitation
```bash
# access share with login
smbclient //[IP]/[Dir] -U [User] -p [Port]

# access share 
smbclient //[IP]/share 
```

`smbmap -H  [IP] -R` 

----
## Download files from SMB
```bash
# download all files recusively
smbget -R smb://<ip>/share

# Download specified file
smbmap -R share -H [IP] -A [filename] -q

smbclient \\\\10.10.10.100\\users
> recurse on
> prompt off
> mget **

```
---



**Connect GUI connection**
```bash
xdg-open smb://cascade.htb/
```
**Open in broswer:**
```bash
URL - smb://friendzone.htb/general/
```

## Mount shares
```bash
mount -t cifs //x.x.x.x/share /mnt/share
mount -t cifs -o "username=user,password=password" //x.x.x.x/share /mnt/share
```

## Error in Connection
```bash
# smbclient -L 10.10.10.3
protocol negotiation failed: NT_STATUS_CONNECTION_DISCONNECTED
```
So in this one we need to enable NT1 because it is blocked by default in SMB client config file `/etc/samba/smb.conf`.

```cmdlet
[global] 
client min protocol=NT1
```


## Commands useful
**Logon** Uses to login the user into the smb shares
```bash
logon "user" "password"
```