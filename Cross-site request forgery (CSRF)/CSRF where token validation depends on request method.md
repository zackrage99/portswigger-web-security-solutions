**Quick Brief:**
- CSRF exploits the browser's automatic credential sending mechanism combined with the application's trust in authenticated requests without verification of user intent
- The vulnerability exists because web applications cannot distinguish between:
      1. A legitimate request initiated by the user through the application's UI
      2. A forged request initiated by an attacker from another site


**What we're testing:** The `/my-account/change-email` endpoint for CSRF protection
**Why it might be vulnerable:** No CSRF token, SameSite cookies, or other anti-CSRF measures
**What CSRF exploits:** Browser automatically sends session cookies with requests


**URL:** https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-validation-depends-on-request-method
## Steps:
1. Open your lab and log in with given credentials wiener:peter
2. Open Burp Suite we know from the lab describtion the endpoint of the email is vulnerable to csrf
3. Change your email and send the request to repeater
4. Observe from the request there is CSRF token at first i tried to add a number to the csrf token to see if the server validates it and error accured 
5. Now i tried to delete the whole token and also an error accured
6. Now we try to change the request method in burp suite to GET now delte the csrf token notice the response 200 ok 
7. Now we know with GET request the token is not validated from the server side only it validate the token with POST
8. Now we will prepare our malicious code

9.
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
    <form action="https://0a1700c6030c875d802fcb8700d30066.web-security-academy.net/my-account/change-email" method="get" target="csrf-iframe" id="csrf-form">
        <input type="hidden" name="email" value="test.tes@ca">
    </form>
    <script>document.getElementById("csrf-form").submit()</script>
</body>
</html>
```


10. Copy the code and change the lab link to your lab link and paste it in the body of the exploit server and then store the exploit then Deliver exploit to victim
11. Lab solved

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

- `<script>document.getElementById("csrf-form").submit()</script>` here we grap the form by it's id and automatically submits it without the user interaction


## How to Fix (For Developers)
1. **CSRF Tokens**: Validate tokens on both request methods or allow only post requests
2. **SameSite Cookies**: `SameSite=Strict` for sensitive actions
3. **Custom Headers**: Require `X-Requested-With: XMLHttpRequest`
4. **Double Submit**: Token in both cookie and request



