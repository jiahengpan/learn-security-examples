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
2. Briefly explain how a malicious attacker can exploit them.
3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?




1. 

The insecure.ts has several vulnerabilities that could lead to a Denial of Service (DoS) attack:

First, there is no rate limiting on the /userinfo endpoint, allowing attackers to send an unlimited number of requests in a short time period.

Second, the code lacks proper error handling for database queries. If an invalid ID format is provided, MongoDB might throw an exception, but there's no try-catch block to handle it gracefully.

Third, the endpoint uses unvalidated user input directly in a database query without any sanitization or validation, potentially allowing for resource-intensive queries.

2. 

A malicious attacker could exploit these vulnerabilities by:

Sending a flood of requests to the /userinfo endpoint from multiple sources, overwhelming the server's capacity to respond.

Submitting specially crafted IDs that cause the MongoDB engine to perform resource-intensive operations, such as regular expressions that trigger catastrophic backtracking.

Sending invalid ID formats that cause unhandled exceptions, potentially crashing the application or causing it to consume excessive resources while handling errors.

Using automation tools to generate thousands of requests per second, preventing legitimate users from accessing the service.

3. 

The secure.ts implements several defensive techniques to prevent DoS attacks:

Rate limiting: It uses the express-rate-limit middleware to restrict each IP address to 1 request per 5 seconds, preventing request flooding.

Error handling: It implements a try-catch block around the database query to gracefully handle any exceptions that might occur, returning appropriate error messages without crashing.

Response timeouts: While not explicitly shown in the code, the proper error handling would prevent long-running operations from tying up server resources indefinitely.

Consistent error responses: The code returns standardized error responses with appropriate HTTP status codes, which helps maintain system stability under attack conditions.

These measures collectively ensure that the application can maintain availability even when under attack, preventing successful DoS exploits.