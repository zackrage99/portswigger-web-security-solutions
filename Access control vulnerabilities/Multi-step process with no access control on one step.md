**Vulnerability:** Access control vulnerability

**Vulnerable Parameter:** Multi-step process

**Impact:** Getting admin privileges

**URL:** https://portswigger.net/web-security/access-control/lab-multi-step-process-with-no-access-control-on-one-step
## Steps:
1. Open your lab
2. Open burp suite and Login with administrator credentials given by the lab
3. Go to admin panel and upgrade the user carlos and send the request to repeater
4. Notice you get another action telling you to confirm the upgrade send this to reeapter also
4. Now log in with non-admin credentials wiener:peter now refresh the page and send the request to repeater
5. Copy your non-admin credentials session cookie and paste it with admin upgrade confirmation request (the second action)
6. Notice you did not get an error change the username from carlos to your username wiener
7. Lab solved








