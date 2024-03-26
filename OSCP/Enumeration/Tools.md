Tools

# Nmap

`nmap -sSCV ip`

-sS - Stealth scan
-sC - Default scripts scan
-sV - version scan

More flags

-sU - UDP scan
-sn - ping scan
`nmap -Pn -sC -sV -p- -vvvvvvv --reason --min-rate=1000 -T4 -oA all_tcp 10.10.10.27`
most fast result like rustscan
# Netcat

-l - listen connection
-v - verbose mode
-p - port to listen on
-e - specify the program to execute
-u - connect to udp ports

# Gobuster

-U & -P for in case of basic auth
-s - specify the status code
-k - skip ssl certificate verification
-a - user agent
-H - HTTP header 
-x - Adapt the extensions

```bash
gobuster dir -u http://laboratory.htb -w /usr/share/dirb/wordlists/common.txt -s 200,204,301,307,401,403
```

This is in case of `Error: the server returns a status code that matches the provided options for non existing urls. http://laboratory.htb/709f484d-8140-426a-81bc-1f28bab1c10c => 302. To force processing of Wildcard responses, specify the '--wildcard' switch`
these errors.

# DirSearch.py
`dirsearch.py -u 10.10.x.x:port -E -x 400,500 -r -t 5- -w /directory-list-2.3-medium.txt`


# Nikto

-h specify the host
-nossl - disable ssl 
-ssl - force ssl
-id - specify the username and password u:p
-plugins+ - plugin to use
