**Lab:** SQL injection UNION attack, determining the number of columns returned by the query

**Vulnerability:** basic sql union queries

**Vulnerable Parameter:** category (GET request)

**Technique:** UNION attack with basic queries

**URL:** https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns
## Steps:
1. Find category parameter
2. Open Burp Suite and send request to Repeater
3. Test with `'` → Internal Server Error
4. Comment out with `--` → worked
5. Find column count: `' ORDER BY 2--` works, `' ORDER BY 3--` works, ORDER BY 4 fails (3 columns)
6. This should solve the lab but we will test in another method
7. ' UNION SELECT NULL, NULL, NULL--
8. Lab solved

## Notes:
Don't forget to url encode your payload before sending it in burpsuite (ctrl+r)
