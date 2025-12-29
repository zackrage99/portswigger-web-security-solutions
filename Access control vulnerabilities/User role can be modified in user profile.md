**Vulnerability:** Access control vulnerability

**Vulnerable Parameter:** Admin panel

**Impact:** Delete users

**URL:** https://portswigger.net/web-security/access-control/lab-user-role-can-be-modified-in-user-profile
## Steps:
1. Open your lab
2. From the lab description we know we need to change our role to 2
3. Go to /admin page examin the response nothing interesting
4. Change the email address send the request to burp observe the responce containing "roleid": 1
5. Now we need to change our roleid to 2 in our request body add `{"roleid":2}` and send
6. Observe you got 200 ok now go to your account refresh the page notice you now get a new choice `admin panel` now delete the user carlos
7. Lab solved

## Notes:
- Content-Type: application/json this shown in the response meaning it is a json format
` We did  curly braces {} because it is the valid json format








