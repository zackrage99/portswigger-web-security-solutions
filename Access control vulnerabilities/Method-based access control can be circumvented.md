**Vulnerability:** Access control vulnerability

**Vulnerable Parameter:** request method

**Impact:** Getting admin privileges

**URL:** https://portswigger.net/web-security/access-control/lab-method-based-access-control-can-be-circumvented
## Steps:
1. Open your lab
2. Open burp suite and Login with administrator credentials given by the lab
3. Go to admin panel and upgrade the user carlos and send the request to repeater
4. Now log in with non-admin credentials wiener:peter now refresh the page and send the request to repeater
5. Copy your non-admin credentials session cookie and paste it with admin upgrade request and send Notice you get an error
6. Right click on the request and change request method and send 
7. Notice you did not get an error change the username from carlos to your username wiener
8. Lab solved


## Notes:
- The core issue of this vulnerability is it checks only certain htpp-methods

 Vulnerable logic example:
if (request.method === "POST") {
    checkAuthorization(); // Only checks for POST requests
}
performAdminAction(); // Executes for ANY method!






