
# Lab: Web shell upload via extension blacklist bypass

**Target URL:** `https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-extension-blacklist-bypass`

---

## 🎯 Vulnerability Overview
The application attempts to prevent malicious file uploads by using a **blacklist** of forbidden extensions (like `.php`). However, because the server is running Apache, we can bypass this restriction by uploading a custom `.htaccess` file to redefine how the server interprets non-blacklisted file extensions.

* **Vulnerability:** Extension Blacklist Bypass via Apache Configuration Overriding
* **Vulnerable Parameter:** `avatar` (POST request)
* **Impact:** Remote Code Execution (RCE) by forcing the server to execute arbitrary file types as PHP scripts.

---

## 🛠️ Exploitation Steps

### 1. Identify the Filter
1.  Log in to the lab using `wiener:peter`.
2.  Attempt to upload a standard PHP shell (e.g., `exploit.php`).
3.  Notice the error message: `php files are not allowed`. This confirms a blacklist is in place.
4.  Try an alternative extension like `exploit.php3`. The upload succeeds (**200 OK**), but when you visit the file, the server returns the **raw text** instead of executing it.

### 2. Identify the Server Environment
Observe the headers in the response from the server. You will see:
`Server: Apache/2.4.41 (Ubuntu)`

This confirms the environment is **Apache**, which typically supports `.htaccess` files for directory-level configurations.

### 3. Upload a Malicious .htaccess File
We need to tell Apache to treat a specific, non-blacklisted extension as a PHP executable.
1.  Capture an upload request in **Burp Suite** and send it to **Repeater**.
2.  Change the `filename` to `.htaccess`.
3.  Change the `Content-Type` to `text/plain`.
4.  Replace the file content with the following configuration:
    ```apache
    AddType application/x-httpd-php .php3
    ```
    *(Note: This maps the `.php3` extension to the PHP handler.)*
5.  Send the request. The server should accept the `.htaccess` file.

### 4. Upload and Execute the Shell
1.  Now, upload your shell again but rename it to `exploit.php3`.
2.  The content of the shell should be:
    ```php
    <?php echo file_get_contents('/home/carlos/secret'); ?>
    ```
3.  Once uploaded, navigate to the file location (usually `/files/avatars/exploit.php3`).
4.  Because of our `.htaccess` rule, the server will now execute the PHP code within the `.php3` file.

### 5. Lab Completion
Check the response of the `GET` request for `exploit.php3`. The body will contain the secret string from Carlos's home directory. Copy and submit it to solve the lab.

---

## 📝 Key Takeaways

### What is .htaccess?
The **.htaccess** (Hypertext Access) file is a configuration file used by Apache web servers to allow directory-level changes without modifying the main server configuration files.
* **Purpose:** Override Apache's default settings for specific directories.
* **Control:** Can be used to enable/disable features or change file handlers (e.g., forcing a `.txt` file to run as code).
* **Instant:** No server restart is required after changes; the server checks the file for every request in that directory.

### Security Best Practices
* **Whitelisting vs. Blacklisting:** Never rely on a blacklist of "bad" extensions. Use a **whitelist** of allowed types (e.g., `.jpg`, `.png`).
* **Disable Overrides:** Configure the server to ignore `.htaccess` files in upload directories using the `AllowOverride None` directive.
* **Execution Permissions:** Ensure that directories where user files are uploaded do not have "Execute" permissions enabled for the web server user.

---

Would you like me to help you draft a similar walkthrough for a different file upload bypass, such as using null byte injections or polyglot files?
