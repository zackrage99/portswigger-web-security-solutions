**Lab**: SQL injection vulnerability allowing login bypass

**Vulnerability:** SQL injection in Login 
**Vulnerable Parameter:** Login (GET request)  
**Impact:** Allowing to login to any account including administrator  
**Technique:** Basic comment out the rest of the query  
**URL:** https://portswigger.net/web-security/sql-injection/lab-login-bypass

## Steps:
1. Go to Login page
2. The lab provided us with username `administrator`
3. Tested with `'` → Internal Server Error (confirmed injectable) 
4. Change username to administrator and add '--
5. add a random password	
6. Login
7. Lab solved

## Payload:
administrator'--

## Notes:
the `'` is for closing the query and the `--` is for commenting out the rest of the query
when the server sees the request it will log you in because you commented out the password field
