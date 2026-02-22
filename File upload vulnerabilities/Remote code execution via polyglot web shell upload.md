# Lab: Remote code execution via polyglot web shell upload

**Target URL:** `https://portswigger.net/web-security/file-upload/lab-file-upload-remote-code-execution-via-polyglot-web-shell-upload`

---

## 🎯 Vulnerability Overview
The application validates uploaded files by checking their internal structure (**Magic Bytes**) to ensure they are genuine images. To bypass this, we use a **Polyglot Web Shell**—a file that is structurally a valid image but also contains executable PHP code within its metadata. This allows us to satisfy both the image validator and the PHP interpreter.

* **Vulnerability:** Web shell upload via Polyglot bypass
* **Vulnerable Parameter:** `avatar` (POST request)
* **Impact:** Remote Code Execution (RCE) / Access to sensitive files (`/home/carlos/secret`)

---

## 🛠️ Exploitation Steps

### 1. Identify the Strict Validation
Log in to the lab using `wiener:peter`. In this scenario, the server doesn't just check the extension; it inspects the file content. A simple PHP file renamed to `.jpg` will be rejected because it lacks the "Magic Bytes" of a real image.

### 2. Create the Polyglot Shell
We will use **ExifTool** (pre-installed in Kali Linux) to embed our PHP payload into a legitimate image. This ensures the file remains a valid image while carrying our malicious code.

1.  Download a legitimate image (e.g., `image.jpg`).
2.  Run the following command to create the polyglot:
    ```bash
    exiftool -Comment="<?php echo 'start shell ' . file_get_contents('/home/carlos/secret') . ' end shell'; ?>" image.jpg -o shell.php
    ```
3. ![Injecting Shell](https://github.com/zackrage99/portswigger-web-security-solutions/blob/main/images/File%20upload%20vulnerabilities/lab6/shell.png)
3.  **Verify the payload:** Run `exiftool shell.php` and look for your PHP code in the "Comment" section.



### 3. Upload the Payload
1.  Navigate to the **My Account** page and upload `shell.php` as your avatar.
2.  Capture the request in **Burp Suite**.
3.  The server should return a `200 OK` because the file starts with the required image Magic Bytes (e.g., `FF D8 FF` for JPEG), even though it has a `.php` extension.

### 4. Execute and Retrieve the Secret
1.  Navigate to your account page to trigger the avatar load or find the `GET` request for the avatar: `/files/avatars/shell.php`.
2.  View the **Raw** response of this `GET` request in Burp Suite.
3.  The response will contain binary "garbage" (the image data). Use `Ctrl+F` to search for the markers we set: `start shell`.
4.  The secret string will be visible between `start shell` and `end shell`.



### 5. Lab Completion
Copy the secret string from the response and submit it to solve the lab.

---

## 📝 Key Takeaways

### The "Two Bosses" Strategy
To successfully upload a polyglot, you must satisfy two different parsers simultaneously:
* **The Image Validator (The Header):** Every file type has "Magic Bytes" at the start. For example, a JPEG starts with `FF D8 FF`. Functions like `getimagesize()` check these bytes to "trust" the file.
* **The PHP Interpreter (The Payload):** The PHP engine ignores binary data until it finds the `<?php` tag. By hiding our code in the **Metadata (EXIF Comment field)**, we prevent the code from corrupting the image structure while ensuring it remains executable.

### Security Best Practices
* **Strip Metadata:** Applications should always strip EXIF data and comments from uploaded images using libraries like `ImageMagick`.
* **Re-encoding:** The safest way to handle uploads is to re-encode the image (e.g., convert JPG to PNG and back), which usually destroys any embedded polyglot payloads.
* **Rename on Store:** Never use the user-supplied filename or extension on the server's filesystem.

---

Would you like me to show you how to perform this same bypass using a hex editor to manually insert the magic bytes?
