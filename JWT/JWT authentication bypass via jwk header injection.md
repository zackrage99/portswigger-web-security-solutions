**Vulnerability:** The server fails to check whether the provided key came from a trusted source

**Vulnerable Parameter:** jwk key  
**Impact:** Authentication bypass → Admin access  

**Steps:**
1. Login as `wiener:peter`
2. Capture GET `/my-account` in Burp → Send to Repeater
3. Change to GET `/admin` → 401 error
4. In JWT Editor tab, modify payload: `sub: "administrator"` → Send → Still 401 (signature invalid)
5. Notice the `alg:RS256` used is asymmetric meaning it has 2 keys  private and public
6. From the lab we know this lab trust any key embeded

**Attack:**

7. Click "jwt editor extension" → new RSA key and generate
8. Back to repeater attack then embedded jwk and ok
9. Send → 200 OK (admin access)
10. Find delete link for `carlos` in response
11. Send GET `/admin/delete?username=carlos`
12. Lab solved

**Key Learning:**  
- Servers usually use a white list for the trusted public key but this lab trust any  generated public key in the JWT header
