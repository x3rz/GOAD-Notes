### What is SMB relay?
Instead of cracking hashed gatherd with Responder, we can instead relay those hashed to specific machines and potentially gain access.


### Requirements
- SMB signing must be disabled on the target (i.e no check in passing hash packets)
- Relayed user credentials must be admin on machine

#### Setup
Turn off the SMB and HTTP in the Responder.conf

Need a tool *ntmlrelayx.py*
```bash
python ntlmrelayx.py -tf target.txt -smb2support
```
This will relay the hash to the machines.

*Now we have responder and ntmlrelayx setup and now we need event to happen.*

#### Discovering Hosts with SMB signing enabled

```bash
nmap --script=smb2-security-mode.nse -p445 192.168.57.0/24
```

#### Mitigation 
- Enable SMB Signing on all devices
	- Pro: Completely stops the attack
	- Con: Can cause performance issues with file copies
- Disable NTML authentication on network
	- Pro: Completely stops the attack
	- Con: If Kerberos stops working, Windows defaults back to NTLM
- Account tiering
	- Pro: Limits domains admins to specific tasks (e.g only log onto servers with need for DA)
	- Con: Enforcing the policy may be difficult
- Local Admin restriction:
	- Pro: can prevent a lot of lateral movement.
	- Con: Potential increase in the amount service desk tickers
