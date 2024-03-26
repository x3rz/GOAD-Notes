Machine: blackhole
Password: superStr0ngPassw0rd
IP : 192.168.43.155

#### Domain Admin 
Earth : earth@blackhole.local : SuperStr0ng

#### AS-Rep Roasting + Remote login
Venus : venus@blackhole.local : SuperV3nus

#### DNSadmin
Mars : mars@blackhole.local : SuperM4r5

#### Domain Admin
moon : moon@blackhole.local : SuperM00nJi

#### Kerberoasting + Domain Admin + Password in Description [Link](https://systemweakness.com/kerberoasting-part-1-lab-setup-6e2a6fa15b93) + Remote Login
MySQLsvc : mysqlsvc@blackhole.local : P@\$\$w0rd@123
SPN: `setspn -a blackhole/SQLservice.blackhole.local:6001 blackhole-DC\SQLservice`

#### Password Spraying
```cmd
net user bob.smit Password123 /dom /add
```

#### RBCD Attack + Kerberosting
Need a Workstation in the DC network of hacing `GenericWrite` permission
Jupiter : jupiter@blackhole.local :  Welcome123

#### DACL Exploitation + Neptune can change the password for Mars and also has remote login
neptune@blackhole.local : Welcome!
Mars ke andr Neptune ki entry daalni

#### Windows Privilege Escalation : Server Operator Group
The Server Operator group is a special user group that often has access to powerful commands and settings on a computer system. This group is typically used for managing a server or for troubleshooting system problems. Server Operators are usually responsible for monitoring the server’s performance, managing system security, and providing technical support to users. They may also oversee installing software updates, creating and maintaining user accounts, and performing routine maintenance tasks.

Mars : mars@blackhole.local 

Being a member of server operator group is not a vulnerability, but the member of this group has special privileges to make changes in the domain which could lead an attacker to escalate to system privilege
From command we can `service` to list all the controlable services.
[[Server Operator Group Exploitation]]

