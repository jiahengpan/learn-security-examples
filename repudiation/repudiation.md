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
2. Briefly explain why the vulnerability is addressed in __secure.ts__.
3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?




1. 

The application has a vulnerability in the GET request to '/get-messages' endpoint which allows anyone to retrieve all messages without any authentication or authorization checks. This is an example of Insecure Direct Object Reference (IDOR) or Missing Function Level Access Control vulnerability. Any user can access potentially sensitive message data that should be restricted.

2. 

In the secure version, the '/get-messages' endpoint likely implements proper authentication and authorization checks before allowing access to the messages. It would verify that the requesting user has permission to view the messages, possibly by checking session tokens, user roles, or other authentication mechanisms.

3. 

The secure version likely implements the Intercepting Filter Pattern (or Middleware Pattern). This pattern uses authentication middleware that intercepts incoming requests before they reach the route handler. The middleware verifies the user's identity and permissions, and only allows the request to proceed if the user is authorized to access the resource. If not authorized, the middleware rejects the request with an appropriate error response. This creates a separation of concerns where authentication logic is handled separately from the business logic, making the system more secure and maintainable.