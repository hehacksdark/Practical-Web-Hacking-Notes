## Lab: SQL Injection UNION Attack - Finding a Column Containing Text

### **Objective**
Identify a column that can store and display text data using SQL injection.

---

### **Steps to Exploit**

1. **Intercept the Request**
   - Use **Burp Suite** to capture the request that sets the product category filter.

2. **Determine the Number of Columns**
   - Modify the `category` parameter to find the correct number of columns:
     ```sql
     '+UNION+SELECT+NULL,NULL,NULL--
     ```
   - Ensure that no error occurs and that the number of `NULL` values matches the column count.

3. **Find a Column That Accepts Text**
   - Replace **one NULL at a time** with the **random text value** provided by the lab.
   - Example:
     ```sql
     '+UNION+SELECT+'abcdef',NULL,NULL--
     ```
   - If an error occurs, move on to the next column:
     ```sql
     '+UNION+SELECT+NULL,'abcdef',NULL--
     ```
   - Continue this process until the text appears in the page response.

4. **Confirm Successful Exploitation**
   - Once the text value appears on the page, that column is **text-compatible**.
   - This column can now be used to extract useful data (e.g., **database version, usernames, passwords**).

---

### **Key Takeaways**
- The column must be capable of displaying **text output** on the webpage.
- Testing one column at a time ensures we identify the **correct** column.
- This step is crucial before extracting **meaningful data** using further SQL injection techniques.

---

🎯 **Next Step:** Use this identified column to retrieve sensitive data (e.g., `database()`, `user()`, or `table_names`). 🚀
