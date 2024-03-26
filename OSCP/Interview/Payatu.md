**Always ask for feedback**
**Give legit examples in code or GTFO**
# Networking questions
- [ ] OSI Model and Layers
- [ ] Transport and Nework Layer
- [ ] ICMP & Traceroute working
- [ ] OS Detection using PIng
- [ ] Nmap is which layer tool and its os detection
- [ ] WPA-2 4way handshake
- [ ] ICMP,TCP,IP Header Length
- [ ] SSL Handshake
- [ ] What happens when we type google.com in browser
- [ ] Router working
- [ ] Subnetting
- [ ] Public/Private IP and Ranges
- [ ] Crpytography ( Asym | Sym)
- [ ] Encoding | Hashing | Encryption
- [ ] Pivoting
- [ ] Port Knocking
	- Ports are basically closed, trying openning ports by generating a connection request is Port Knocking. 
	- Knock service is used in linux for this purpose where we can define rules which port to open for what when connection is made
	- Example:

```bash
[options]
 logfile = /var/log/knockd.log
 interface = ens160

[openSSH]
 sequence = 571, 290, 911 
 seq_timeout = 5
 start_command = /sbin/iptables -I INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
 tcpflags = syn

[closeSSH]
 sequence = 911,290,571
 seq_timeout = 5
 start_command = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
 tcpflags = syn

```
- [ ] TCP 3-Way Handshake
- [ ] HTTP is stateless and HTTPS is stateful protocol
- [ ] SSH Local Forwarding
- [ ] Scenario Based Question
- [ ] SSH working( Detailed Description)
- [ ] Nmap switches and their working
- [ ] DHCP
- [ ] ARP
- [ ] Mac and Switching
- [ ] Lateral Movement
- [ ] Reverse | Bind Shell
- [ ] Web Shell
- [ ] Network Tools used in recon
- [ ] SOCKS Proxy and its working

# Web questions
- [ ] OWASP Top 10
- [ ] SOP & implementation & working & headers
- [ ] There is XSS on site via file upload thriugh pdf then how will you fetch the PII (By crafting malicious PDF with[ embeded javascript ](https://portswigger.net/research/portable-data-exfiltration))  (Actrobat, PDFlib, jsPDF, Chrome(PDFium))
- [ ] CORS & Headers
- [ ] CSP
- [x] Access Control | IDOR with Mitigation [Solution](https://www.geeksforgeeks.org/insecure-direct-object-reference-idor-vulnerability/) [More Solution](https://securecode.wiki/docs/lang/php/#a5--broken-access-control)
- [ ] Blind XSS
- [ ] Dom XSS | Source & Sync
- [ ] Template Injection
- [ ] Cookies vs Session
- [ ] Cookies Security Attributes ( HTTPonly & Secure Flag)
- [ ] Second Order SQLi and Remediation (Important)
- [x] CSRF | Mitigation (Give them code or GTFO)
- [ ] Scenario Question (CSRF,XSS,CORS)
- [x] Anti CSRF Toke Implementation in Response Body | Headers which is secure
- [ ] Recon Approach
- [ ] SQL Testing on Login Page
- [ ] Buisness Logic
- [x] JWT Basics and Common Attacks
- [x] Oauth Working (2.0)
- [x] SSRF
- [ ] Frida 
- [ ] He asked about Blogs?
- [ ] Session vs Token Based Authentication Difference
- [ ] Threat | Risk | Vulnerability ( Risk = Threat * Vulnerability)
- [ ] VA | PT 
- [ ] Block vs Stream Ciphers
- [ ] LFI vs RFI
- [ ] XXE | Mitigation
- [ ] SSRF & Blind SSRF
- [ ] RCE
- [ ] Broken Authentication
- [x] LFI to RCE leading to Log Poisoning
- [ ] HTTP 1.0 vs 1.1
- [ ] Ping Sweep Program (Any Language)
csrf
xss
sop
ssrf
lfi
arbitrary file read
afr with lfi
nmap default scan when you are root and when you are normal user.
sqli inject 
	- how it works?
	- how parameterise query works and how it protects
	- stored procedures in query
Nmap Default Scan:
SYN (-sS) scan deafult if root
Connect Scan (-sT) as a normal user




*C Question can be present in the interview so please prepare well.*