1.  Obtain the latest database of vulnerabilities by executing the command `wes.py --update`.
2.  Use Windows' built-in `systeminfo.exe` tool to obtain the system information of the local system, or from a remote system using `systeminfo.exe /S MyRemoteHost`, and redirect this to a file: `systeminfo > systeminfo.txt`
3.  Execute WES-NG with the systeminfo.txt output file as the parameter: `wes.py systeminfo.txt`. WES-NG then uses the database to determine which patches are applicable to the system and to which vulnerabilities are currently exposed, including exploits if available.
4.  As the data provided by Microsoft's MSRC feed is frequently incomplete and false positives are reported by `wes.py`, @DominicBreuker contributed the `--muc-lookup` parameter to validate identified missing patches against Microsoft's Update Catalog. Additionally, make sure to check the [Eliminating false positives](https://github.com/bitsadmin/wesng/wiki/Eliminating-false-positives) page at the Wiki on how to interpret the results. For an overview of all available parameters, check [CMDLINE.md](https://github.com/bitsadmin/wesng/blob/master/CMDLINE.md).

***For better understanding watch*** https://youtu.be/VDf99UpGmSY


[Windows Exploit Suggester](https://github.com/bitsadmin/wesng)

Save the ouput of `systeminfo` into the file.txt

In Kali:

```bash
wes.py /home/file.txt -i 'Elevation of Privilege' --exploits-only | more
```

Pre-Compiled Kernel Exploit: [https://github.com/SecWiki/windows-kernel-exploits](https://github.com/SecWiki/windows-kernel-exploits)

Quiet Efficient:

```bash
suggestme
```