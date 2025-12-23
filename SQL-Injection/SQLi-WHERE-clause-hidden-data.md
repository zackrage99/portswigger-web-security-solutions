# Lab: Basic SQLi in WHERE clause

**Vulnerability:** SQL injection in `category` parameter  
**Vulnerable Parameter:** `category` (GET request)
**Impact:** Information disclosure (view hidden products)  
**Technique:** Basic WHERE clause injection

## Steps:
1. Found `category` parameter in `/filter?category=Corporate+gifts`
2. Tested with `'` → 500 error (confirmed injectable)
3. Added `' OR 1=1--` → All products revealed
4. Lab solved

## Payload:
