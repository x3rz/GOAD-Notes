[Remote File Copy](https://attack.mitre.org/techniques/T1105/) Files may be copied from an external adversary-controlled system through the Command and Control channel to bring tools into the victim network or through alternate protocols with another tool such as FTP. Files can also be copied over on Mac and Linux with native tools like scp, rsync, and sftp.

We going to setup rsync:
Config file is located at `/etc/rsynd.conf`
Check the path and drop a webshell on that path
So we have a thing that sync is distributed accross the machine on the network so `/app` can be accessible on thourghout the network