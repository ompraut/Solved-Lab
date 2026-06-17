# Path Traversal Lab

## Vulnerability
Path Traversal

## Objective
Access files outside the intended directory.

## Tools Used
- Burp Suite
- Browser

## Payload
../../../etc/passwd

## Steps
1. Open the lab.
2. Intercept the request in Burp Suite.
3. Find the vulnerable parameter.
4. Modify the parameter using a path traversal payload.
5. Send the request.
6. Observe the response.

## Impact
An attacker may access sensitive files on the server.

## Result
Lab Successfully Solved.
