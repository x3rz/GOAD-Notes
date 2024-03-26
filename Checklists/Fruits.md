## First things to check
- [ ] CSRF on each endpoint
- [ ] XSS on each endpoint
- [ ] File Upload Bypasses
- [ ] Rate Limit Bypasses 
- [ ] Command injection
- [ ] [[SSRF]]
- [ ] PATH Traversal


-----
- [ ] Check for **[[Improper Restriction of Rendered UI Layers or Frames]]** (zero bounty)
- [ ] Check for **[[Use of a Broken or Risky Cryptographic Algorithm]]**
- [ ] Try Credential stuffing (Finding breached credentials) [[MethodCred]]
- [ ] Check for `HttpOnly` Flag in the webapp [[HttpOnly]] and Check in Developer option if `Secure` is checked or not.
- [ ] Check if the UI performs wrong Action. [[WrongAction]]
- [ ] Use of Predictable Algorithm in Random Number Generator
	- In PHP [check](https://www.ambionics.io/blog/php-mt-rand-prediction) for `mt_rand()` , [[Reports for this]]
	- Also for `rand()` function. [Report](https://www.huntr.dev/bounties/1624974055573-w7corp/easywechat/)
	- Use of these `microtime : could be bruteforced`, `mt_rand()` and even `rand`
	- **Mitigation:** random_bytes() use this one

- [ ] Check for ***Use of Cache Containing Sensitive Information***
- [ ] Check for [[session fixation]] by changing the password and see if other session got logged out or not.
- [ ] [[Insufficient Granularity of Access Control]]
- [ ] Check for [[Exposure of Sensitive Information to an Unauthorized Actor]]
- [ ] Dorking

- [ ] Check if `X-Frame-Option` header is aviable in response else its possible to do clickjacking attack [report](https://www.huntr.dev/bounties/872e57e6-f7ab-4f9b-aa0c-209c8a3f499e/)
- [ ] Access to xmlrpc.php? dos || bruteforce attack
- [ ] Broken link hijacking [[BLtools]]
- [ ] [[Find AEM in webapp]]
- [ ] Previous fixed XSS? try [mutation XSS](https://www.acunetix.com/blog/web-security-zone/mutation-xss-in-google-search/)
- [ ] Check for [[CVEs]]
- [ ] **[[Wordpress Plugin]]**


- [ ] Reliance on Cookies without Validation and Integrity Checking
	- [[Reports for this too]]

- [ ] SQLi + XSS + SSTI
	- `'''><svg/onload=prompt(5);>{{7*7}}`
	- ``"><iMg SrC="x" oNeRRor="alert(1);">``
	- Django applications [[one.pdf]] [two](https://blog.cobalt.io/a-pentesters-guide-to-server-side-template-injection-ssti-c5e3998eae68)

- [ ] Check for SSTI
	- `{{2*2}}[[3*3]]`
- [ ] **EXIF Geo data not stripped**
	1.  Got to Github ( [https://github.com/ianare/exif-samples/tree/master/jpg](https://github.com/ianare/exif-samples/tree/master/jpg))  
	1.  There are lot of images having resolutions (i.e 1280 \* 720 ) , and also whith different MB’s .  
	1.  Go to Upload option on the website  
	1.  Upload the image
	1.  see the path of uploaded image ( Either by right click on image then copy image address OR right click, inspect the image, the URL will come in the inspect , edit it as html )  
	1.  open it ([http://exif.regex.info/exif.cgi](http://exif.regex.info/exif.cgi))  
    
	7.  See wheather is that still showing exif data , if it is then Report it.


- [ ] [[Email Verification Bypass]]
- [ ] SPF, DMARK check
- [ ] CSP 
	- [ ] https://csp-evaluator.withgoogle.com/
	- [ ] Lead to XSS

- [ ] Weak Password Policy
- [ ] Bruteforce user credentials restriction
- [ ] Open S3 buckets
	- https://buckets.grayhatwarfare.com/ [Blog](https://mikey96.medium.com/cloud-based-storage-misconfigurations-critical-bounties-361647f78a29)

- [ ] Search for API keys, Hard-coded credentials, encryption keys,etc.
	- [ ] [[Regex to find them]]
- [ ] Search for sensitive files 
	- [ ] have docker-compose.yml ?

- [ ] Json request/response
	- [ ] Try [[Prototype Pollution]]

- [ ] [[Host Header Injection]]
- [ ] RCE via referer header