**Vulnerability:** Access control vulnerability

**Vulnerable Parameter:** Admin panel

**Impact:** Optain sensetive information about users

**URL:** https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter
## Steps:
1. Open your lab
2. Log in with the given credentials wiener:peter
3. open burp suite and send the request to repeater
4. Observe the response containing your api key
5. GET /my-account?id=wiener we try to change the username to carlos `GET /my-account?id=carlos` 
6. Observe the response contains carlos api key copy it and paste it in the lab solution section
7. Lab solved









