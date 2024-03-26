1. Deleted content can be recovered using `grep` command 
	```bash
	grep -binary-files=text -context=100 'root' /deb/sdb > /tmp/root.txt
	```

2. Escape RBASH restriction
	https://www.hacknos.com/rbash-escape-rbash-restricted-shell-escape/