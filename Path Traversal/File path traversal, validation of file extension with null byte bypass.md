**Vulnerability:** path traversal vulnerability

**Vulnerable Parameter:** image (GET request)

**Impact:** List /etc/passwd contents

**Technique:** Directory Traversal Sequences with null byte

**URL:** https://portswigger.net/web-security/file-path-traversal/lab-validate-file-extension-null-byte-bypass
## Steps:
1. Find a vulnrebal parameter
2. Look for user-controlled input 
3. We find image parameter
4. Send request to burp suite
5. GET /image?filename=53.jpg 
6. From the lab we know that the application validates that the supplied filename ends with the expected file extension.
7. GET /image?filename=../../../etc/passwd.jpg → Error
8. GET /image?filename=../../../etc/passwd%0053.jpg 
9. Observe the response containing the /etc/passwd contents
10. Lab solved


## Notes:
- First thing you should look for is user-controlled input:
1. URL parameters : /download?file=report.pdf
2. POST parameters : filename=user1.txt

- At first time i did alot of payloads and didn't worked so i googled the lab and found we need to use the null byte
-  %00 is the URL-encoded representation of a null byte (ASCII character 0) 
- In many programming languages (C, PHP, etc.), null bytes mark the end of a string
- Meaning when we send it with null byte the server will terminate the rest of it and excute our payload system reads ../../../etc/passwd






