
**Vulnerability:** Server doesn't verify the signature of any JWTs that it receives

**Vulnerable Parameter:** Authentication bypass

**Impact:** Access admin account

**URL:** https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-unverified-signature

1. Open your lab
2. Log in with given credentials wiener:peter
3. Open burp suite and make sure you installed the jwt editor extension
4. send the request GET /my-account?id=wiener to repeater
5. Change the ` GET /my-account?id=wiener ` to `GET /admin` notice you got error
6. From the json web token tab looking at the payload section you have `sub:wiener` we try to change it to `sub:administrator`
7. Send the request notice you got `200 ok` 
8. Now search for carlos in the response you should see a link to delet the user carlos
9. Copy the link replcae it `GET /admin/delete?username=carlos` send the request
10. Lab solved

## Notes:
- The server accepts JWTs without validating the signature
- You can tamper with the payload (change sub from "wiener" to "administrator") and the server blindly trusts it
- No signature validation = anyone can forge tokens








