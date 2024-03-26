- [ ] Find the scope
- [ ] Accumlate all the findings and sort and uniq them
	- [ ] Start the amass active scan (When you have alot of time)
	`amass enum -brute -active -d domain.com -o amass-output.txt`
	- [ ] Start subscraper subdomains
	- [ ] Start assetfinder subdomains
	- [ ] FIndomain?
	- [ ] fuff to bruteforce for subdoamins ( pending implementation)
	`ffuf -u http://FUZZ.$domain/ -t 200 -p 1.0-2.0 -w ~/wordlists/subdomains.txt -H "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.77 Safari/537.36" -mc 200,404,403 -o ffuf.json -s &> /dev/null`
- [ ] Pass the list in httprobe


**Quick Check**
- [ ] Pass the list in httpx for small finding [[Pasted image 20210719162830.png]]
```bash
httpx -l online.txt -path /sm/login/loginpagecontentgrabber.do -threads 100 -random-agent -x GET -title -tech-detect -status-code -follow-redirects -title -mc 200
```
- [ ] Sort/Uniq Extract Endpoints,parameters 
	- [ ] pass the list in *waybackurls*
	- [ ] pass the list in *gauplus* (its more advance than gau)
		`cat ../win-d.txt| httprobe | gauplus --random-agent -b eot,jpg,jpeg,gif,css,tif,tiff,png,ttf,otf,woff,woff2,ico,pdf,svg,txt,js  -o gauplus.tx`
	- [ ] pass the list in hakrawler
		`cat domains.txt| hakrawler -depth 3 -plain >> spider.txt`
	- [ ] pass the list in *paramspider* , *Arjun*, *param-miner*
		`python3 paramspider.py --domain domain.com --exclude svg,jpg,css,js --output domainParam.txt`
		- [ ] Fuzzing via dictonary [w1](https://gist.github.com/nullenc0de/9cb36260207924f8e1787279a05eb773), [w2](https://github.com/s0md3v/Arjun/tree/master/arjun/db),[w3](https://github.com/PortSwigger/param-miner/blob/master/resources/params)
	- [ ] Use JSFinder.js , subJS(takes subdomains directly)

		`cat domain.txt | httpx -silent | subjs | anew -q subjs`
	- [ ] Find stuff out of js files
	- [ ] Using gau output
```bash
	assetfinder site.com | gau|egrep -v '(.css|.png|.jpeg|.jpg|.svg|.gif|.wolf)'|while read url; do vars=$(curl -s $url | grep -Eo "var [a-zA-Zo-9_]+" |sed -e 's, 'var','"$url"?',g' -e 's/ //g'|grep -v '.js'|sed 's/.*/&=xss/g'):echo -e "\e[1;33m$url\n" "\e[1;32m$vars";done
```
**Quick Check**
- [ ] Vulnerability check using *nuclei + [Cent](https://github.com/xm1k3/cent) (@xm1k3)* on *domains and endpoints* and take screenshots
```bash
cat domains.txt | httpx -silent | xargs -n 1 gospider -o output -s ; cat output/* | egrep -o 'https?://[^ ]+' | nuclei -t ~/nuclei-templates/ -o result.txt

cat domanins.txt | nuclei -t teamplates

```

- [ ] Check screenshots of domains/Endpoints in Aquatone
	- [ ] Send endpoints
	- [ ] Send domains
	- [ ] Pass nuclei output for quick check

- [ ] Check for Subdomain takeover
	- [ ] Use nuclei
	- [ ] Use Subzy


- [ ] Check Port Scan (only when you are targting on specific target)
```bash
naabu -ports full -exclude-ports 80,443 -hL target.txt -o out.txt

cat subdomains.txt | xargs -n1 host | grep "has address" | cut -d" " -f4 | sort -u > ips.txt
masscan -iL ips.txt -p0-65535 --rate=10000 -oL scan.txt


cat alive.txt | sed 's/https\?:\/\///' > scan-ip.txt
rustscan 'scan-ip.txt' -p --ulimit 5000 -- -n -sV -Pn -oA scan-result
```

- [ ] Find AEM
	- [ ] Pass list in aem_hacker
	`python3 aem_discoverer.py --file ../targets/redbull/online.txt --workers 150 `
	- [ ] Fuzzing list [w1](https://github.com/emadshanab/Adobe-Experience-Manager), [w2](https://github.com/emadshanab/AEM-List), 
	- [ ] 

- [ ] Check for Aquatone screenshots and then procced to select the targets 

- [ ] SQLi finding automate
	```bash
	cat subdomains.txt | httpx -silent | anew | waybackurls | gf sqli >> sqli ;sqlmap -m sqli --batch --random-agent --level 5 --risk 3
	```

- [ ] S3 buckets?
	- [ ] Find bucket name by appending in url `?AWSAccessKeyId=[Valid_ACCESS_KEY_ID]&Expires=1766972005&Signature=ccc`

- [ ] Backup file bruteforce
	`python3 dirsearch.py -e conf,config,bak,backup,swp,old,db,sql,asp,aspx,aspx~,asp~,py,py~,rb,rb~,php,php~,bak,bkp,cache,cgi,conf,csv,html,inc,jar,js,json,jsp,jsp~,lock,log,rar,old,sql,sql.gz,sql.zip,sql.tar.gz,sql~,swp,swp~,tar,tar.bz2,tar.gz,txt,wadl,zip,log,xml,js,json  -u https://target`










## To-Do 
During the making of endpoints /parameters / js files like with the gospider
Do one thing to make directory for each jsfiles, endpoints, robots, etc to sort the content more 

- Implementation of search sploit / make nuclei templates
	- it should provided version and software name
	- it should provide all the suggested exploits related to that

