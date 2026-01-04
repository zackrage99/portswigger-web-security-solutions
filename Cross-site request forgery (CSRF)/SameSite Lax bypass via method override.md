**Quick Brief:**
- CSRF exploits the browser's automatic credential sending mechanism combined with the application's trust in authenticated requests without verification of user intent
- The vulnerability exists because web applications cannot distinguish between:
      1. A legitimate request initiated by the user through the application's UI
      2. A forged request initiated by an attacker from another site

  
-The "2-Minute Grace Period" Rule in Chrome What Happens:

    For the first 2 minutes after a cookie without SameSite is set:

        The cookie is treated as SameSite=Lax for POST requests

        But it's treated as SameSite=None for non-POST requests (like GET, HEAD, OPTIONS, TRACE) during this short window

    After 2 minutes:

        The cookie fully defaults to SameSite=Lax for ALL request types

-Bypassing "Lax" by Switching to GET

By default, modern browsers treat cookies as SameSite=Lax. Under this rule:

    POST requests from an external site do not include the session cookie.

    GET requests from an external site do include the session cookie, provided it is a "top-level navigation" (like a link or a redirect).


**What we're testing:** The `/my-account/change-email` endpoint for CSRF protection
**Why it might be vulnerable:** No CSRF token, SameSite cookies, or other anti-CSRF measures
**What CSRF exploits:** Browser automatically sends session cookies with requests


**URL:** https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions/lab-samesite-lax-bypass-via-method-override
## Steps:
1. Open your lab and log in with given credentials wiener:peter
2. Open Burp Suite we know from the lab describtion the endpoint of the email is vulnerable to csrf
3. Change your email and send the request to repeater
4. Observe from the request there is no csrf key or csrf cookie 
5. Observe from the cookie editor extension that there is no same-site attribute is set meaning the rule of 2-minute grace period will set
6. To be able to solve the lab we need to do something called method overriding
7. Now we will prepare our malicious code

12.
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <iframe style="display: none;" id="csrf-iframe"></iframe>
    <form id="csrf-form" action="https://0a7c005b031680e298001a9c00b900c4.web-security-academy.net/my-account/change-email" method="GET" target="csrf-iframe">
        <input type="hidden" name="email" value="test.test@ca">
        <input type="hidden" name="_method" value="POST">
    </form>
    <script>document.getElementById("csrf-form").submit()</script>
</body>
</html>
```


13. Copy the code and change the lab link to your lab link and paste it in the body of the exploit server and then store the exploit then Deliver exploit to victim
14. Lab solved

## Notes:
- This code is like a normal html page that change the users email automatically without even them knowing by just making the user click on the link
- The  `<form> </form>` is to create a form in html :
`action` here we put our lab link id and the functionality `/my-account/change-email` you want to excute
`method:get` we know from the request that it doesnot validate tokens on get 

- The `<input>` is to create an input for the email in the html form:
`type:hidden` so the no input field appear to the user
`name:email` we got it from the rquest
`value` is the new email you want to change to

- The `<iframe></iframe>` is a sand box so the form is excuted inside it :
`style="display: none;` so nothing appear to the user

The `<img` is a link to an image `display:none` so when error accure nothig appear to the user:
It starts with %0d%0a which are URL-encoded carriage return and line feed (\r\n)
%3b = ; (semicolon)
%20 = (space)
So %3b%20 becomes ; (semicolon followed by a space).

-If a lab's vulnerable action (like changing an email) normally uses POST, a CSRF attack would fail because the browser won't send the cookie. 
However, many back-end frameworks are "method-agnostic"—they will process the action even if you send the data via GET instead. 
By overriding the method to GET, you force the browser to include the cookie, bypassing the Lax restriction.

Exaplanation:
Browser's Automatic Behavior:

   1.IMG tag loads → Browser makes request to search URL

   2. Server responds with Set-Cookie header

   3.Browser stores cookie (no user interaction needed)

   4.Form submits → Browser sends stored cookie automatically



## How to Fix (For Developers)
1. **CSRF Tokens**: Validate tokens on both request methods or allow only post requests
2. **SameSite Cookies**: `SameSite=Strict` for sensitive actions
3. **Custom Headers**: Require `X-Requested-With: XMLHttpRequest`
4. **Double Submit**: Token in both cookie and request



