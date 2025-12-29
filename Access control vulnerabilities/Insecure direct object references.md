**Vulnerability:** Access control vulnerability

**Vulnerable Parameter:** Chat Logs

**Impact:** Optain sensetive information about users

**URL:** https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references
## Steps:
1. Open your lab
2. From the lab description we know lab stores user chat logs directly on the server's file system, and retrieves them using static URLs
3. We go to live chat Observe the view transcript when clicked it downloads a copy of the conversation 
4. Open burp suite and download the conversation again 
5. Observe the request GET /download-transcript/2.txt HTTP/2 notice the number 2 what if we changed it to 1 `GET /download-transcript/1.txt HTTP/2`
6. Notice in the response you get a conversation with a password inside it now log in with the credentials carlos and the password you found
7. Lab solved








