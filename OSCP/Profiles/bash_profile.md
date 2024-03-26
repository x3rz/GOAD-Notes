```bash
mkfol(){
mkdir $1
mkdir $1/scans
touch $1/founds.md
}
cplookupsid(){
cp /usr/share/doc/python3-impacket/examples/lookupsid.py .
}
tcpscan(){
sudo nmap -v -sSCV -A -T4 -p- $1 -oA scans/output.txt --min-rate=5000
}

udpscan(){
sudo nmap -sU --top-ports 20 -sV -oA scans/udp $1
}

se(){
searchsploit --nmap scans/nmap.out.xml
}

startsmb(){
echo "Enter the command: net use \\IP\share /u:df df"
python3 /usr/share/doc/python3-impacket/examples/smbserver.py share . -smb2support -username df -password df
}
startsmbp(){
python3 /usr/share/doc/python3-impacket/examples/smbserver.py share . -smb2support -debug
}
cppower(){
cp /usr/share/powershell-empire/empire/server/data/module_source/situational_awareness/network/powerview.ps1 .
}
cpgetnpusers(){
cp /usr/share/doc/python3-impacket/examples/GetNPUsers.py .
}
cpgetadusers(){
cp /usr/share/doc/python3-impacket/examples/GetADUsers.py .
}
anarchy(){
/home/x3rz/Desktop/OSCP-prep/AD/username-anarchy/username-anarchy --input-file $1 > ./users_combi
}

alias kerbrute="/home/x3rz/Desktop/OSCP-prep/AD/kerbrute_linux_amd64"
alias burp="java -javaagent:/home/x3rz/Desktop/burpsuite/BurpSuiteLoader_v2021.8.jar -noverify -jar /home/x3rz/Desktop/burpsuite/burpsuite_pro_v2021.8.jar"

```