**Vulnerability:** Access control vulnerability

**Vulnerable Parameter:** Any restericted resources on a webapp

**Impact:** Expose sensetive information

**URL:** https://portswigger.net/web-security/access-control/lab-url-based-access-control-can-be-circumvented

## Notes:  X-Original-URL HTTP header is used by reverse proxies  to preserve the original requested URL before internal rewriting or redirection. 
It helps backend applications determine the original request when URL rewriting is in use
If a website is misconfigured an attacker can override the original request and access provate directories

## Steps:
1. Open your lab
2. Now go to admin panel notice you get access denied
3. Open burp suite and send the request to repeater
4. To test for X-Original-URL header all you need to do is to add it in burp request
5. Add the header X-Original-URL header: /example and delete the /admin make it / and send Notice you ghet `"Not Found"`
6. This is a good sign meaning the website rely on only  X-Original-URL to validate requests
7. Now replace the  X-Original-URL with /admin so the request is overrriden `X-Original-URL: /admin` 
8. Notice in the responce you get the admin panel now search for carlos and you get a delete link replace it in X-Original-URL
9. Lab solved








