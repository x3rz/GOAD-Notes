Port 79
--------------

```bash 
git clone https://github.com/pentestmonkey/finger-user-enum.git

# Enumerate single user
perl finger-user-enum.pl -u root -t 10.10.10.76

# Enumerate multiple user
-   -   perl finger-user-enum.pl -U /usr/share/seclists/Usernames/Names/names.txt -t 10.10.10.76

# Show the home directory of the user
-   finger -sl root@10.10.10.76

```

