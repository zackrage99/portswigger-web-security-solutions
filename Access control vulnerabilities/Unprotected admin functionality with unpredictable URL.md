**Vulnerability:** Access control vulnerability

**Vulnerable Parameter:** Admin panel

**Impact:** Delete users

**URL:** https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality-with-unpredictable-url
## Steps:
1. Open your lab
2. From the lab description we know the location of the admin panel is disclosed somewhere in the application
3. First examin the main page (Inspect Element) we don't see anything interesting
4. Go to login page and examin it (Inspect Element) Notice the script containing an interesting link `/admin-qgvjcl`
5. Go to https://0adf008e03abb59e801fc6bd003e005d.web-security-academy.net/admin-qgvjcl
6. Notice you get the admin panel delete the user carlos
7. Lab solved








