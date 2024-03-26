- [ ] Users with Weak Passwords
- [x] Kerberoasting
- [ ] WinRM misconfigurations
- [ ] Abusing ACE/ACL
- [ ] Passwords in SYSVOL
- [ ] Passwords in Registry
- [ ] AlwaysInstallElevated Misconfiguration
- [x] AS-REP Roasting
- [x] [Abuse DnsAdmins](https://www.hackingarticles.in/windows-privilege-escalation-dnsadmins-to-domainadmin/)
- [x] Password in Object Description
- [x] SMB Shares
- [ ] User Objects With Default password (Changeme123!)
- [ ] Password Spraying
- [ ] DCSync
- [ ] Silver Ticket
- [ ] Golden Ticket
- [ ] Pass-the-Hash
- [ ] Pass-the-Ticket
- [ ] SMB Signing Disabled
- [ ] DACL and SACL based attacks
- [ ] RBCD Resource Based Contraint Delegation



#todo 
Enable PSremoting so that you can PS remote to any account from Administrator

[AD Attacks](https://github.com/WazeHell/vulnerable-AD)

Server Name: RT-DC01
Domain Controller: backhole.local
Administrator Password: superStr0ngPassw0rd
DSRM Password : superStr0ngPassw0rd@1337
IP : 10.78.81.114
Host : 10.78.81.115

DC2: neutron : superStr0ngPassw0rd3
DC3: deathstar : superStr0ngPassw0rd2

Adam :: User1 :: Str0ngPass :: adam@backhole.local :: Password Never Expires
Trump :: iamelite@1337 :: trump@backhole.local :: same  

Domain Admin
Eve :: MyPassStr0ng :: eve@backhole.local :: 

One more but service account for the "Descriptors passwords thing. and service accounts should not be in domain admins"

MySQLSvc :: mysql@1337 :: Domain Admin :: mysqlsvc@backhole.local :: Description

Shares :: Done 
Name :: backup

Kerberosting ::  Done 
For this we need to setup the SPN for the sql service.
`setspn -a HYDRA-DC/MySQLSvc.backhole.local:60011 Backhole\SQLService`
List all the SPNs:
`setspn -T Backhole.local -Q */*`

AS-REP Roasting
Sam :: sam@backhole.local :: Password@1234

DNSadmin
Toy :: toy@backhole.local :: T0y@1337

PS session ke liye `net localgroup` krke 

10.78.81.115
