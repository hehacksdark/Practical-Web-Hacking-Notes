# Lab: SSRF with Blacklist-Based Input Filter

## Overview
This lab demonstrates how to bypass blacklist-based input filters to exploit a Server-Side Request Forgery (SSRF) vulnerability and gain access to an internal admin panel.

---

## **Steps to Solve the Lab**

### 1. **Intercept the Stock API Request**
- Visit any product page.
- Click **"Check stock"** and intercept the request using **Burp Suite**.
- Send the request to **Burp Repeater**.

### 2. **Test for SSRF**
- Modify the `stockApi` parameter to `http://127.0.0.1/`.
- Observe that the request is **blocked** due to blacklist filtering.

### 3. **Bypass the Blacklist**
- Change the `stockApi` parameter to `http://127.1/`.
- This bypasses the blacklist, allowing access to the localhost.

### 4. **Access the Admin Panel**
- Modify the URL to `http://127.1/admin`.
- Notice that the request is **blocked** again due to filtering.

### 5. **Bypass Blacklist Using Double URL Encoding**
- Obfuscate the letter "a" in "admin" using **double URL encoding**:
http://127.1/%2561dmin

pgsql
Copy
Edit
- This successfully bypasses the filter and grants access to the admin panel.

### 6. **Delete the Target User**
- Identify the **delete user** endpoint in the admin panel.
- Submit the following payload in `stockApi`:
http://127.1/%2561dmin/delete?username=carlos

yaml
Copy
Edit
- The user **Carlos** is deleted, successfully solving the lab.

### **PS** - We can also use case-manipulation bypass, IPv6 which is generally misconfigured due to lack of knowledge.

---

## **Key Takeaways**
✅ **Blacklists can be bypassed** using alternative IP representations (e.g., `127.1`).  
✅ **Encoding tricks** like **double URL encoding** can evade string-based filtering.  
✅ **Testing various encoding methods** (e.g., UTF-8, hex, octal) can help bypass security filters.  
✅ **Always validate and sanitize user inputs properly** to prevent SSRF attacks.

---

## **References**
- [PortSwigger SSRF Labs](https://portswigger.net/web-security/ssrf)
- [Bypassing SSRF Blacklists](https://book.hacktricks.xyz/pentesting-web/ssrf-server-side-request-forgery)
