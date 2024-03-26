Blog: https://blog.projectdiscovery.io/nuclei-v2-4-0-release/

Template collection by EMAD:
https://github.com/emadshanab/Nuclei-Templates-Collection

**Cent:** tools that fetches alll the community avaiable templates
https://github.com/xm1k3/cent
Basic template command:
```
cat subdomain.txt | httpx | nuclei -t ~/nuclei-template/
```

One liner:
```
cat domains.txt | httpx -silent | xargs -n 1 gospider -o output -s ; cat output/* | egrep -o 'https?://[^ ]+' | nuclei -t ~/nuclei-templates/ -o result.txt

```

subdomains > params | endpoints > nuclei

## RCE via httpx
![[Pasted image 20210719162830.png]]