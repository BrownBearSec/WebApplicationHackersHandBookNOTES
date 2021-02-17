# Attacking Authentication

- Should be simple in theory however is often done wrong and can be abused in many ways.

# Authenticaation Technologies

- HTML forms-based authentication
- Multifactor mechanism
- Client SSL certs
- HTTP basic and digest auth
- Windows-integrated auth using NTLM or Kerberos
- Auth services
- 90% of authentication is done via HTML forms

# Design Flaws in Authentication Mechanisms

Bad passwords
- common to encounter applications that allow passwords to be:
- -> very short **or** blank
- -> Common dictionary words or names
- -> The same as username
- -> Still set to a default value

Brute-Forcible login
- You can be rate limited
- rate limiting can be bypassed if it is poorly implemented, eg a cookie counting number of times you have attempted login can be deleted
- even if you are rate limited, it may still give you a different message if you got it correctly

Verbose failure messages 
- If the error message is specific to which piece of info was wrong, you can enumerate
- even with generic error messages, usernames can sometimes be enumerated with how long the request took, eg if it was found it may take longer

Vulnerable Transmission of Credentials
- HTTP eavs droppers may reside in:
- -> on the user's local network
- -> within the user's IT deparment
- -> within the user's ISP
- -> on the internet backbone
- -> Within the ISP hosting application
- -> within the IT deperatment managing the application
- Even with HTTPS credentials may not be safe:
- -> as a query string may be logged in browsing history, web server logs, proxies/vpns
- -> even if they use a POST form it may be passed to a 302 redirect URL without user knoledge
- -> bad apps may store creds in the cookie 

Password change functionality
- periodic enforced password changes can mitigate vulnerabilities
- verbose error messages indicating if the user is valid 
- often forget to rate limit existing password field
- often runs into error due to complex action 

Forgotten password functionality
- opens up the possiblity for account takeover
- security questions are not secure
- if the app has a "hint" functionality, users will often just write their password or something very obvious
- one time passwords or challenges can be vulnerable 
- 