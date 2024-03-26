[Crack the hash 2](https://tryhackme.com/room/crackthehashlevel2)
# Hash Identification
**Haiti**
Install Haiti:
`sudo pacman -S haiti`
To identify the hash:
`haiti -e <hash>`

# Wordlist Download
**wordlistctl**
Installation:
`sudo pacman -S wordlistctl`
Usage:
Search
`wordlistctl search rockyou`
`wordlistctl search facebook`
Downloading
`wordlistctl fetch -l rockyou`
Listing
`wordlistctl list -g fuzzing`

# Cracking Modes
1. **Wordlist mode**: which consist in trying all words contained in a dictionary. For example, a list of common passwords, a list of usernames, etc.
2. **Incremental mode**: which consist in trying all possible character combinations as passwords. This is powerful but much more longer especially if the password is long.
3. **Rule mode**: which consist in using the wordlist mode by adding it some pattern or mangle the string. For example adding the current year, or appending a common special character.


**There are 2 ways of performing a rule based bruteforce:**
1. Generating a custom wordlist and using the classic wordlist mode within it.
2. Using a common wordlist and tell the cracking tool to apply some custom mangling rules on it.

The second option is much more powerful as you wont waste gigabytes by storing tons of wordlists and waste time generating ones you will use only one time. Rather having a few interesting lists and apply various mangling rules that yo u can re-use over different wordlist.

[**JohnTheRipper rules**](https://www.openwall.com/john/doc/RULES.shtml)

I'll give you the main ideas of mutation rules, of course several can be combined together.

-   **Border mutation** \- commonly used combinations of digits and special symbols can be added at the end or at the beginning, or both
-   **Freak mutation** \- letters are replaced with similarly looking special symbols  
    
-   **Case mutation** \- the program checks all variations of uppercase/lowercase letters for any character
-   **Order mutation** \- character order is reversed
-   **Repetition mutation** \- the same group of characters are repeated several times
-   **Vowels mutation** \- vowels are omitted or capitalized
-   **Strip mutation** \- one or several characters are removed
-   **Swap mutation** \- some characters are swapped and change places
-   **Duplicate mutation** \- some characters are duplicated
-   **Delimiter mutation** \- delimiters are added between characters


## Adding rules 
open text edtior and make john-local.conf and then write the following 
```
[List.Rules:Rule_Name]
$[0-9]$[0-9] 		# {rules to make under rule name}
```


[Hashcat cheatsheet](https://github.com/frizb/Hashcat-Cheatsheet)

Universal rule:
[OneRuleToRuleThemAll](https://github.com/stealthsploit/Optimised-hashcat-Rule.git)
* Make wordlist of word or use it along bruteforce with the rules and dictonary

Rule along side of bruteforce:
`hashcat -m 3200 hash.txt /wordlist/rockyou.txt -r OneruleforRuleThemAll.rule`

Making wordlist:
`echo 'word' | hashcat -r OneRuleForRuleThemAll.rule --stdout wordlist.txt `

# Single mode
#### Single Crack Mode

So far we've been using John's wordlist mode to deal with brute forcing simple., and not so simple hashes. But John also has another mode, called Single Crack mode. In this mode, John uses only the information provided in the username, to try and work out possible passwords heuristically, by slightly changing the letters and numbers contained within the username.

  

#### Word Mangling  

The best way to show what Single Crack mode is,  and what word mangling is, is to actually go through an example:

If we take the username: Markus

Some possible passwords could be:

-   Markus1, Markus2, Markus3 (etc.)
-   MArkus, MARkus, MARKus (etc.)
-   Markus!, Markus$, Markus\* (etc.)

This technique is called word mangling. John is building it's own dictionary based on the information that it has been fed and uses a set of rules called "mangling rules" which define how it can mutate the word it started with to generate a wordlist based off of relevant factors for the target you're trying to crack. This is exploiting how poor passwords can be based off of information about the username, or the service they're logging into.  

####   

#### GECOS

John's implementation of word mangling also features compatibility with the Gecos fields of the UNIX operating system, and other UNIX-like operating systems such as Linux. So what are Gecos? Remember in the last task where we were looking at the entries of both /etc/shadow and /etc/passwd? Well if you look closely You can see that each field is seperated by a colon ":". Each one of the fields that these records are split into are called Gecos fields. John can take information stored in those records, such as full name and home directory name to add in to the wordlist it generates when cracking /etc/shadow hashes with single crack mode.

  

#### Using Single Crack Mode

To use single crack mode, we use roughly the same syntax that we've used to so far, for example if we wanted to crack the password of the user named "Mike", using single mode, we'd use:  

`john --single --format=[format] [path to file]`

`--single` \- This flag lets john know you want to use the single hash cracking mode.

**Example Usage:**

john --single --format=raw-sha256 hashes.txt

**A Note on File Formats in Single Crack Mode:**

If you're cracking hashes in single crack mode, you need to change the file format that you're feeding john for it to understand what data to create a wordlist from. You do this by prepending the hash with the username that the hash belongs to, so according to the above example- we would change the file hashes.txt

**From:**  

1efee03cdcb96d90ad48ccc7b8666033

**To**

mike:1efee03cdcb96d90ad48ccc7b8666033


# Tips and Tricks
1. use fastrack.txt for faster results.