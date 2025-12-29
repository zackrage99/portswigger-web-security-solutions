**Vulnerability:** Access control vulnerability

**Vulnerable Parameter:** Admin panel

**Impact:** Delete users

**URL:** https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-data-leakage-in-redirect
## Steps:
1. Open your lab
2. Log in with given credentials wiener:peter
3. Open burp suite and send the request to repeater
4. Notice the request GET /my-account?id=wiener We try to change the username to carlos  `GET /my-account?id=carlos`
5. Observe the responce containing carlos api key 
6. Lab solved
