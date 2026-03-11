# Lab Walkthrough: Inconsistent Handling of Exceptional Input

## Objective

Exploit a **logic flaw caused by inconsistent handling of exceptional input** to register an account that appears to belong to the **@dontwannacry.com** domain and gain access to the **admin panel**, then delete the user **carlos**.

---

## Tools Used

- Browser
- Burp Suite (Community or Professional Edition)

---

## Step-by-Step Guide

## 1. Launch the Lab and Analyze the Application

Launch the lab and open **Burp Suite**.

While exploring the application you will notice:

- A **registration form**
- An **email client**
- An **admin panel**

Try accessing the admin panel directly:

```
/admin
```

You will receive an **access denied** error, meaning only privileged users can access it.

---

## 2. Inspect the Registration Message

On the registration page you will see the following message:

```
If you work for DontWannaCry, please use your @dontwannacry.com email address
```

This indicates that **users with the @dontwannacry.com email domain may receive special privileges**, possibly access to the admin panel.

First, register a normal account.

Example:

```
username: test
email: attacker@exploit-0ab700e0030750db809b0c2b01bc0008.exploit-server.net
password: test
```

The email domain is provided by the **email client server**.

---

## 3. Confirm the Account

After registering:

1. Open the **Email Client**
2. Locate the verification email
3. Click the confirmation link
4. Log in to the account

Now try accessing:

```
/admin
```

You will still receive the **unauthorized access message**.

---

## 4. Investigate the Email Client

Inside the email client you will see the following message:

```
Displaying all emails @exploit-0ab700e0030750db809b0c2b01bc0008.exploit-server.net and all subdomains
```

This suggests the email client accepts **all subdomains of the exploit server**.

This gives us an idea to craft an email such as:

```
test@dontwannacry.exploit-0ab700e0030750db809b0c2b01bc0008.exploit-server.net
```

Since **dontwannacry** appears in the domain, the application might treat it as a company email.

---

## 5. Test Email Length Handling

Before crafting the exploit, we should check whether the application accepts **arbitrarily long email addresses**.

Send a registration request to **Burp Intruder**.

Example request:

```
POST /register

csrf=WSPdszaHUSj8YTNSsgOXOkXkJcIUFaK3&username=test&email=test%40dontwannacry.com.exploit-0aff0004033444a9822cf5ce01b400ac.exploit-server.net&password=test
```

Highlight the **username value** and mark it as the payload position:

```
username=$test$
```

---

## 6. Configure Burp Intruder

In the **Payloads** tab configure the following:

### Payload Type

```
Character block
```

### Payload Settings

```
Minimum length: 100
Maximum length: 500
Step: 100
```

Start the attack.

After the attack finishes, take the **largest payload request**.

---

## 7. Check the Email Client

Go back to the **Email Client**.

You should see **multiple new verification emails**.

Confirm one of them and log in.

Example credentials:

```
username: test
password: test
```

Now check the **email displayed in the account profile**.

You will notice something interesting:

The email only shows a **long sequence of A characters**, and the rest of the domain is missing.

This means the server is **truncating the email value**.

---

## 8. Determine the Truncation Length

Copy the A characters and count them using any **character counting website**.

You will find that the stored length is:

```
255 characters
```

This confirms that the application **truncates email input after 255 characters**.

---

## 9. Craft the Exploit Email

We can exploit this behavior by ensuring that **@dontwannacry.com appears before the truncation point**.

Example payload:

```
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA@dontwannacry.com.exploit-0aff0004033444a9822cf5ce01b400ac.exploit-server.net
```

After truncation, the stored email will appear as:

```
AAAAAAAAAAAAAAAAAAAA...@dontwannacry.com
```

This tricks the application into thinking the user belongs to the **DontWannaCry organization**.

---

## 10. Register the Exploit Account

Register a new account using the crafted email.

Example:

```
username: test5
password: test5
email: [crafted payload]
```

Confirm the email using the **Email Client**, then log in.

When you view your profile, the email will end with:

```
@dontwannacry.com
```

---

## 11. Access the Admin Panel

Now navigate to:

```
/admin
```

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
