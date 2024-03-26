hashcat

`hashcat -m 100 hash1_2.txt /usr/share/wordlists/rockyou.txt`

hash1_2.txt includes hashes and modes also 

`https://hashes.com/en/decrypt/hash`

>never use --force option gives false positive

Prefix	Algorithm
``$1$``	md5crypt, used in Cisco stuff and older Linux/Unix systems
``$2$``, ``$2a$``, ``$2b$``, ``$2x$``, ``$2y$``	Bcrypt (Popular for web applications)
``$6$``	sha512crypt (Default for most Linux/Unix systems)


>Bcrypt

`hashcat -m 3200 hash1 /usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt`
>SHA%!@crypt

`hashcat -m 1800 hash1 /usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt`

[Cheatsheet](https://github.com/frizb/Hashcat-Cheatsheet)