NFS stands for "Network File System" and allows a system to share directories and files with others over a network. By using NFS, users and programs can access files on remote systems almost as if they were local files. It does this by mounting all, or a portion of a file system on a server. The portion of the file system that is mounted can be accessed by clients with whatever privileges are assigned to each file.

## Enumeration
`showmount -e [IP]`
## Exploitation
`mount -t nfs IP:/share /tmp/mnt/ -nolock`
```bash
# nmap scan
nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.183.1
```

## Mounting

Mount with command
```bash
sudo mount -t nfs IP:/share /tmp/mnt/ -nolock
```

Mount with `/etc/fstab`
```bash
# Add the following line in /etc/fstab
# <file system>     <dir>       <type>   <options>   <dump>	<pass>
IP:/share /tmp/mnt  nfs      defaults    0       0
```

After adding run the following command:
```bash
mount /tmp/mnt
mount IP:/share
```


## Post Exploitation
Hunt: check for `cat /etc/exports`

Files created vua NFS inherits the remote user's ID, If the user is root, and root squashing is enabled, the ID will instead be set to 'nobody' user.

Check for NFS share configuration on the debian VM

`cat /etc/exports` | `showmount -e <IP>` | `nmap -sV -script=nfs-showmount <IP>` 
**Mount an NFS share**
`mount -o rw,vers=2 <target>:<share> <local_directory>`