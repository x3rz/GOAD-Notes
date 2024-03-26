### Group Policy Preferences
- These allowed admins to create policies using embedded credentials
- These credentials were encrypted and places in a "cpassword"
- The key was accidentally released (whoops)
- Patched in MS14-025, but doesnt prevent uses

[Resource](https://www.rapid7.com/blog/post/2016/07/27/pentesting-in-the-real-world-group-policy-pwnage/)

Whenever a Group policy is created there is an XML file which is created in the SYSVOL share with that config data, including any passwords associated with the GPP. For security, Microsoft AES encrypts the password before its stored as `cpassword` 

Basically passwords stores in form of cpasswords previously

This is corrected in 2014 update that prevented admins from putting passwords into GPP.

#### Tool 
```bash
# Decrypt this hash
gpp-decrypt  <cpassword>
```