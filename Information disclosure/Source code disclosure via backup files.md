**Vulnerability:** Information disclosure vulnerability

**Vulnerable Parameter:** /backup directory 

**Impact:** Optain sensetive information about the server database

**URL:** https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-via-backup-files
## Steps:
1. Open your lab
2. First thing i do is to broswe /robots.txt
3. We see something interesting `Disallow: /backup`  
4. Go to the /backup folder by replacing /backup after your lab link a file `ProductTemplate.java.bak	` is shown open it
5. Notice a function contain database name `postgres` indicating it is PostgreSQL database under it you see the password copy it and paste it in the lab solution
6. Lab solved









