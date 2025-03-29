# 🛡️ Lab: Basic SSRF Against Another Back-End System

## 📝 Summary
This lab demonstrates **Server-Side Request Forgery (SSRF)** to access an internal admin panel hosted on a different back-end system (`192.168.0.1:8080`). By fuzzing the internal IP range, we identify the admin interface and use it to delete a user.

---

## 🚀 Steps to Exploit

### **1️⃣ Intercept and Modify the Request**
1. Visit any product page and click **"Check stock"**.
2. Open **Burp Suite**, intercept the request, and send it to **Burp Intruder**.

### **2️⃣ Scan the Internal Network**
3. Modify the `stockApi` parameter to target an internal system:
http://192.168.0.1:8080/admin

4. Highlight the last octet (`1`) of the IP address and click **Add §**.
5. Go to the **Payloads** panel and:
- Set **Payload type** to `Numbers`.
- Configure:
  ```
  From: 1
  To: 255
  Step: 1
  ```
6. Click **Start Attack**.

### **3️⃣ Identify the Admin Interface**
7. Sort the results by **Status** column (ascending).
8. Look for a response with **Status 200** — this reveals the **admin panel**.

### **4️⃣ Exploit the Admin Panel**
9. Send the request with status `200` to **Burp Repeater**.
10. Modify the `stockApi` path to:
 ```
 /admin/delete?username=carlos
 ```
11. Send the request to delete the target user.

---

## 🔥 Key Takeaways
✅ **SSRF can expose internal services.**  
✅ **Burp Intruder helps automate scanning.**  
✅ **Sorting by status codes can reveal sensitive endpoints.**  
✅ **Internal admin panels often lack authentication.**  

---

## 🛡️ Mitigation Strategies
- **Restrict internal requests** (block `localhost`, `127.0.0.1`, `192.168.x.x`).  
- **Use allowlists** for external API requests.  
- **Authenticate internal admin interfaces.**  
- **Implement CSRF protection** to prevent abuse.  

---

### 🏴‍☠️ **Author**: [@hehacksdark](https://github.com/hehacksdark)
