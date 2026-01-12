**Vulnerability:** The server signs an empty key
***URL:*** https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-kid-header-path-traversal

**Vulnerable Parameter:** kid parameter 
**Impact:** Authentication bypass → Admin access   

**Steps:**
1. Login as `wiener:peter`
2. Capture GET `/my-account` in Burp → Send to Repeater
3. Change to GET `/admin` → 401 error
4. In JWT Editor tab, modify payload: `sub: "administrator"` → Send → Still 401 (signature invalid)
5. Notice the `alg:HS256` used is symmetric meaning it has 1 key for both signing and verifying the token
6. From the lab we know The server is vulnerable to path traversal in the kid parameter `the server uses the kid parameter in JWT header to fetch the relevant key from its filesystem`
7. Now we need to find a way to make the kid parameter reads from an empty file and sign an empty key to the request

**Attack:**

8. From the repeater go to JSON Web Toekn tab in the kid parameter ` "kid": "../../../../dev/null"` 
9. Now click on attack and sign with empty key 
10. Change the request to `GET /admin` and `"sub": "administrator"`
11. Notice You got 200 ok response meaning now are now in the administrator panel search for `carlos` you will find a link to delete the user carlos copy it and replace it in GET /admin
12. Lab solved

**NOTES:** 
- The kid (Key ID) parameter in JWT headers is used to identify which key should be used to verify the token
- When we made the kid read from /dev/null we essentialy made it reads from an empty file because the /dev/null is a special file in linux :
  1.  Reads from /dev/null: Always returns EOF (end of file) immediately


