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
2. Briefly explain how a malicious attacker can exploit them.
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?




1. 

The insecure.ts file contains a serious privilege escalation vulnerability in the /update-role endpoint. The key issue is that the endpoint validates authorization based on the role of the user being modified rather than the role of the authenticated user making the request. This means the endpoint checks if the target user is an admin instead of verifying that the requesting user has admin privileges.

2. 

A malicious attacker can exploit this vulnerability by sending a POST request to /update-role with a userId parameter set to an admin user's ID (e.g., 1). Since the code checks if the user with that ID is an admin (which they are), the authorization check will pass even if the attacker isn't actually authenticated as that admin. The attacker can then set the newRole parameter to "admin" for any user ID they control or want to modify, effectively gaining admin privileges or modifying other users' roles without proper authorization.

3. 

The secure.ts implements several defensive techniques to prevent privilege escalation:

(1). Session-based authentication: It uses express-session to maintain user sessions, requiring users to be logged in before performing sensitive operations.

(2). Proper authorization logic: It verifies that the currently logged-in user (identified by req.session.userId) is an admin before allowing role changes, rather than checking the role of the target user.

(3). Clear separation between authentication and authorization: The code first checks if the user is logged in at all (authentication) before verifying their permission level (authorization).

(4). Enhanced cookie security: It sets the httpOnly and sameSite flags on cookies to protect against XSS and CSRF attacks that could otherwise be used to hijack the session.

These measures ensure that only properly authenticated users with appropriate admin privileges can update user roles, preventing unauthorized privilege escalation.