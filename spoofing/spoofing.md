# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out? 


## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.
   - The spoofing vulnerability in **insecure.ts** stems from the fact that the session cookies are not set with the `httpOnly` attribute (it's explicitly set to `false`). This allows client-side scripts to access the cookies, making them vulnerable to theft through XSS attacks.
   - The session secret is also hardcoded ("SOMESECRET") in the application code rather than being passed as an environment variable or command-line argument, making it easier for attackers to forge session cookies if they gain access to the code.

2. Briefly explain different ways in which vulnerability can be exploited.
   - The vulnerability can be exploited in two main ways:
     1. Stealing cookies programmatically: Since `httpOnly` is set to `false`, malicious scripts can access the session cookie using JavaScript's `document.cookie` and send it to an attacker-controlled server, allowing the attacker to impersonate the user.
     2. Cross-site request forgery (CSRF): Without proper CSRF protection or `sameSite` cookie attributes, attackers can trick users into making unwanted requests to the server from a different site, potentially performing sensitive operations if the user is authenticated.

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.
   - **secure.ts** mitigates the spoofing vulnerability by:
     1. Setting the `httpOnly` attribute to `true`, preventing client-side scripts from accessing the cookies
     2. Enabling the `sameSite` attribute, which helps prevent CSRF attacks by ensuring cookies are only sent with same-site requests
     3. Taking the session secret as a command-line argument (`process.argv[2]`) rather than hardcoding it, improving security by keeping the secret out of the source code