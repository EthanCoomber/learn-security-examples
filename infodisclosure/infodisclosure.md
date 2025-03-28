# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?username[$ne]=
    ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
   - The insecure version directly uses user-provided input in the MongoDB query without any validation or sanitization.
   - It passes the raw query parameter to the `findOne()` method, allowing MongoDB operators like `$ne` to be injected.
   - The application doesn't validate the type or format of the input before using it in database operations.
   - It returns the complete user object, potentially exposing sensitive information like passwords.

2. Briefly explain how a malicious attacker can exploit them.
   - An attacker can use NoSQL injection by passing MongoDB operators in the query parameters.
   - For example, using `username[$ne]=` creates a query that finds documents where username is not equal to an empty string, effectively returning all users.
   - This allows attackers to bypass authentication and retrieve information about all users in the database.
   - The attacker doesn't need to know any valid usernames to access user data.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?
   - Input validation: The secure version checks that the username is a string type before processing.
   - Input sanitization: It removes non-alphanumeric characters from the username using regex (`username.replace(/[^\w\s]/gi, '')`).
   - Error handling: It implements proper try/catch blocks to handle errors gracefully without exposing system details.
   - Specific error messages: It returns generic error messages that don't reveal whether a username exists or not.
   - Type checking: It explicitly validates the data type of inputs before using them in database queries.