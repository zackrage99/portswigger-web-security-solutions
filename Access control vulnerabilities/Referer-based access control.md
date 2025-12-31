**Vulnerability:** Access control vulnerability

**Vulnerable Parameter:** referre header

**Impact:** Getting admin privileges

**URL:** https://portswigger.net/web-security/access-control/lab-referer-based-access-control
1. Open your lab
2. Open burp suite and Login with administrator credentials given by the lab
3. Go to admin panel and upgrade the user carlos and send the request to repeater
4. Now log in with non-admin credentials wiener:peter now refresh the page and send the request to repeater
5. Notice when delete the Referer from the request of admin upgrade it gives you an error  `HTTP/2 401 Unauthorized` this is a good sign
5. Now copy non-admin credntials session cookie and paste it in the admin cookie request while keeping the Referer:
6. Notice you did not get an error change the username from carlos to your username wiener
7. Lab solved

## Notes:
- Referer : is an HTTP request header that indicates the URL of the web page that linked to the resource being requested
- Bad implementations of referer or misconfigurations can lead to serious exploits





