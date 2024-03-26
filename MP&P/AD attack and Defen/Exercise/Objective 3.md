- ACL for the Users Group
```powershell
Get-ObjectAcl -SamAccountName "users"
```
- ACL for the Domain Admins Group
```powershell
Get-ObjectAcl -SamAccountName "Domain Admins"
```
- All modify rights/permissions for the student
```powershell
Invoke-ACLScanner -ResolveGUIDs | ?{$_.IdentityReference -match "user"}
```


