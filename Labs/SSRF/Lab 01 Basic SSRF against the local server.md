## Lab: Basic SSRF against the Local Server  -  Portswigger Lab (1)

### 1️⃣ Accessing the Admin Page Restriction  
- Browsed to `/admin` but was **unable to access it directly**.  

### 2️⃣ Identifying the SSRF Attack Vector  
- Visited a product page and clicked **"Check stock"**.  
- Intercepted the request using **Burp Suite** and sent it to **Burp Repeater**.  
- Found a **stockApi** parameter making external requests.  

### 3️⃣ Exploiting SSRF to Access Admin Panel  
- Modified the **stockApi** parameter to:  
http://localhost/admin

markdown
Copy
Edit
- Successfully accessed the **administration interface**.  

### 4️⃣ Finding the User Deletion Endpoint  
- Inspected the HTML response and found a URL to **delete the target user**:  
http://localhost/admin/delete?username=carlos

markdown
Copy
Edit

### 5️⃣ Performing the SSRF Attack  
- Sent the **stockApi** parameter with the user deletion request:  
http://localhost/admin/delete?username=carlos

yaml
Copy
Edit
- Successfully deleted the user and completed the lab. ✅  

---

## 🛡️ **Mitigation Strategies**  
To prevent **SSRF vulnerabilities**, implement the following security measures:  

1️⃣ **Use an Allowlist:** Restrict the `stockApi` parameter to only allow trusted domains.  
2️⃣ **Block Localhost Requests:** Prevent requests to `127.0.0.1`, `localhost`, `169.254.169.254`.  
3️⃣ **Enforce Authentication:** Require authentication for internal admin pages.  
4️⃣ **Validate & Sanitize Inputs:** Reject user-controlled URLs that attempt to access internal resources.  

---

🚀 **Follow me for more security labs & writeups!** 🔥  
