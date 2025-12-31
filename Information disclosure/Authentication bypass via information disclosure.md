**Vulnerability:** Information disclosure vulnerability

**Vulnerable Parameter:** Authentication bypass

**Impact:** Log in to administrator account

**URL:** https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-authentication-bypass
## Steps:
1. Open your lab
2. Log in with given credentials wiener:peter
3. Open burp suite
3. Try open the admin panel /admin and send the request to repeater Notice an interesting  message Admin interface only available to local users  
4. This means it is validating admin account on some kind of local ip addresses
6. Try to change the method to `POST` and `OPTIONS` and `TRACE` notice wtih `TRACE` an intersting message `X-Custom-IP-Authorization: YOUR ip here` 
7. This is the custom http header used to validate local admin note it contain your ip address whcih is not local maybe we can try to make it local and see
8. X-Custom-IP-Authorization: 127.0.0.1 add this to your request to /admin and change the request methid to `GET` and send notice you get the admin panel opened
9. Search for carlos in the response and you will get the delete link copy and send it while keeping the `X-Custom-IP-Authorization: 127.0.0.1`
8. Lab solved

## Notes:
- All http methods can be found on mozilla website








