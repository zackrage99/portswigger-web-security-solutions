**Vulnerability:** The server uses a jku header parameter to verify tokens but fails to check the url if it's from trusted source

***URL:*** https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-jku-header-injection 
**Vulnerable Parameter:** jku header parameter  
**Impact:** Authentication bypass → Admin access   

**Steps:**
1. Login as `wiener:peter`
2. Capture GET `/my-account` in Burp → Send to Repeater
3. Change to GET `/admin` → 401 error
4. In JWT Editor tab, modify payload: `sub: "administrator"` → Send → Still 401 (signature invalid)
5. Notice the `alg:RS256` used is asymmetric meaning it has 2 keys public and private for both signing and verifying the token
6. From the lab we know The server supports the jku parameter in the JWT header. However, it fails to check whether the provided URL belongs to a trusted domain before fetching the key
7. **Attack**: First go to JWT Editor tab and click `New RSA key` and generate 
8. Now right click on your newly generated key and copy public key as jwk
9. Now go to your exploit server provided by port swigger lab and in the Body section paste your key
```
{"keys":[
{
    "kty": "RSA",
    "e": "AQAB",
    "kid": "4fb5f5c5-f604-4415-a8f2-1ef257bbc9db",
    "n": "lPYKfbBV_XFGBkdb-mAGYgmwguwhbIYY6iJlEqjO1BF7-p_Ll9E3-UFn-qJL-av3M8S7XqzONSuGz0xgraUz37nHpHvBxktvdCh7VOfTjiEHvv0PJaa1Y28f3UpWTMifvI9Ub2gsQxSQymrtjKQS6sywGRfpQPduybyxainoSWo9fa63AIMnSMIabKxf7a-HmXlFetIxP4xOEqNYpXehvmcM1dwPAgl211vRCLT7tr0JH1EAVvEHGo4I2_22oegpq6eIB0Iyf1QME_HcKIOdo-7DK1PZRTI91zsBGSk12SxUtTka-mSgjCd-UjT_8gD_NVyjHaT2Hf13RZupySjOsw"
}
]}
```

10. The next step is to copy the `kid` value from your key and paste it in `JSON Web Token` tab in repeater and place in the `kid` parameter value
11. Now we need to add the 'jku' header which will be the link of our exploit server paste the link of our exploit server
```
{
    "kid": "4fb5f5c5-f604-4415-a8f2-1ef257bbc9db",
    "alg": "RS256",
    "jku": "https://exploit-0ad100180321a7098016caf501bf00ff.exploit-server.net/exploit"
}
```
12. Now we need to sign our generated key to match the key in the exploit server click on Sign in the tab under and ok
13. Change the request to `GET /admin` and 
14. Notice You got 200 ok response meaning now are now in the administrator panel search for `carlos` you will find a link to delete the user carlos copy it and replace it in GET /admin
15. Lab solved

**NOTES:** 
- The server here count on the `jku` header to verify public keys but it fails to check if the url came from trusted source that's why we injected our exploit server

