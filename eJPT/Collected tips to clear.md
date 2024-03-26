1. Most important one is `Take NETWORKS module seriously`. I kind of skimmed those module which talked about networking because I felt like I know this which caused problems during exam.

2. Learn to pivot if you can’t pivot you won’t get far in your pentest. By learn to pivot I mean understand it completely even if you have to read something from outside the course just understand it properly.

NOTE: If you would like to try out a Vulnhub VM that covers pivoting then do Tempus fugit by 4ndr34z & DCAU. This would really give you an idea.

3. When you start your pentest try to understand the bigger picture because there is a storyline behind the pentest you are doing, which you’ll understand if you’d read the letter of engagement properly.
4. Nothing which was not taught in the material will not come in the exam. Actually if you would do the labs properly you’ll start to see few similarities during the exam.
5. 20 multiple choice questions with one or more than one.
6. Nothing which was not taught in the material will not come in the exam and if you’re having a connection error , the service which you’re trying to attack is not the one , keep that in mind.
7. Read all the questions before starting your penetration test because this gives you what to expect in the exam , like for example, if you see questions about XSS, you know there’s going to be a web server in the subnet which you have to attack. Also don’t waste your time in useless heavy nmap scans, the basic scan will suffice.

8. Good fundamental penetration testing methodolgy

9. Enumerate, EnUmerAtE, ENUMERATE!
This means be familiar with nmap, netdiscover, metasploit scanners, sqlmap, gobuster, <insert other scanner/enumeration tool here>. Especially nmap, nmap will always be your best friend forever.
10. Know how to exploit services, and also know how to use services
Metasploit usage is allowed, so if you are brand new, familiarity with the Metasploit Framework goes a long way. More importantly you should understand how to interact with services normally. You may be expected to interact with a SQL database of some sort, or an ftp server. You need to be sure you know how to do those things so you do not get tripped up by fundamentals.
11. Web Exploitation
SQL Injection
SQLMap is allowed, and is great to get the job done quick, but you better know how to do it manually too, or you WILL miss a few things.
Cross Site Scripting (XSS)
12. PIVOTING!!!
This exam relies heavily on understanding how to pivot, if you can’t pivot you won’t get far.
13. Password Cracking
Familiarize yourself with hashcat and john the ripper, you will need them
14. Brute Forcing
hydra is your friend, but if that doesn’t work, a little python scripting goes a long way!
15. **routing;** understand the roads on which you will be driving
ip route
prints the routing table for the host you are on
ip route add [new network] via [gateway]
add a route to a new network if on a switched network and you need to pivot
16. Just some tools that I used:
fping
nmap
gobuster
hydra
smbclient
enum4linux
nessus
zap
msfconsole
sqlmap
john
