# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?id[$ne]=
    ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack.
   - The insecure version lacks proper error handling - there's no try/catch block to handle database query errors.
   - It doesn't implement any rate limiting, allowing unlimited requests in a short time period.
   - The application directly uses unsanitized user input in database queries without validation.
   - NoSQL injection vulnerabilities can be exploited to create complex queries that consume excessive server resources.

2. Briefly explain how a malicious attacker can exploit them.
   - An attacker can send a large number of requests in rapid succession to overwhelm the server.
   - They can use NoSQL injection with operators like `$ne` to create queries that return large result sets.
   - Without rate limiting, the attacker can automate these requests to continuously consume server resources.
   - The lack of error handling means that malformed queries can crash the server entirely rather than being gracefully handled.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?
   - Implementation of rate limiting using the `express-rate-limit` middleware that restricts each IP to 1 request per 5 seconds.
   - Proper error handling with try/catch blocks to prevent server crashes when database queries fail.
   - Returning appropriate error messages without exposing sensitive information.
   - The rate limiter returns a "Server is busy" message when limits are exceeded, maintaining availability for other users.