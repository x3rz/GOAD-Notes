**Description:**
Sensitive data on the application can be exposed after the user logout

**Proof of Concept**
1. Login to the application (demo.livehelperchat.com/site_admin/)
2. Go to page like My Account , or Any other page
3. Click logout
4. Click browser back button

**Impact**
```
When a user logs out without closing the browser someone can view the information inside by clicking the back button on the browser.
```

**About the vulnerability:**
```
The back, forward and refresh buttons of the browser can be used to steal the password of a previous user. In this article we examine the vulnerability and look at ways to solve them.A web browser has the functionality to store the recent pages browsed by the user in its history. The back and forward buttons on the browser make use of this history to display the pages that the user visited recently. The browser also keeps track of the variables that were sent as part of the request to the server for each page. The refresh feature of the browser automates posting of the variables to the server thereby greatly improving the user experience while browsing.These features enhance the user experience but at the same time they expose a high risk vulnerability. This happens due to the application being insecurely designed. Attackers exploit these functionalities of the browser to obtain access to user credentials. Let’s see how this works and the solutions to overcome this problem.
```

**Solution :**
```
Use an intermediate page between the login page and the first page displayed after authentication.This intermediate page should be used to redirect the user via an “HTTP Redirect command” to  after successful login. In such a scenario, the login request is redirected immediately by the intermediate page. 2, use a salted hash technique for authentication. In this method, the password is hashed before sending it to the server. This hash is made random using a salt (a random value) provided by the server. This salt is added to the hash generated from the password and then hashed again. This salted hash is sent to the server for authentication. This way, even if the attacker uses the refresh button to capture the request, only the salted hash value will be visible. It will not allow the attacker to login by refreshing as the salt would change at the next login.
```

**Refrences:**
-   https://huntr.dev/bounties/2b200b7d-8896-4603-a231-305912a1a2e8/