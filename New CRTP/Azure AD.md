- Managament  Groups : a way to manage your 
- subscription
- resources

Azure + AD (Phonebook)

Azure supports SSO 
- Tenant is an individual instance of azure AD
- hAS ONLY users and group unllike ou and groups and all
- Security Prinicipals - An entity that can vbe uesd to authenticate or autorize to azure AD

Verify the email:
```powershell
$exists = Invoke-RestMethod -Uri "[https://login.microsoftonline.com/common/GetCredentialType"](https://login.microsoftonline.com/common/GetCredentialType%22 "https://login.microsoftonline.com/common/getcredentialtype%22") -ContentType "application/json" -Method POST -Body (@{"username"="harsh.btech19_sushantuniversity.edu.in#EXT#@harshbtech19sushantuniversi.onmicrosoft.com"; "isOtherIdpSupported" =  $true}|ConvertTo-Json) | Select -ExpandProperty IfExistsResult

$properties = [ordered]@{"OIUsername"=$user; "Exists"=$($exists -eq 0 -or $exists -eq 6)}

New-Object -TypeName PSObject -Property $properties
```




1. Install AADInternals

### Initial Access
Illicit Concent Grant

Requirements:
- Attacker tenant and attacker controled application 
- Some specific permissions mail.send, User.readbasic.all
- 365-stealer used to do that 

Anonymous Blob Access

Requirements:
- Any anonymous accessible blob storage and contining any sensitive API or key
- 


==How do we find out blobs : Microburst==


Exploiting Public Facing application

Requirements:
- Website having some sort of SSRF or RCE


Password Spray 
- Noisy 


### Priv Esc
Dynamic Groups
Check Role Assignment (You might be admin but not an admin) (Role Based Access Control Administrator)

Guest Account


### Exfiltration
Storage Accounts

