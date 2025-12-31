**Vulnerability:** Information disclosure vulnerability

**Vulnerable Parameter:** productId parameter 

**Impact:** Optain sensetive information about the server

**URL:** https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-in-error-messages
## Steps:
1. Open your lab
2. Open burpsuite send the home page request to repeater we try to break the cookie query with `'` to see if any error accure Cookie: session=cookievalue' nothing happens
3. Next we see products view a product and send the request to repeater  
4. We see an interesting parameter `GET /product?productId=4` we try to miniplate it `GET /product?productId=4we` Error accured
6. Notice in the response you get system messages the last line contain apache and the version paste it in the lab solution
8. Lab solved

## Notes:
- When looking for information disclouser vulnerabilities always look for parameters to miniplate with like we did with product id
- Look for entries you can miniplate with








