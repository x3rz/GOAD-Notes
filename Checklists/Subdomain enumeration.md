# Ultimate tip
if target is scope bound lets say www.xyz.com then do also enum for its subdomans ex : abc.www.xyz.com there is a chance you will find more and widen your scope

**If you found out subdomains which redirects to other one then use gau on found subdomains which redirects to other one maybe you able to find out some parameters which are not aviable on subdomains and still works on redirected one.**

# Meta-Finder (TO-ADD)
Use for finding documents in subdomains 

# Notes
- subfinder isnt that effective comare to assetfinder and amass active scan. *for online domains*
	- Also add subscraper in it
		- Subscraper Feteches from {run for 1 and 2}
			1. DNS-Brute
			2. DNSBufferOverRun
			3. crt.sh
			4. DNS sumpster
			5. ThreatCrowd
- Nahamsec .bashrc profile is very useful
- From RapidDNS, Riddler.io, Archive, CertSpotter, JLDC, securitytrails, DNS over, sonar.omnisint.io,decon.dev, FFUF, 
- finding subdomains by ASN number from amass



- altdns, massdns
- **Find sibdomains of subdomains**
	- `./altdns.py -i subdomains.txt -o data_output -w words.txt -r -s output.txt`

**Shodan alternative fofa.so**

--------
## Up-till now

- Amass + assetfinder + subscraper + subfinder
*Find subdomains of subdomains now*
- altdns
*Find live domains*
- use `httprobe` for better results else in oneliners use the same.
- RCE via httpx :) see [[Nuclei]]


*Find active DNS resolutions*
- masscan , massdns
[m1](https://medium.com/@noobhax/my-recon-process-dns-enumeration-d0e288f81a8a)
[m2](https://bugbountytuts.files.wordpress.com/2019/01/dirty-recon-1.pdf)

- Find IP ranges now
[IP](https://bgp.he.net/search?search%5Bsearch%5D=Smule&commit=Search)
[IP2](https://whois.arin.net/ui/query.do)
(Fire nmap and run NSE scripts on those discovered IP ranges)
(Do content discovery on each IP)


#### Endpoints 
waybackurl, gau,  (JSfinder, zscanner, JS-Scan)

--------

ZSeanos Methodology

`amass enum -brute -active -d domain.com -o amass-output.txt`

Emad Shanab:

`amass enum -brute -w /root/HugeDNS.txt -d target -o target.out`
Also check his bruteforce github repo got these


> Find http|https server by httprobe
`cat amass-output.txt | httprobe -p http:81 -p http:3000 -p https:3000 -p http:3001 -p http:8000 -p http:8080 -p https:8443 -c 50 | tee online-domains.txt`

> Find more gems
`cat amass-output.txt | dnsgen - | httprobe`

> Passing also endpoints and files in aquatone 
`cat domains-endpoints.txt | aquatone`


TBHMv4

- Linked and JS Discovery
- Subdomain Scraping
- Subdomain Bruteforce

> **Subdomainizer** scans JS files and finds refrences
> **subscraper** use recursion for only sub-domains

> Enum via favicon


Aditya Shinde

> subfinder -d target.com 
- **finddomain**, assetfinder, 
- brokenlink hijacker, ahrtfs.com

**Twitter**

*Subdomain Enumeration*
> subfinder -d domain.com --silent | httprobe | tee -a subdomains.txt
> amass enum --passive -d domain.com | httprobe | tee -a subdomain.txt
> assetfinder -subs-only domain.com | httprobe | tee -a subdomain.txt

*Find endpoints*
> cat subdomains.txt | waybackurls >> wayback.txt
> cat subdomains.txt | hakrawler -depth 3 -plain >> spider.txt

*Find all parameter*
> python3 paramspider.py --domain domain.com --exclude svg,jpg,css,js --output domainParam.txt
> cat subdomains.txt | waybackurls | grep "=" | tee -a domainParam.txt

*Extract .js Subdomains*
> echo "domain" | haktrails subdomais | httpx -silent | getJS --complete | anew JS
> echo "domain" | haktrails subdomais | httpx -silent | getJS --complete | tojson | anew JS1

*Search .json subdomain*
> assetfinder domain.com | waybackurls | grep -E ".json(?:onp?)?$" | anew

*Search subdomains using github and httpx*
> ./github-subdomains.py -y APYKEYGITHUB -d domain.com | httpx --title

*Subdomains in cert.sh*
> curl -s "https://crt.sh/?q=%25.att.com&output=json" | jq -r '.[].name_value' | sed 's\\/*\\.//g' | httpx -title -silent | anew



**Devansh bhattnam**

![](https://miro.medium.com/max/875/1*Qnmfe6bT8RX_FuvgRVconA.png)


OSINT/Recon

[infosecmatters](https://www.infosecmatter.com/bug-bounty-tips/#finding-subdomains)
-   [BBT1-7](https://www.infosecmatter.com/bug-bounty-tips-1/#7_finding_subdomains) – Finding subdomains
-   [BBT1-8](https://www.infosecmatter.com/bug-bounty-tips-1/#8_curl__parallel_one-liner) – Curl + parallel one-liner
-   [BBT2-1](https://www.infosecmatter.com/bug-bounty-tips-2-jun-30/#1_find_subdomains_with_securitytrails_api) – Find subdomains with SecurityTrails API
-   [BBT3-3](https://www.infosecmatter.com/bug-bounty-tips-3-jul-21/#3_find_related_domains_via_favicon_hash) – Find related domains via favicon hash
-   [BBT3-7](https://www.infosecmatter.com/bug-bounty-tips-3-jul-21/#7_find_subdomains_using_rapiddns) – Find subdomains using RapidDNS
-   [BBT4-10](https://www.infosecmatter.com/bug-bounty-tips-4-aug-03/#10_a_recon_tip_to_find_more_subdomains_shodan) – A recon tip to find more subdomains (Shodan)
-   [BBT7-10](https://www.infosecmatter.com/bug-bounty-tips-7-sep-27/#10_find_subdomains_using_asns_with_amass) – Find subdomains using ASNs with Amass