## IP
censys.io
ipinfo.io
shodan.io


## Port scan
`Masscan`
`Nmap`

```bash
cat subdomains.txt | xargs -n1 host | grep "has address" | cut -d" " -f4 | sort -u > ips.txt

masscan -iL ips.txt -p0-65535 --rate=10000 -oL scan.txt
```


## JS
`waybackurls target.com | grep "\\.js" | xargs -n1 -I@ curl -k @ | tee -a content.txt`

```bash
curl http://web.archive.org/cdx/search/cdx/search/cds?url=*.$1/*&output=text&fl=original&collapse=urlkey

curl http://index.commoncrawl.org/CC-MAIN-2018-22-index\?url\=\*.$1\&output\=json |jq .url
```

**JSFinder**
`python JSFinder.py -u http://www.oppo.com`

```bash
cat domains.txt | waybackurls >> ePs
cat ePs | grep ".js"
```


## Files extraction
Same endpoints that being collected test for sensitive files via 
- dirsearch
- gobuster

> Dir brute on every endpoint

Example dict:
https://github.com/C1h2e1/MyFuzzingDict

Methods:
https://github.com/emadshanab/Acomplete-guide-to-dir-brute-force-admin-panel-and-API-endpoints
