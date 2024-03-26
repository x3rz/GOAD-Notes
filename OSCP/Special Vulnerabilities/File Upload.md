**Windows Server(IIS):**

*web.config* file upload is similar to the *.htaccess* file upload where these files overwrites the rules of server.
https://soroush.secproject.com/blog/tag/unrestricted-file-upload/
https://poc-server.com/blog/2018/05/22/rce-by-uploading-a-web-config/

**Linux Server(Apache):**
*htaccess* file upload attack creted in Wormcon 0x01


#### Check which extensions are available

*extension.txt*
```txt
png  
jpg  
php  
php5  
php7  
phtml  
txt  
html  
asp  
aspx  
exe  
config  
js
```

Load this file in python script and run it.

*check.py*
```py
#!/usr/bin/python3import requests  
import sys  
import re  
from bs4 import BeautifulSoup  
url = "http://10.10.10.93/transfer.aspx"  
filename = "extension.txt"def upload(f):  
    s = requests.Session()  
    r = s.get(url)  
    #if r.status_code == 200:  
    #    print("[INFO] Checking...{0}".format(f))  
    #else:  
    #    print("[ERROR] Can't connect...")  
    #    sys.exit(1) p = BeautifulSoup(r.content, "html.parser") viewState = p.find(attrs = {'name' : '__VIEWSTATE'})['value']  
    eventValidation = p.find(attrs = {'name' : '__EVENTVALIDATION'})['value'] postData = {  
            '__VIEWSTATE' : viewState,  
            '__EVENTVALIDATION' : eventValidation,  
            'btnUpload' : 'Upload'  
            } uploadedFile = {'FileUpload1' : (f, 'test')} r = s.post(url, files=uploadedFile, data=postData)  
    return r.textprint("[INFO] Allowed Extensions:")for i in open(filename, 'r'):  
    #print(i[:-1])  
    response = upload('bigb0ss.' + i[:-1])  
    if "successfully" in response:  
        print("[+] %s" % i.strip())
```


## File upload in exifdata of the image

see Magic HTB, Networked HTB

Basically use exiftool and embed in comments and then rename the extrension to .png to .php.png