# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Run the server __insecure.ts__.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.
   - The vulnerability in the insecure version is related to repudiation - the ability for users to deny their actions. In `insecure.ts`, there is no proper logging mechanism, no authentication system, and no way to reliably track who performed which actions. The server simply accepts messages with user-provided identities without verification, and there's no audit trail of who sent what message or when. This allows malicious users to send messages as anyone and later deny their actions.

2. Briefly explain why the vulnerability is addressed in __secure.ts__.
   - The secure version addresses the repudiation vulnerability through several mechanisms:
     - Comprehensive logging: It logs all requests with timestamps, IP addresses, and actions
     - Error handling with logging: All errors are captured and logged
     - Audit trail: Message sending is logged with user information and timestamps
     - Authentication placeholder: Though not fully implemented, it has a structure for user authentication
     - Consistent timestamps: Uses a standardized time format and timezone

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?
   - The secure version uses the **Observer Pattern** through middleware and logging mechanisms. The application observes all activities and records them to a persistent log file using a write stream. This pattern works by:
     - Creating a middleware that intercepts all requests
     - Recording details about each request (time, method, URL, IP)
     - Writing this information to a persistent log file
     - Additionally logging specific actions like message sending and error occurrences
   - This creates a reliable audit trail that prevents repudiation by maintaining evidence of all user actions.