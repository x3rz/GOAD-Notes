- List all the OUs

```powershell
Get-NetOU | select name
```

- List all the computers in the cyberlix OU

```powershell

Get-NetOU cyberlix | %{Get-NetComputer -ADSPath $_}

Get-DomainComputer -SearchBase "ldap://OU=..."

```
- List the GPOs

```powershell
Get-NetGPO
```
- Enumerate GPO applied on the cyberlix OU

```powershell
Get-NetOU "cyberlix" | %{Get-NetGPO}
```

