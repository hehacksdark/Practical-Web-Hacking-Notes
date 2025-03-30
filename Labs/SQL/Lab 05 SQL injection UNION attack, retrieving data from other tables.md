# Lab: SQL Injection UNION Attack - Retrieving Data from Other Tables

## Steps to Solve the Lab:

1. **Intercept the Request**  
   - Use Burp Suite to intercept and modify the request that sets the product category filter.

2. **Determine the Number of Columns**  
   - Identify how many columns are returned by the query using:  
     ```sql
     '+UNION+SELECT+NULL,NULL--
     ```
   - Adjust the number of `NULL` values until the query executes without an error.

3. **Identify Columns Containing Text Data**  
   - Replace `NULL` values with random text to check which columns accept text:  
     ```sql
     '+UNION+SELECT+'abc','def'--
     ```
   - If an error occurs, adjust the positions of text values.

4. **Extract Data from the Users Table**  
   - Use the following payload to retrieve usernames and passwords:  
     ```sql
     '+UNION+SELECT+username,+password+FROM+users--
     ```
   - Verify that the response contains usernames and passwords.

## Expected Outcome:
- The application's response should display usernames and passwords from the `users` table.
