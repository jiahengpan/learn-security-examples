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
2. Briefly explain different ways in which vulnerability can be exploited.
3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.



1. Spoofing vulnerability in insecure.ts:  
- The session cookie is not secure (no secure flag), allowing interception.  
- No proper admin authentication - just checks if username is "Admin".  
- Session data can be easily manipulated.  

2. Exploitation methods:  
- Cookie theft via MITM (no secure/HttpOnly flags)  
- Registering as "Admin" through normal form  
- Direct cookie manipulation in browser  
- CSRF attacks (no protection tokens)  

3. Why secure.ts is safe:  
- Uses secure cookie settings (secure, HttpOnly flags)  
- Proper admin authentication (password check)  
- CSRF protection tokens  
- Separate admin flag (not just username check)  
- Validates session integrity server-side  

Main difference: secure.ts makes it impossible to just change username or cookie to gain admin access.