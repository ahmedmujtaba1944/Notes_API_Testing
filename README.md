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

- Verify if APIs return correct data ?
- is the responce time acceptable ?
- Are the status codes correct ?
- does the API handle invalid input properly ?
- Check error handling
- Test performance (response time)
- Automate validations

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


### API Return 200 OK but functionality is not working. is it failure ?
- Even if an API returns 200 OK, i will mark it as failue if the business login is not working correctly, Because API testing is not just about status codes, its aobut validating whether the system delivers the expected outcomes.
### Get API changes data in the database. is it correct behavior ?
- If a GET API is changing database data, i would treat it as a design defet, unless its limited to non-critical side effects like logging, Idealy data modification should be handled by POST, PUT or PATCH APIs.
### API Return 204 No Content. is it a failure ?
- 2-4 no Content is a successful repsonse. i woudl not treat it as a failue unless the requirment expects a responce body. As a tester, i validate it against the business requirment before marking it passo or fail.

### API works without authentication is it acceptable ?
- IF a API works withtou authentication, i evaluate it based on its purpose. For public APIs it may be acceptable, but for protected or sensitive operations, its a critical security issue and should be reported as defect. 

### DELETE API returns success, but data still exists. What is your conclusion?
- I would mark this as a defect. A successful DELETE response implies the resource has been removed, so if the data is still retrievable afterward (via GET), the API is not fulfilling its contract. I'd verify with a follow-up GET call and log it with both requests/responses as evidence.

### PUT and PATCH behave the same way. Is that acceptable?
- No, this is a design/implementation issue. PUT should replace the entire resource, while PATCH should apply a partial update. If both behave identically (e.g., PATCH also overwrites untouched fields), it's a functional defect because it can cause unintended data loss for consumers expecting partial updates.

### You sent an API request but did not receive any response. What will you do?
- First, I'd check my own setup — network connectivity, correct endpoint/environment, and whether the request timed out. Then I'd check server/API logs if accessible, verify with the dev team if the service is up, and retest. If it's reproducible, I'd log it as a defect with timeout duration, request details, and environment info.

### API returns 404 Not Found error. What does it indicate?
- It indicates the requested resource or endpoint doesn't exist at that URL. I'd check whether it's an incorrect endpoint on my end (typo, wrong path parameter, wrong environment/base URL) versus a genuine case where the resource was deleted or never existed — those are two very different root causes.

### GET request works but POST request fails. What could be the reason?
- Common causes include: missing/incorrect request body or headers (like Content-Type), missing authentication/CSRF token required specifically for write operations, incorrect HTTP method permissions, or a validation rule failing on the payload. I'd compare headers and payload against API documentation to isolate the cause.
### A mandatory field is missing in the request. What response do you expect?
- I'd expect a 400 Bad Request with a clear, descriptive error message specifying which field is missing. If instead the API returns 200 OK or a 500 error, that's a defect — proper input validation should never crash the server or silently succeed.
### API returns 500 Internal Server Error. What is your approach?
- I'd treat this as a critical defect since it points to an unhandled server-side issue. I'd capture the exact request (payload, headers, endpoint), the timestamp, and share it with backend devs to check server logs/stack trace. I'd also try to reproduce it with different data to see if it's data-specific or consistent.
###  API response time is higher than expected. What will you do?
- I'd first confirm the expected SLA/benchmark from requirements. Then I'd retest under normal conditions to rule out network issues, check if it's consistent across multiple runs, and report it with response time metrics if it consistently exceeds the threshold — flagging it for performance/load testing if needed.
### What is negative testing in Postman?
- Negative testing means sending invalid, unexpected, or malformed input to verify the API fails gracefully — e.g., missing fields, wrong data types, invalid tokens, boundary values, SQL injection strings, or oversized payloads — and checking that it returns proper error codes/messages instead of crashing or exposing data.
### API works in Postman but fails when called from the application. Why?
- Common reasons: different headers being sent by the app (auth tokens, content-type), CORS restrictions, different environment/base URLs, request payload formatted differently by the app's code, session/cookie handling differences, or the app hitting a different API version. I'd compare the raw request from both sources using browser dev tools or logging.
### What details should be included while reporting an API defect?
- Endpoint URL and HTTP method, request headers and body, actual vs expected response (status code + body), environment (Dev/QA/Prod), timestamp, steps to reproduce, and screenshots or Postman collection export if possible. This ensures devs can reproduce it without back-and-forth.
### How do you execute multiple APls together in Postman?
- Using Postman Collections with Collection Runner, or the Newman CLI for automated/scheduled runs. I also chain APIs by using scripts (pm.environment.set) in the "Tests" tab to pass data like tokens or IDs from one request to the next.
### API response contains multiple records. How will you validate the response?
- I'd validate the schema of each record, check the total count matches expectations (e.g., against DB or a count endpoint), verify pagination if present, check for duplicate or missing records, and confirm sorting/filtering logic if applicable — often automated using Postman test scripts with loops.
### You need to test the same API in Dev, QA, and Prod. How will you manage this?
- I'd use Postman Environments with variables (base URL, tokens, credentials) specific to each stage, so the same collection runs across all environments just by switching the environment selector — avoiding hardcoded URLs.
### How do you test an invalid login scenario usingPostman?
- I'd send requests with wrong username/password combinations, empty credentials, SQL injection strings, expired/invalid tokens, and case-sensitivity variations — then verify the API returns proper 401/403 errors with no sensitive data leaked (like whether it's the username or password that's wrong).

