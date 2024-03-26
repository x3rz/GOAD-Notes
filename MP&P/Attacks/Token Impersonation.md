#### What are tokens?
Temprorary keys that allow you access to a system.network without having to provide credentials each time you access a file
Just like cookies in computer

#### Types of Tokens
There are two types of tokens
 - Delegate - created for logging into a machine or using remote Desktop
 - Impersonate = "Non-Interactive" suck as attaching a network drive or a domain logon script


 We can archive this using `mimikatz` and `incognito`

#### Defense
- Limit user/group token creation permissions
- Account tiering
- Local admin restriction
