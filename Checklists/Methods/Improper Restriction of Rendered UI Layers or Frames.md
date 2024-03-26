Possible attack: Clickjacking

Impact: 
```
it is possible for a page controlled by an attacker to load the website within an iframe. This will enable a clickjacking attack, in which the attacker's page overlays the target application's interface with a different interface provided by the attacker

```

Remedy:
```
it can be possible to perform a clickjacking attack due to the lack of frame restrictions. The application does not set the response header X-Frame-Options: DENY.
```

**How to hunt:**

1. Create a html page with iframe tag including website 

Example:
```html
<html>
    <head>
        <title>Clickjack test page</title>
    </head>
    <body>
        <iframe src="https://Vulnerable-website.com" width="500" height="500"></iframe>
    </body>
</html>


```


2. Save the script as clickjacking .html and page will render in iframes
3. 