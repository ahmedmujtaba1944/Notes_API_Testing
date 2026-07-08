## Notes_API_Testing

#### What is an API, really?

**simple real-life example:**

We go to a restaurant. We don’t walk into the kitchen and cook our own food, right? Instead:

* We tell the waiter what we want
* The waiter takes our request to the kitchen
* In the kitchen Cheif prepares our food
* The waiter brings it back to us
* That waiter is exactly like an API.

Analogy:
- API → Waiter
- Our app → Customer
- Server/database → Kitchen

_The API acts as a bridge between our application and the server. It takes our request, sends it to the server, and returns the response._

**In technical terms:**

An API (Application Programming Interface) is a set of rules that allows different software systems to communicate with each other.

**For example:**
- When we check the weather on our phone → API fetches data
- When we log in using Google → API handles authentication
- When we scroll social media → APIs load posts
- We don’t see it, but APIs are working behind the scenes everywhere.

**Postman helps us:**

Verify if APIs return correct data ?
is the responce time acceptable ?
Are the status codes correct ?
does the API handle invalid input properly ?
Check error handling
Test performance (response time)
Automate validations

**Key Features of Postman**

- Send API Requests : We can send requests like GET, POST, PUT, DELETE without writing code.
- Collections : Organize our API requests into folders.
- Environments : Switch between development, staging, and production easily.
- Automated Tests : Write simple scripts to validate responses.


**Understanding HTTP Methods**

When working with APIs, we must specify what action we want to perform. This is done using HTTP methods. (HTTP, HTTPS) 
Think of them as verbs.
- GET → Retrieve Data : Used to fetch information from the server. Example “Give me all users”
- POST → Create Data : Used to send new data. Example: “Create a new account”
- PUT → Update Data : Used to modify existing data. Example: “Update my profile”
- Patch → Update Data : Used to modify existing data signle attribute. Example: “Update my name only”
- DELETE → Remove Data : Used to delete something. Example: “Delete this user”

**Understanding the Response**
After sending a request, focus on these three things:
1. **Status Code**
This tells us if the request was successful.

**Common codes:**
* 200 OK → Success
* 201 Created → Data created
* 404 Not Found → Resource missing
* 500 Server Error → Server issue
* 401 Unauthorized → Not logged in
* 403 Forbidden → No permission
As a QA tester, this is the first thing we should check.
2. **Response Body**
This is the actual data returned.

**Example:**
```
{
“userId”: 101,
“id”: 1,
“title”: “sample title”,
“body”: “sample content”
}
```

> This format is called JSON (JavaScript Object Notation).
Think of it like:
Key → “title”
Value → “sample title”
>
3. **Response Time**
Postman shows how long the request took.

**Example:**
Normal → 200ms
Slow → 3000ms
Slow responses can indicate performance issues.

