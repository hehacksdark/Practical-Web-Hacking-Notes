# Lab: SQL Injection Attack - Querying the Database Type and Version on MySQL and Microsoft

## Steps to Solve the Lab:

1. **Intercept the Request**  
   - Use Burp Suite to capture and modify the request that sets the product category filter.

2. **Determine the Number of Columns**  
   - Identify the number of columns returned by the query using:  
     ```sql
     '+UNION+SELECT+NULL,NULL#
     ```
   - Keep adding `NULL` values until the query executes without an error.

3. **Identify Columns Containing Text Data**  
   - Test which columns accept text by modifying the query:  
     ```sql
     '+UNION+SELECT+'abc','def'#
     ```
   - Ensure both columns accept string values.

4. **Retrieve Database Version**  
   - Use the following payload to retrieve and display the database version:  
     ```sql
     '+UNION+SELECT+@@version,+NULL#
     ```
   - The `@@version` function returns the database version.

5. **Verify Output**  
   - Check the application's response to confirm that the database version is displayed.

## Expected Outcome:
- The database version should be displayed in the response, confirming that the attack successfully retrieved system information.
