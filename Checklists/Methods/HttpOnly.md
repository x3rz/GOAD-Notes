Secure
----

**Description:**
```
The purpose of the secure flag is to prevent cookies from being observed by unauthorized parties due to the transmission of a the cookie in clear text. To accomplish this goal, browsers which support the secure flag will only send cookies with the secure flag when the request is going to a HTTPS page. Said in another way, the browser will not send a cookie with the secure flag set over an unencrypted HTTP request. By setting the secure flag, the browser will prevent the transmission of a cookie over an unencrypted channel.
```

**Impact:**
```
If the secure flag is set on a cookie, then browsers will not submit the cookie in any requests that use an unencrypted HTTP connection, thereby preventing the cookie from being trivially intercepted by an attacker monitoring network traffic. If the secure flag is not set, then the cookie will be transmitted in clear-text if the user visits any HTTP URLs within the cookie's scope. An attacker may be able to induce this event by feeding a user suitable links, either directly or via another web site.
```



-----

HttpOnly
----

# Description

The software uses a cookie to store sensitive information, but the cookie is not marked with the HttpOnly flag. The HttpOnly flag directs compatible browsers to prevent client-side script from accessing cookies. Including the HttpOnly flag in the Set-Cookie HTTP response header helps mitigate the risk associated with Cross-Site Scripting (XSS) where an attacker's script code might attempt to read the contents of a cookie and exfiltrate information obtained. When set, browsers that support the flag will not reveal the contents of the cookie to a third party via client-side script executed via XSS.  
`https://github.com/YesWiki/yeswiki/` is vulnerable to Sensitive Cookie Without HttpOnly Flag as shown below:

# Proof of concept

Snippet:

```
    public function logIn($remember = 0)
    {
        $_SESSION['user'] = array(
            'name'              => $this->properties['name'],
            'password'          => $this->properties['password'],
            'email'             => $this->properties['email'],
            'motto'             => $this->properties['motto'],
            'revisioncount'     => $this->properties['revisioncount'],
            'changescount'      => $this->properties['changescount'],
            'doubleclickedit'   => $this->properties['doubleclickedit'],
            'show_comments'     => $this->properties['show_comments'],
        );
        $this->wiki->setPersistentCookie('name', $this->properties['name'], $remember);
        $this->wiki->setPersistentCookie('password', $this->properties['password'], $remember);
        $this->wiki->setPersistentCookie('remember', $remember, $remember);
    }
```

The login function doesnt set the HTTPOnly flag on valid session, to show that do the following:

## Payload

Login to the site  
In firefox Press `Ctrl+Shift+I (Cmd+Option+I on macOS)` to open Developer Tools.  
Click the heading of the 'Storage' tab.  
On the left side of the panel, make sure to select the desired site under 'Cookies.'  
Observe the HTTP Only property is set to false.  
`CookieName:'COOKIEVAL',Domain:'localhost',Path:'PATH',HttpOnly:false`

# Impact

If the cookie in question is an authentication cookie, then not setting the HttpOnly flag may allow an adversary to steal authentication data (e.g., a session ID) and assume the identity of the user.