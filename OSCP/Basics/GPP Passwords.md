Whenever a Group policy is created there is an XML file which is created in the SYSVOL share with that config data, including any passwords associated with the GPP. For security, Microsoft AES encrypts the password before its stored as `cpassword` 

Basically passwords stores in form of cpasswords previously

This is corrected in 2014 update that prevented admins from putting passwords into GPP.

#### Tool 
```bash
# Decrypt this hash
gpp-decrypt  <cpassword>
```

