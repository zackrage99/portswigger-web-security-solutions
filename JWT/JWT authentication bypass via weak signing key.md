**Vulnerability:** The server uses an extremely weak secret key to both sign and verify tokens

***URL:*** https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-weak-signing-key 

**Vulnerable Parameter:** jwk token  
**Impact:** Authentication bypass → Admin access   

**Steps:**
1. Login as `wiener:peter`
2. Capture GET `/my-account` in Burp → Send to Repeater
3. Change to GET `/admin` → 401 error
4. In JWT Editor tab, modify payload: `sub: "administrator"` → Send → Still 401 (signature invalid)
5. Notice the `alg:HS256` used is symmetric meaning it has one key for both signing and verifying the token
6. From the lab we know this lab has a weak siging key we need to try to crack it

**Attack:**

7. First go to `/jwt.secrets.list` on github provided by port swigger download the whole list
8. Let's lunch hashcat
``` bash:
 hashcat -a 0 -m 16500 eyJraWQiOiI3MmM4YTdiMC1jNGUyLTRjNDYtOGE1NS1lNThmNDBiNWRkZTIiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTc2ODIxMjA0Miwic3ViIjoid2llbmVyIn0.DKlTfk7Rn35dbgwhSbZDW8N1InApyV0aufPh-iSAKYE jwtsecretlist.txt
```
9. hashcat immedialty cracked the password which is 'secret1'
10. The next step is to encode the `secret1` as base64 from burp go to Decoder tab paste your text encode as base64 now copy the result
11. From the JWTeditor tab go to New Symmetric Key and click generate and paste your base64 text in k parameter
12. Now after we hav our key we need to sign it
13. Go to repeater change the request `GET /my-account?id=wiener` to `GET /admin` and from jsonwebtoken tab change `"sub": "wiener"` to `"sub": "administrator"` and send the request
14. Notice You got 200 ok response meaning now are now in the administrator panel search for `carlos` you will find a link to delete the user carlos copy it and replace it in GET /admin
15. Lab solved

**NOTES:** 
- The value 16500 refers to the hash mode identifier in the password cracking tool Hashcat specifically used for brute-forcing JSON Web Tokens (JWTs) signed with the HS256 (HMAC-SHA256) algorithm
- JWTs use base64url-encoded keys in their structure. Look at the "k" parameter in your symmetric key - it's base64 encode that's why we base64 before using it
