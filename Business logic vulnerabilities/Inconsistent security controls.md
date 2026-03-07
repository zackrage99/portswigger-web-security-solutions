## Lab Walkthrough: Inconsistent Security Controls

### 🎯 Objective

Exploit a business logic flaw where security controls are inconsistently applied during the email update process to gain administrative access and delete the user **carlos**.

## 🛠️ Tools Used

* **Browser** (Integrated with PortSwigger Labs)
* **Email Client** (Provided lab server)
* **Burp Suite Professional/Community Edition**

## 📖 Step-by-Step Guide

### 1. Setup and Registration
* Launch the lab and click on the **Register** button.
* Notice the message: **"If you work for DontWannaCry, please use your @dontwannacry.com email address"**.
* Click the **Email client** button to access your assigned email server.
* Copy your email address (e.g., attacker@exploit-server.net) and register a new account.
* Check your **Email client**, click the registration link, and log in.

### 2. Identify the Security Restriction
* Try to navigate to the **/admin** panel.
* Notice the error: **"Admin interface only available if logged in as a DontWannaCry user"**.
* This shows the system checks your email domain specifically for administrative access.

### 3. Exploit Inconsistent Controls
* Navigate to the **My Account** page.
* Locate the **Email** update field.
* Change your email to a restricted domain address, for example: **test@dontwannacry.com**.
* Click **Update email**.
* Notice that the application updates the email immediately without requiring a verification link for the new domain.

### 4. Access the Admin Panel
* Now that your account is associated with a **@dontwannacry.com** address, navigate to **/admin** again.
* You should now have access to the admin interface.

### 5. Solve the Lab
* Find the user **carlos** in the list.
* Click **Delete**.
* The lab banner will update to **Congratulations, you solved the lab!**

### 💡 Key Takeaway

**Security controls must be applied consistently across all features.** In this case, the registration process was secure, but the "Update Email" feature lacked the same domain verification, allowing an attacker to self-escalate their privileges.
