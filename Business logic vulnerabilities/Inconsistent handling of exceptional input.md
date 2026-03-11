# Lab Walkthrough: Inconsistent Handling of Exceptional Input

After truncation, the stored email will appear as:


AAAAAAAAAAAAAAAAAAAA...@dontwannacry.com


This tricks the application into thinking the user belongs to the **DontWannaCry organization**.

---

## 10. Register the Exploit Account

Register a new account using the crafted email.

Example:


username: test5
password: test5
email: [crafted payload]


Confirm the email using the **Email Client**, then log in.

When you view your profile, the email will end with:


@dontwannacry.com


---

## 11. Access the Admin Panel

Now navigate to:


/admin


You will gain **administrator access**.

Locate the user **carlos** and delete the account.

Once the user is deleted, the **lab is solved**.

---

## Key Takeaway

This vulnerability exists because the application **handles exceptional input inconsistently**, specifically when processing **very long email addresses**.

The server **truncates the email field at 255 characters**, which allows attackers to manipulate how the domain is interpreted.

### To prevent this issue

- Enforce strict **input length validation**
- Validate **email domains before storage**
- Avoid truncating **security-sensitive fields**
- Perform **server-side validation** on all critical inputs

Improper handling of exceptional input can lead to **privilege escalation and unauthorized administrative access**.
