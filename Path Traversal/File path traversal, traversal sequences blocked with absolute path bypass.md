**Vulnerability:** path traversal vulnerability

**Vulnerable Parameter:** image (GET request)

**Impact:** List /etc/passwd contents

**Technique:** Directory Traversal Sequences

**URL:** https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass
## Steps:
1. Find a vulnrebal parameter
2. Look for user-controlled input 
3. We find image parameter
4. Send request to burp suite
5. GET /image?filename=53.jpg 
6. Test with ../../../etc/passwd → Error (No such file)
7. Test without Traversal Sequence → (Absolute Path)
8.GET /image?filename=/etc/passwd
9. Observe the response containing the /etc/passwd contents
10. Lab solved


## Notes:
- First thing you should look for is user-controlled input:
1. URL parameters : /download?file=report.pdf
2. POST parameters : filename=user1.txt

- If the first case of using Traversal Sequences didn't work we should try another methods
which will be shown in the next few labs






