**Vulnerability:** Server accepts unsigned JWTs via `alg: "none"`  
**Vulnerable Parameter:** `alg` header parameter  
**Impact:** Authentication bypass → Admin access  

**Steps:**
1. Login as `wiener:peter`
2. Capture GET `/my-account` in Burp → Send to Repeater
3. Change to GET `/admin` → 401 error
4. In JWT Editor tab, modify payload: `sub: "administrator"` → Send → Still 401 (signature invalid)
**Attack:**
5. Click "Attack" → "none algorithm" → Choose first option
6. Notice token changes to: `header.payload.` (no signature, ends with dot)
7. Send → 200 OK (admin access)
8. Find delete link for `carlos` in response
9. Send GET `/admin/delete?username=carlos`
10. Lab solved

**Key Learning:**  
When `alg: "none"`, the JWT should have NO signature (just `header.payload.`).  
Some servers skip verification if algorithm is "none".
