**Vulnerability:** path traversal vulnerability

**Vulnerable Parameter:** image (GET request)

**Impact:** List /etc/passwd contents

**Technique:** Directory Traversal Sequences

**URL:** https://portswigger.net/web-security/file-path-traversal/lab-simple
## Steps:
1. Find a vulnrebal parameter
2. Look for user-controlled input 
3. We find image parameter
4. Send request to burp suite
5. GET /image?filename=21.jpg
6. Test with ../../../etc/passwd
7. GET /image?filename=../../../etc/passwd
8. Observe the response containing the /etc/passwd contents
9. Lab solved


## Notes:
- First thing you should look for is user-controlled input:
1. URL parameters : /download?file=report.pdf
2. POST parameters : filename=user1.txt





