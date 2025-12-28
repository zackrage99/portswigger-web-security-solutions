**Vulnerability:** Access control vulnerability

**Vulnerable Parameter:** Admin panel

**Impact:** Delete users

**URL:** https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality
## Steps:
1. Open your lab
2. Add robots.txt to the link  https://0a96008404f59fd9808a8fb1006400f9.web-security-academy.net/robots.txt
3. You find the administrator panel /administrator-panel
4. open https://0a96008404f59fd9808a8fb1006400f9.web-security-academy.net/administrator-panel
5. Notice the admin panel is opened delete the user carlos
6. Lab solved


## Notes:
- robots.txt often reveals sensitive directories
- robots.txt is a text file placed in the root directory of a website that tells web crawlers (like Googlebot) which parts of the site they should or should not access





