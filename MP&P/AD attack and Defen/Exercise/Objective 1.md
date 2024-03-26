- Users

```powershell
Get-NetUser | select cn
```
- Computers

```powershell
Get-NetComputer | select name
```
- Domain Administrators

```powershell
# All the Adinistrators
Get-NetGroupMember -Identity "Administrators" -Recurse

# All Domain Administrators
Get-NetGroup "Domain Admins"


```

- Enterprise Administrators

```powershell
Get-NetGroup "Enterprise Admins"
```
- Shares

```powershell
# Get sahres
Get-NetShare

# Get All the shares list
Invoke-ShareFinder -Verbose
```