### What's the difference between authentication and authorization in API testing?
- Authentication verifies who is making the request (e.g., login credentials, API key, token), while authorization determines what that authenticated user is allowed to do (e.g., access permissions, roles). In testing, I check authentication by trying invalid/missing credentials, and I check authorization by trying valid credentials but attempting actions outside that user's role (like a normal user hitting an admin-only endpoint) — it should return 401 for auth failures and 403 for authorization failures.
### What is the difference between PUT and POST?
- POST is used to create a new resource — calling it multiple times with the same data typically creates multiple new records (not idempotent). PUT is used to update or replace an existing resource — calling it multiple times with the same data results in the same final state (idempotent). As a tester, I verify this idempotent behavior specifically for PUT by sending the same request repeatedly and confirming the result doesn't change after the first call.
### How do you handle dynamic values (like tokens or timestamps) in Postman tests?
- I use Postman's environment/global variables along with scripting. For example, after a login API call, I extract the token from the response using a script in the "Tests" tab (pm.environment.set("token", jsonData.token)) and reference it as {{token}} in subsequent requests' headers. For timestamps, I generate dynamic values using pre-request scripts (e.g., Date.now()) so tests remain valid regardless of when they run.
### What is rate limiting, and how would you test it?
- Rate limiting restricts how many requests a client can make within a given time window, to prevent abuse or overload. I test it by sending repeated requests in quick succession (manually or via Collection Runner/Newman) and verifying the API returns a 429 Too Many Requests once the limit is crossed, along with proper headers like Retry-After or rate-limit remaining counts.
### How do you test file upload APIs?
- I test with valid file types/sizes first to confirm successful upload, then negative cases: invalid file formats, oversized files, empty files, missing file fields, and corrupted files. I also check the response for correct metadata (file name, size, URL) and verify the file is actually retrievable/stored correctly afterward, not just that the API returned success.
### What's the difference between REST and SOAP APIs?
- REST is an architectural style using HTTP methods (GET, POST, PUT, DELETE) with lightweight formats like JSON, making it faster and easier to test with tools like Postman. SOAP is a stricter protocol using XML only, with built-in standards for security and transactions, but it's heavier and requires WSDL files to understand the contract. In practice, most modern APIs I test are REST-based since they're simpler and more widely adopted.
### How would you test an API for SQL injection or XSS vulnerabilities?
- For SQL injection, I send inputs like ' OR '1'='1 or '; DROP TABLE users;-- in text fields and verify the API sanitizes/rejects them rather than executing them or throwing a raw database error. For XSS, I send script tags like <script>alert(1)</script> in input fields and check that the response properly escapes/encodes it rather than reflecting it back as executable content. Any raw execution or database error leakage is a critical security defect.
### What is idempotency, and which HTTP methods are idempotent?
- Idempotency means making the same request multiple times produces the same result as making it once, with no additional side effects. GET, PUT, DELETE, HEAD, and OPTIONS are idempotent — calling DELETE twice on the same resource should leave the system in the same state (deleted) both times. POST and PATCH are generally NOT idempotent, since repeated calls can create duplicates or apply changes multiple times.
### How do you handle API versioning during testing?
- I check how the API implements versioning — typically via URL path (/v1/, /v2/), query parameter, or custom header (Accept-Version). I test that older versions still work as expected (backward compatibility) if they're meant to be supported, and that new versions introduce the intended changes without breaking existing consumers. I also verify proper error handling when an unsupported/deprecated version is requested.
