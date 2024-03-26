**Initial Scan: Shlok Bhaiya** (Takes avg time of 2: 19min)
`sudo nmap -sSCV -A -T4 -p- 192.168.101.123 -oA output.txt --min-rate=5000`


Also try this if you have time 
`nmap -Pn -sC -sV -p- -vvvvvvv --reason --min-rate=1000 -T4 -oA all_tcp 10.10.10.27`
*This one took 4mins*

**Masscan:**
`sudo masscan -p80 10.11.1.0/24 --rate=2000 -e tun0 --router-ip 10.11.0.1`


**Normally**

nmap -T4 -A -p- 

nmap -sV -v -Pn 
nmap -T4 -p- 

nmap -T4 -A -sV -v -Pn -p- 