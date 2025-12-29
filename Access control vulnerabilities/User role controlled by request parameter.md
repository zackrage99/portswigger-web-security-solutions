**Vulnerability:** Access control vulnerability

**Vulnerable Parameter:** Admin panel

**Impact:** Delete users

**URL:** https://portswigger.net/web-security/access-control/lab-user-role-controlled-by-request-parameter
## Steps:
1. Open your lab
2. Log in with given credntials wiener:peter
3. Go to /admin page notice Admin interface only available if logged in as an administrator 
4. Open burp suite and send the request of /admin to repeater
5. Observe the request contain a parameter `Admin=false` we try to change it to true and send it 
6. Now notice the response 200 ok You are now in the admin panel search in the responce for carlos you get the delete link
7. change the GET /admin to /admin/delete?username=carlos and send the request
8. Lab solved








