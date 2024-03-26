# Powershell
Powershell is the Windows Scripting Language and shell environment that is built using the .NET framework.

This also allows Powershell to execute .NET functions directly from its shell. Most Powershell commands, called cmdlets, are written in .NET. Unlike other scripting languages and shell environments, the output of these cmdlets are objects - making Powershell somewhat object oriented. This also means that running cmdlets allows you to perform actions on the output object(which makes it convenient to pass output from one cmdlet to another). The normal format of a cmdlet is represented using Verb-Noun; for example the cmdlet to list commands is called Get-Command.

Common verbs to use include:
Get
Start
Stop 
Read
Write
New
Out
To get the full list of approved verbs, visit [this](https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/approved-verbs-for-windows-powershell-commands?view=powershell-7) link.

# Commands
> Get-Help [command] - man [command]
> As ps commands are verbs-noun format so to get whole list of commands by `Get-Command verb-*` or `Get-Command *-noun`
>  the output of every cmdlet is an object. If we want to actually manipulate the output, we need to figure out a few things:
>  1. passing output to other cmdlets
>  2. using specific object cmdlets to extract information.

> Pipeline `|` works same way as in linux

Select-Object -Property Mode,Name - grep  
Ex: 
```powershell
Get-ChildItem | select-object -property Name, Mode
Get-ChildItem | Select-Object -Property Name,Mode -First 3
```

  first - gets the first x object
  last - gets the last x object
  unique - shows the unique objects
  skip - skips x objects
> Get-ChildItem - ls 

# Filtering Objects

When retrieving output objects, you may want to select objects that match a very specific value. You can do this using the `Where-Object` to filter based on the value of properties. 

The general format of the using this cmdlet is 

`Verb-Noun | Where-Object -Property PropertyName -operator Value`

`Verb-Noun | Where-Object {$_.PropertyName -operator Value}`

The second version uses the $_ operator to iterate through every object passed to the Where-Object cmdlet.

**Powershell is quite sensitive so make sure you don't put quotes around the command!**

Where ``-operator`` is a list of the following operators:

- Contains: if any item in the property value is an exact match for the specified value
- EQ: if the property value is the same as the specified value
- GT: if the property value is greater than the specified value
For a full list of operators, use this link.

EX: Check stopped processes:
`Get-Service |Where-Object -Property Status -eq Stopped`

# Sort Object
This is similar to the `sort` command of linux 
`Verb-Noun | Sort-Object`


### Get-Command & Get-Help are your best frnds!

```powershell
# Using Get-Help command
Get-Help Get-Command -Examples
```

```powershell
# Get-Command gets all the cmdlets installed on the current device/

Get-Command Verb-*
Get-Command *-Noun
```

```powershell\
# Object Manipulation

Verb-Noun | Get-Member
```

```powershell
# Filter Objects

Verb-Noun | Where-Object -property PropertyName -operator Value
Verb-Noun | Where-Object {$_.propertyName -operator Value}

#Example
Get-Service | Where-Object -Property Status -eq Stopped
```
-operator is a list of the following operators.
Contains - Of any on the property value is an exact match for the specified value
EQ - if the property value is the same as the specified value.
GT - If the property value is greater than the specified value.



```powershell
# Sort Objects

Verb-Noun | Sort-Object
```

Basic Powershell Commands:
```powershell
# Computer details
Get-ComputerInfo

# Start a process/program
Start-Process notepad.exe

#List all the processes runnign ATM
Get-Process -name noetpad

#Export command output to CSV file
Get-Process | Export-csv output.csv

# See the content of file
cat output.csv

# Get file Hash
Get-FileHash output.csv
Get-FileHash -Algorithm SHA256 output.csv
```

**Pentesting:**
```powershell
# Finding missing patches
Get-HotFix
```

```powershell
```