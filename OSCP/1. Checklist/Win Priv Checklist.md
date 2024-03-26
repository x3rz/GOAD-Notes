- [ ] Check for passwords in config files 
- [ ] Check for saved credetials
- [ ] Check for registery modification or password in registries 
- [ ] Check for Service Exploits
- [ ] Check for scheduled tasks
- [ ] Check for insecure GUI apps
- [ ] Check for startup apps
- [ ] Check for kernel Exploits (windows-exploit-suggester)
- [ ] Check for Potatoes
	- Windows m whoami /priv se Impersonate aur CreateGlobal wale token ho to seedha PrintSpoofer (Windows 8.1 ke baad) aur agar 7 chalri ho to juicypotato or hot potato(TCM)
	- Wrna agar token ho hi na to same cheez ki haand ke files dekho credentials ke liye
	- Wrna Seatbelt.exe
- [ ] Run WinPEASany 
- [ ] Run Seatbelt.exe

> - [ ] If you encounter Active Directory then for privesc use `BloodHound` and [`SharpHound`](https://github.com/BloodHoundAD/BloodHound/raw/master/Collectors/SharpHound.exe) [Poweshell-version](https://raw.githubusercontent.com/BloodHoundAD/BloodHound/master/Collectors/SharpHound.ps1) for enumerating AD's.
	> Use `SharpHound.exe --CollectionMethod All` to collect data for `BloodHound`
----


[[Win PrivEsc]]
- [ ] Try changing file permission using `icacls root.txt /grant alfred:F`
- [ ] Check for potatoes i.e SetTokenImpersonation Enabled, SetGlobal in some cases -- juicypotato needs only Impersonation one.
	- [ ] If system is old (Have *Document and Settings* directory) then go for [churrasco.exe](https://github.com/Re4son/Churrasco/raw/master/churrasco.exe)
- [ ] If have RDP check for browser history and more just navigate.
- [ ] `reg save HKLM\SYSTEM C:\windows\temp\SYSTEM` to dump SYSTEM and SAM files.
- [ ] `whoami /priv` - check for token impersonation.
- [ ] `netstat -ano | findstr TCP | findstr ":0"`
- [ ] Look for Installed programs
- [ ] WinPEAS
- [ ] SeatBelt.exe
- [ ] Manuall check files like with accesschk.exe for software installed on the system.

*In the end*
- [ ] Copy the windows version and search for its [kernel exploits](https://github.com/SecWiki/windows-kernel-exploits) 
- [ ] [Windows exploit suggester ](https://github.com/AonCyberLabs/Windows-Exploit-Suggester)
```python 
windows-exploit-suggester.py --database <xls database  of exploits provided with the script> --systeminfor <systeminfo saved in a txt file> --ostext <Os> -1
```