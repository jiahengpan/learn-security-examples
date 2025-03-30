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
2. Briefly explain how a malicious attacker can exploit them.
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?



1. 

The insecure.ts file contains an information disclosure vulnerability in the /userinfo endpoint. The main issue is that it directly uses unsanitized user input (username) in the MongoDB query without any validation or sanitization. Additionally, it sends the entire user object back in the response, which includes sensitive information like the password hash. The code also lacks proper error handling, potentially exposing implementation details if an error occurs.

2. 

A malicious attacker can exploit this vulnerability using NoSQL injection techniques. For example, they could use a query parameter like `username[$ne]=anything` which translates to "where username is not equal to 'anything'" - a condition that matches all users in the database. This could result in the application returning all user information, including passwords, to the attacker. Additionally, other NoSQL operators like $regex, $where, or $gt could be used to craft attacks that extract specific information or bypass authentication mechanisms.

3. 

The secure.ts implements several defensive techniques to prevent information disclosure:

(1). Input validation: It verifies that the username parameter is a string before processing it.

(2). Input sanitization: It removes non-alphanumeric characters from the username using a regular expression replacement (`username.replace(/[^\w\s]/gi, '')`), which prevents NoSQL injection attacks by stripping special characters that could be used to manipulate the query.

(3). Proper error handling: It implements try-catch blocks to handle errors gracefully without exposing implementation details.

(4). Clear user feedback: It provides appropriate HTTP status codes and messages for different types of errors (400 for bad input, 401 for invalid username, 500 for server errors).

These measures collectively ensure that user input cannot be manipulated to expose sensitive information, and that the application responds securely to all types of requests.