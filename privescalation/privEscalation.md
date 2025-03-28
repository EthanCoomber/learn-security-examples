# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

    ```
        http://localhost:3000/send-form
    ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
   - The insecure version has a critical authentication flaw in the `/update-role` endpoint. It attempts to authenticate users by checking if the user with the provided userId has an admin role, but this means an attacker can simply provide the userId of an admin (e.g., userId=1) in their request to gain admin privileges.
   - The application doesn't maintain any session state to verify the actual identity of the requester.
   - There's no proper separation between authentication (verifying who you are) and authorization (verifying what you're allowed to do).

2. Briefly explain how a malicious attacker can exploit them.
   - An attacker can exploit this by sending a POST request to `/update-role` with the userId of the admin (userId=1) and their own userId as the target to update.
   - For example, if an attacker knows that userId=1 is an admin, they can send a request with userId=1 and specify that userId=2 should be given admin role.
   - The server will check if userId=1 is an admin (which it is), and then grant the role change, even though the attacker isn't actually authenticated as the admin user.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?
   - The secure version implements proper session-based authentication using `express-session`.
   - It separates authentication (checking if a user is logged in via session) from authorization (checking if the logged-in user has admin privileges).
   - The server maintains session state to track the actual authenticated user making the request.
   - It uses secure cookie settings with `httpOnly` and `sameSite: 'strict'` to protect against session hijacking and CSRF attacks.
   - The application checks the session's userId (req.session.userId) to determine the actual logged-in user, rather than trusting user input.
   - Only after confirming the logged-in user is an admin does it allow role changes, preventing privilege escalation attacks.