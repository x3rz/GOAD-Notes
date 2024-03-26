# Scanning network
Scanning network for up hosts.
`fping -a -g 10.54.12.0/24`
`fping -a -g 10.54.12.0  10.54.12.255`
If fping is not available:

`for i in {1..254} ;do (ping -c 1 192.168.1.$i | grep "bytes from" &) ;done`
Port Scanning
`for i in {1..65535}; do (echo > /dev/tcp/192.168.1.1/$i) >/dev/null 2>&1 && echo $i is open; done`





