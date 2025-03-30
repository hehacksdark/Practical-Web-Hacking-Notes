# Lab: SQL Injection UNION Attack - Determining the Number of Columns  

## Overview  
This lab demonstrates how to determine the number of columns returned by a SQL query using a **UNION-based SQL Injection attack**.  

## Steps to Exploit  

1. **Intercept the Request:**  
   - Open **Burp Suite** and enable **intercept** mode.  
   - Navigate to the product category page.  
   - Capture the request that includes the `category` parameter.  

2. **Modify the `category` Parameter:**  
   - Change its value to:  

     ```sql
     ' UNION SELECT NULL--  
     ```  

   - Submit the request.  
   - If an error occurs, it indicates that the number of selected columns is incorrect.  

3. **Determine the Correct Number of Columns:**  
   - Gradually increase the number of **NULL** values until the error disappears.  
   - Modify the `category` parameter by adding additional `NULL` values:  

     ```sql
     ' UNION SELECT NULL, NULL--  
     ```  

     ```sql
     ' UNION SELECT NULL, NULL, NULL--  
     ```  

     - Continue adding `NULL` values **one at a time** until the error **stops appearing** and the response includes additional content.  
     - The correct number of columns is the number of `NULL` values in the successful query.  

## Explanation  
- The **UNION operator** is used to combine results from multiple queries.  
- To work, the number of columns in both queries **must match**.  
- By testing with `NULL` values, we determine how many columns the original query returns.  

## Mitigation  
- **Use prepared statements (parameterized queries).**  
- **Disable verbose error messages to prevent information disclosure.**  
- **Implement input validation and allowlisting.**  
- **Use Web Application Firewalls (WAFs) to detect SQLi attempts.**  
