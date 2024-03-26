1. Netstat - `netstat -ano | findstr TCP | findstr ":0"`
2. ps aux - `tasklist /v | findstr 2820`
4. Add user : `net user austin dangerpowers /add /domain`
	Add group: `Add-ADGroupMember -Identity "Exchange Windows Permissions" -Members austin`
	Check added user property if set properly or not:
	`Get-ADUser -Identity x3rz -Properties *`
	
	