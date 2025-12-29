**Vulnerability:** Access control vulnerability

**Vulnerable Parameter:** privilege escalation 

**Impact:** Optain users accounts

**URL:** https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-unpredictable-user-ids
## Steps:
1. Open your lab
2. Log in with the given credentials wiener:peter
3. open burp suite and send the request to repeater
4. Observe the request GET /my-account?id=246351cd-9c0d-4332-9d38-1aa8d4bb773e the lab  identifies users with GUIDs
5. We need to optain the GUID for the user carlos because we cannot change the user name 
6. We go to home page search for a post written by carlos now click on his name and send it to repeater
7. Notice you get carlos GUID GET /blogs?userId=c8783c74-941b-4389-b2f9-1427789ea194
8. Change you GUID with carlos GUID GET /my-account?id=c8783c74-941b-4389-b2f9-1427789ea194 
9. Observe the response containig carlos api key
7. Lab solved









