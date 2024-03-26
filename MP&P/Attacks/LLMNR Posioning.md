### What is LLMNR?

Link local mulicast name res

- Basically a DNS used to identify hosts when DNS fails to do so.
- Previously Known as NBT-NS.
- *Key Flaw:* Service utilize a user's username and NTLMv2 hash when appropriately responded to.

![[Pasted image 20220113141543.png]]

#### Tool Used
*Responder.py* - run this in the moring or after lunch break so that you can capture hashed 

*This is MITM situation in which attacker responds to the client query and gets the hash*

```bash
python Responder.py -l tun0 -rdw
```



#### Mitigation
-   **Disabling LLMNR** – LLMNR can be turned-off through the group policy editor, under the “policy setting” menu under Local Computer Policy > Computer Configuration > Administrative Templates > Network > DNS Client.
-   **Disabling NBT-NS** – NBT-NS can be turned off through the Network Connection Settings. Navigate to Network Connections > Internet Protocol Version 4 > Properties > General > Advanced > WINS, then select “Disable NetBIOS over TCP/IP”.
-   **Network Traffic Filtration**– host-based security products can be used to block LLMNR, NBT-NS and mDNS traffic.
-   **SMB Signing** – as mentioned above, SMB Signing can be used to prevent NTLM relay attacks by digitally signing the data transferred.
	-   **Monitoring** – hosts should be monitored for (1) traffic on LLMNR and NBT-NS ports (UDP 5355 and 137), (2) event logs with event IDs 4697 and 7045 (relevant to relay attacks)and (3) changes to registry DWORD _EnableMulticast_ under _HKLM\Software\Policies\Microsoft\Windows NT\DNSClient
- If could not disable LLMNR then ask for strong passwords
- Require Network Access control.
