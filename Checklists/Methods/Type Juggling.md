# Proof of concept

Vuln variable: `$_POST['admin_password']`  
Snippet:

```
        $admin_password = $_POST['admin_password'];
        $admin_password_conf = $_POST['admin_password_conf'];
        ...
        test(
            _t('CHECKING_THE_ADMIN_PASSWORD_CONFIRMATION').' ...',
            $admin_password == $admin_password_conf,
            _t('ADMIN_PASSWORD_ARE_DIFFERENT'),
            1
        );  
```

## Payload

Go to the install step of yeswiki, in the first password field under Administrator account insert

```
0e12345
```

In the password confirmation field insert:

```
0e54321
```

Observe that these are two different passwords, but yeswiki allows to continue the installation