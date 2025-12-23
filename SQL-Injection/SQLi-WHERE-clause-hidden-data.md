# Lab: Basic SQLi in WHERE clause

**Vulnerability:** SQL injection in `category` parameter  
**Vulnerable Parameter:** `category` (GET request)
**Impact:** Information disclosure (view hidden products)  
**Technique:** Basic WHERE clause injection
** URL: https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data

## Steps:
1. Search for endpoints that uses sql query
1. Found `category` parameter in `/filter?category=Corporate+gifts`
2. Tested with `'` → 500 error (confirmed injectable)
3. Added `' OR 1=1--` → All products revealed
4. Lab solved

## Payload:
/filter?category=Corporate+gifts'+OR+1=1--

## Notes:
we put the payload 1=1 to make the condetion true
we closed the sql query with `'` and put `1=1` since 1 will always equal to one it will display true and reveal the hiddeen products
