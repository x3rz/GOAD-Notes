## What is SOP 
- Web security is based on SOP.
- It is implemented by Browser by default and **server** have to implement CORS or `Access-Control-Allow-Origin`



>This is the default policy of the web and prevents the user's data from being leaked when logged into multiple sites at the same time.


### Implementations
### Working
### Role of CORS 
**What is CORS**



## What is CSP
- Primary goal of CSP is to mitigate XSS.

In Short **CSP** is more modified and narrowed apporach than **SOP**. If browser doesnt support CSP then it switeches to SOP.

**Mitigating XSS**
Primary goal of CSP is to mitigate and report XSS.
It makes it possible for server administrator to reduce or eleminate the vectors by which XSS can occur by specifyuing *domains that the browser should consider to be valid sources of executable scripts*

**Mitigating Packet Sniffing**
Content must be loaded from HTTPS only websotes 

## Difference between CORS (SOP) and CSP
[Link](https://www.appsecmonkey.com/blog/same-origin-policy)
[Weakness Exploitation](https://infosecwriteups.com/cors-and-its-misconfigurations-1654ee04d140)

> CORS allows the Same Origin Policy to be relaxed for a domain.

ie. If user logs into both `example.com` and `example.org` the SOP prevents `.com` for making request to `example.org/current_user/full_user_details` and gaining access to resopnse.

#todo 
The header used to do that is:


>This is the default policy of the web and prevents the user's data from being leaked when logged into multiple sites at the same time.

With the Help of CORS `example.org` cpi;d set a policy to say it will allow the origin `https://example.com` to read responses made by AJAX. This would be done if both `.com` and `.org` are ran by the same company and data sharing is allowed in the user's browser. It only affects the cloent-side of things, not the server-side.

**CSP** What content can run on the current site. Ex: of JS can be executed inline or which domains `.js` file can be loaded from. This can be benificial to act as another line of defense against XSS attacks.