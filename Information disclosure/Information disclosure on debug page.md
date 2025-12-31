**Vulnerability:** Information disclosure vulnerability

**Vulnerable Parameter:** Debug page

**Impact:** Optain sensetive information about the server

**URL:** https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-on-debug-page
## Steps:
1. Open your lab
2. Open burpsuite send the home page request to repeater i applied all the previous steps nothing seemed to work
3. I even tried to change the request method to see if anything weired happend
4. Back to home page observe the response scrolling and searching we find something interesting `<!-- <a href=/cgi-bin/phpinfo.php>Debug</a> -->`
6. We go to that directory `/cgi-bin/phpinfo.php` Notice you got information page
7. Keep scrolling untill you see the secret key copy it and paste it in the lab solution
8. Lab solved

## Notes:
- When not seeing any signs of errors or messages keep observing each page response and read all the source code









