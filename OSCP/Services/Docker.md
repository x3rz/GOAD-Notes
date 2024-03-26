### Docker Escape


https://medium.com/better-programming/escaping-docker-privileged-containers-a7ae7d17f5a1


### Docker escalation

Docker was introduced as a competitor to the virtual machines that we have been using. The inherent problem with Docker is that every docker command needs to be run with sudo i.e. in a privileged mode. This leads to the issue that it gives the user methods to enhance his/her privilege if they have access to the daemon. So anyone who is a part of the docker group, in turn has access to everything that a root user has access to.

```bash
docker run -v /root:/hack -t debian:jessie /bin/sh -c 'cat /root/root.txt'
```

The command above allowed the user to run a command as a privileged user even though the user don’t have sudo right.


