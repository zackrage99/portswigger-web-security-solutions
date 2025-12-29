**Vulnerability:** Access control vulnerability

**Vulnerable Parameter:** password disclosure

**Impact:** Delete users

**URL:** https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-password-disclosure
## Steps:
1. Open your lab
2. Log in with given credentials wiener:peter
3. Open burp suite and send the request to repeater
4. Notice you got your passsword prefilled in a masked input You can reveal it  using inspect element and change it's value
5. Notice the GET /my-account?id=wiener we try to change it to `GET /my-account?id=administrator` and you got 200 ok
6. search in the response you got the password of the administrator account Log in with the credentials and delete the user carlos
7. Lab solved
