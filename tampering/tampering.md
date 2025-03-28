# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

    `npm install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

    ```
        <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
    ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
   - The main vulnerability in **insecure.ts** is Cross-Site Scripting (XSS) due to lack of input sanitization. When a user submits a name through the form, the application directly stores the raw input in the session and renders it back in the HTML without escaping special characters. This allows malicious users to inject JavaScript code that will be executed in other users' browsers.

2. Briefly explain how a malicious attacker can exploit them.
   - An attacker can exploit this vulnerability by submitting JavaScript code in the name field, as shown in the example. When this script is rendered back in the browser, it executes and can:
     - Modify the page content (as demonstrated)
     - Steal cookies or session information
     - Redirect users to malicious websites
     - Perform actions on behalf of the victim
     - Log keystrokes or steal sensitive information

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?
   - **secure.ts** implements proper input sanitization through the `escapeHTML()` function. This function replaces potentially dangerous characters like `<`, `>`, `"`, `'`, and `&` with their HTML entity equivalents. When user input is sanitized this way in the `register` route, any script tags or HTML injected by an attacker are rendered as plain text rather than being interpreted as executable code by the browser, effectively preventing XSS attacks.